---
title: nodejs, mongodb 게시판 검색 기능
date: 2020-02-07 23:40:00 +0800
categories: [Develop, MongoDB]
tags: [nodejs, mongodb, EZSET]
seo:
  date_modified: 2020-06-01 17:18:48 +0900
---

## 목표
게시판 내의 모든 게시물들에 대해 제목, 내용, 제목 + 내용으로 검색할 수 있게 한다.  
예를 들어 제목이 'helloworld'라는 게시물이 있으면 'wor'을 검색해도 나올 수 있게 하는 것이 목표다.


## MongoDB의 검색기능 사용  
처음 인터넷에서 검색을 해서 찾은 것은 [https://sy34.net/mongodb-full-text-search/](https://sy34.net/mongodb-full-text-search/)  
이 블로그의 내용을 토대로 코딩을 하기 시작했다. 하지만 이 방법은 적용하는것도 복잡하고 동적으로 검색 방법을 바꾸는 것도 힘들어보였다.   
혹시 나중에 이 방법을 쓸 일이 있을 수도 있으니 써본다.

일단 이 방법으로 데이터를 검색하기 위해서는 검색대상의 데이터를 text로 인덱싱해야한다.  
예를 들어 posts 스키마가
```
{
    "title": <글 제목>,
    "content": <글 내용>,
    "author": <글 작성자>,
}
```
이런식으로 만들어져 있을 때
내가 글 제목에 대해 검색하려고 하면
```javascript
db.posts.createIndex({"title":"text"})
```
이런식으로 인덱스를 만들어줘야 검색을 할 수 있게 된다.

위 코드는 mongodb기준의 코드이고  
실제로 내가 express와 mongoose를 통해 구현을 할 때는 posts의 스키마를 구성하는 코드 뒤에다   
```javascript
postSchema.index({ title: 'text' });
```
이런식으로 설정을 해주었더니 인덱스가 적용되었다.

이 과정에서 막힌 것이 2가지가 있었는데 한가지는
```javascript
postSchema.index({ title: 'text' });
postSchema.index({ content: 'text' });
```
이런식으로 두 줄로 쓰면 위에있는 title에 대해서만 인덱스가 생성되는 것이었다.

이 문제는 그냥 두 줄의 코드를 합쳐서 해결했다.
```javascript
postSchema.index({ title: 'text' , content: 'text'});
```
이렇게 작성하면 두개의 값이 인덱싱된다.  
인덱스 된 결과가 두개의 인덱스가 합쳐져있어 잘못된 것인줄 알았는데 잘 실행되었다.

검색은 `find`를 이용해서 `$search: "검색할 문자"` 옵션을 이용하면 된다.
```javascript
posts.find({$text: {$search: "te"}})
```

하지만 이렇게 설정하니 두번째로 막히는 부분이 나왔다.  
내가 애초에 생각했던 방식은 제목, 내용, 제목+내용 3가지의 방식으로 찾는 것이었는데  
인덱스를 설정하는것은 코드를 돌리면서 동적으로 설정했다 풀었다 하기가 힘들어진다는 것이었다.  
dropIndex를 사용해서 인덱스를 풀 수는 있지만  
인덱스를 설정하는 것은 해당하는 데이터를 균형이진트리로 만드는 것 같은데 이 것을 반복하는 것은  
시간복잡도에 상당한 영향을 주는 방법이다.

또한 dropIndex가 코드 상에서 동적으로 작동할 수 있는지도 잘 모르겠다.  
아무튼 이 방법 말고 다른 방법이 있나 찾아보았다.  



## javascript의 정규표현식 이용하기  
찾은 해결 방법은 정규표현식을 사용하는 것이다.  
자바스크립트의 `RegExp()`을 사용하여 정규표현식 객체를 생성하고 그 객체로 `find`를 하면 된다.  

예를들어 제목에 test라는 문자열이 들어가는 게시글을 찾으려면
```javascript
Post.find({ title: new RegExp('test')})
```
이런식으로 코드를 짜면 된다.

앞쪽에서 너무 많이 헤맸는데 생각보다 쉽게 해결되어서 허무했다.

[javascript의 정규표현식에 관한 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp)  
[정규표현식을 테스트할 수 있는 사이트](https://regexr.com/)



## 코드
프론트에서 백엔드로 get을 할 때 query값으로 어떤 검색 조건을 선택했는지와  
어떤 내용으로 검색을 하는지 각각 title과 content라는 이름으로 넘겨주었다.

검색 조건중에 제목과 내용 두가지 다 검색을 해야하는 경우가 있다.  
이 경우를 처리하기 위해서 코드상에 options라는 배열을 만들어  
검색옵션과 정규표현식으로 바꾼 내용을 집어넣은 후 `find()`를 할 때 or옵션을 사용했다.  
`find({ $or: options })` 이런식으로 하면 or를 통해 제목과 내용 두가지 검색 결과를 얻을 수 있다.

```javascript
router.get(
    '/searchpost',
    asyncRoute(async function(req, res) {
        let options = []
        if (req.query.option == 'title') {
            options = [{ title: new RegExp(req.query.content) }]
        } else if (req.query.option == 'content') {
            options = [{ content: new RegExp(req.query.content) }]
        } else if (req.query.option == 'title+content') {
            options = [
                { title: new RegExp(req.query.content) },
                { content: new RegExp(req.query.content) },
            ]
        } else {
            const err = new Error('검색 옵션이 없습니다.')
            err.status = 400
            throw err
        }
        const posts = await Post.find({ $or: options })
```


## 전체 코드

[https://github.com/Tekiter/EZSET/blob/master/backend/src/api/v1/simple.route.js](https://github.com/Tekiter/EZSET/blob/master/backend/src/api/v1/simple.route.js)

