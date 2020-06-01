---
title: nodejs, mongodb 익명게시판 만들기
date: 2020-02-11 22:00:00 +0800
categories: [Develop, NodeJs]
tags: [nodejs, mongodb, EZSET]
seo:
  date_modified: 2020-02-12 02:47:23 +0900
---

## 목표
게시판을 만들던 도중 익명게시판도 만들어야겠다는 생각이 들었다.  
게시글을 올리면 작성자가 '익명'으로만 나오고 댓글의 작성자도 '익명'으로 나오도록 만들 생각이다.  



## 문제점들과 해결책
처음에는 단순히 게시판과 게시글의 스키마에 익명게시판인지 확인할 수 있는 flag를 추가해서  
익명게시판이라면 작성자를 '익명'으로 보이게만 하면 된다고 생각했는데 구현을 하고보니 문제점이 생겼다.  

일단 단순히 프론트에서 보이는 내용만 '익명'으로 처리한다면 생기는 문제점은  
서버의 운영자가 DB를 열어보면 누가 작성했는지 알 수 있기 때문에 제대로된 익명게시판이 아니다.  
그래서 생각한 해결 방법은 nodejs의 내장함수인 crypto를 사용해서 익명게시판이라면 글을 작성할 때  
작성자의 이름을 암호화 시켰다. (crypto의 사용방법은 나중에 포스팅하기로)

그렇게 암호화를 하고나니 또 다른 문제점이 생겼다.  
내가 만든 게시판에는 삭제와 수정기능이 있는데 그 기능들을 만들 때  
작성자와 현재 로그인한 사용자가 다르다면 수정과 삭제버튼이 보이지 않게 만들었다.  
하지만 작성자의 이름을 암호화시켜버렸기 때문에 원래의 방법으로는 수정과 삭제가 불가능했다.  
이 문제는 간단하게 게시글이 익명게시글이라면 현재 로그인한 사용자와 게시글의 작성자가 일치하는지 판단할 때  
현재 로그인한 사용자도 다시 암호화를 시켜 암호화한 두 값이 같은지 판단해줬다.
같은 내용을 암호화한 값은 결과값이 같기 때문이다.



## 코드
게시판과 게시글 스키마에  
```javascript
isAnonymous: {
    type: Boolean,
    default: false,
},
```
이런식으로 해당 게시판과 게시글이 익명인지 아닌지 boolean형식으로 표시한다.  



`router.post()`안에 해당게시판이라면 작성자의 이름을 암호화해서 저장하는 코드이다.
```javascript
let post
if (board.isAnonymous == false) {
    post = new Post({
        board: boardId,
        title: req.body.title,
        content: req.body.content,
        author: req.user.username,
        isAnonymous: false,
        created_date: Date.now(),
    })
} else {
    post = new Post({
        board: boardId,
        title: req.body.title,
        content: req.body.content,
        author: crypto
            .createHash('sha512')
            .update(req.user.username)
            .digest('base64'),
        isAnonymous: true,
        created_date: Date.now(),
    })
}
let newpost = await post.save()
```


`router.delete()`에서 익명게시판이라면 현재 로그인한 사용자를 암호화해서 서로 비교하는 코드이다.  
현재 로그인한 사용자는 `req.user.username`으로 받아올 수 있게 했다.  
```javascript
let post = await Post.findById(req.params.post_id)
if (post) {
    if (post.isAnonymous == false) {
        if (post.author != req.user.username) {
            res.status(403).end()
            return
        }
    } else {
        if (
            post.author !=
            crypto
                .createHash('sha512')
                .update(req.user.username)
                .digest('base64')
        ) {
            res.status(403).end()
            return
        }
    }
    await post.delete()
    res.status(200).json({ message: 'post deleted', target: post })
}
```
게시글 수정도 같은 방식으로 코딩했다.  
또한 프론트에서도 위와같은 방식으로 게시글의 삭제와 수정버튼을 보이게 할지 숨길지 판단했다.  



## 전체 코드
[백엔드](https://github.com/Tekiter/EZSET/blob/master/backend/src/api/v1/simple.route.js)  
[프론트엔드](https://github.com/Tekiter/EZSET/blob/master/frontend/src/views/Board/Content.vue)