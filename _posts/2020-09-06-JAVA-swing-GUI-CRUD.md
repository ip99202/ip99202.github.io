---
title: JAVA swing을 이용한 GUI CRUD 만들기
date: 2020-09-06 01:30:00 +0800
categories: [Develop, JAVA]
tags: [java]
---

# 회원관리 프로그램
내가 만들 것은 회원정보를 관리하는 프로그램이다.  
회원을 추가 삭제 수정할 수 있고  
회원목록 조회와 이름 검색까지 만들어 볼 것이다.  
<br>

## Model
우선 DTO에는 회원의 이름, 생년월일, 전화번호를 넣을 것이다.  
편의를 위해 모두 스트링을 사용했다.  
```java
public class Model {
    String name;
    String birth;
    String tel;

    public Model(String name, String birth, String tel) {
        this.birth = birth;
        this.name = name;
        this.tel = tel;
    }
}
```  
<br>

## Controller
DAO 역할을 하는 Controller에는 db와 연결하는 생성자를 만들어두었다.  
```java
public Controller() {
        try {
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3305/memberinfo?serverTimezone=UTC", "root",
                    "비밀번호");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```  
<br>

그리고 아래와 같이 각 기능을 함수로 만들어주었다.  
```java
// 회원 추가
public void insertMember(Model model) {
    try {
        st = conn.createStatement();
        int stmt = st.executeUpdate(
                "insert into member values ('" + model.name + "', '" + model.birth + "', '" + model.tel + "');");
    } catch (SQLException e) {
        e.printStackTrace();
    } finally {
        try {
            st.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
<br>

## View, Main
View에는 gui를 구성했다.  
swing을 이용해 버튼과 입력상자, 출력 등을 만들고  
각 버튼을 Controller에 만들어놓은 함수와 연결시켰다.  

Main 클래스는 단순히 프로그램을 실행시키는 기능으로 사용했다.

<br>

## 실행화면
<img width="400px" src="https://user-images.githubusercontent.com/52627952/92304639-c36ede80-efba-11ea-9b71-afdaacea5047.gif">  
<br><br>

## Model 전체코드
```java
package memberInfo;

public class Model {
    String name;
    String birth;
    String tel;

    public Model(String name, String birth, String tel) {
        this.birth = birth;
        this.name = name;
        this.tel = tel;
    }

    public String getName() {
        return name;
    }

    public String getBirth() {
        return birth;
    }

    public String getTel() {
        return tel;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setBirth(String birth) {
        this.birth = birth;
    }

    public void setTel(String tel) {
        this.tel = tel;
    }
}
```
<br>

## Controller 전체코드
```java
package memberInfo;

import java.sql.*;
import java.util.ArrayList;

public class Controller {
    Connection conn = null;
    ResultSet rs = null;
    Statement st = null;

    public Controller() {
        try {
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3305/memberinfo?serverTimezone=UTC", "root",
                    "비밀번호");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }


    // 회원 추가
    public void insertMember(Model model) {
        try {
            st = conn.createStatement();
            int stmt = st.executeUpdate(
                    "insert into member values ('" + model.name + "', '" + model.birth + "', '" + model.tel + "');");
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                st.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    // 회원 목록 출력
    public ArrayList<Model> readMember() {
        ArrayList<Model> arr = new ArrayList<Model>();
        System.out.println(arr);
        try {
            st = conn.createStatement();
            rs = st.executeQuery("select * from member;");
            while (rs.next()) {
                arr.add(new Model(rs.getString(1), rs.getString(2), rs.getString(3)));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                st.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return arr;
    }

    // 회원수정
    public void updateMember(String name, String tel) {
        try {
            st = conn.createStatement();
            int stmt = st
                    .executeUpdate("update member set tel = '" + tel + "' where name = '" + name + "';");
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                st.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    // 회원삭제
    public void deleteMember(String name) {
        try {
            st = conn.createStatement();
            int stmt = st.executeUpdate("delete from member where name = '" + name + "';");
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                st.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    // 회원 검색
    public ArrayList<Model> searchMember(String content) {
        ArrayList<Model> arr = new ArrayList<Model>();
        System.out.println(arr);
        try {
            st = conn.createStatement();
            rs = st.executeQuery("select * from member where name like '%" + content + "%';");
            while (rs.next()) {
                arr.add(new Model(rs.getString(1), rs.getString(2), rs.getString(3)));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                st.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return arr;
    }
}
```
<br>

## View 전체코드
```java
package memberInfo;

import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

public class View {
    JFrame jframe = new JFrame();
    JPanel jpanel = new JPanel();
    JTextField t1 = new JTextField();
    JTextField t2 = new JTextField();
    JTextField t3 = new JTextField();
    JTextField t4 = new JTextField();
    JTextArea ta = new JTextArea();
    JButton btn1, btn2, btn3, btn4, btn5;
    JLabel ㅣ1 = new JLabel("이름 : ");
    JLabel ㅣ2 = new JLabel("생년월일 : ");
    JLabel ㅣ3 = new JLabel("전화번호 : ");
    JLabel ㅣ4 = new JLabel("검색내용 : ");

    View() {
        GUI_init();
    }

    public void GUI_init() {
        jframe.setTitle("회원관리");
        jframe.setBounds(50, 50, 480, 450);
        jframe.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jframe.setVisible(true);
        jpanel.setLayout(null);
        jframe.add(jpanel);

        t1.setBounds(75, 25, 70, 25);
        jpanel.add(t1);
        ㅣ1.setBounds(37, 21, 70, 30);
        jpanel.add(ㅣ1);

        t2.setBounds(213, 25, 70, 25);
        jpanel.add(t2);
        ㅣ2.setBounds(150, 21, 70, 30);
        jpanel.add(ㅣ2);

        t3.setBounds(352, 25, 80, 25);
        jpanel.add(t3);
        ㅣ3.setBounds(290, 21, 70, 30);
        jpanel.add(ㅣ3);

        t4.setBounds(213, 105, 80, 25);
        jpanel.add(t4);
        ㅣ4.setBounds(150, 100, 70, 30);
        jpanel.add(ㅣ4);

        JScrollPane jsp = new JScrollPane(ta);
        jsp.setBounds(23, 145, 420, 250);
        jpanel.add(jsp);

        jpanel.add(btn1 = new JButton("입력"));
        btn1.setBounds(40, 60, 80, 30);

        jpanel.add(btn2 = new JButton("출력"));
        btn2.setBounds(145, 60, 80, 30);

        jpanel.add(btn3 = new JButton("수정"));
        btn3.setBounds(250, 60, 80, 30);

        jpanel.add(btn4 = new JButton("삭제"));
        btn4.setBounds(350, 60, 80, 30);

        jpanel.add(btn5 = new JButton("검색"));
        btn5.setBounds(300, 100, 80, 30);

        Controller dao = new Controller();


        // 회원 추가
        btn1.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent arg0) {
                ta.setText("");

                String name = t1.getText();
                String birth = t2.getText();
                String tel = t3.getText();

                dao.insertMember(new Model(name, birth, tel));

                ta.append("입력 완료 \n");

                t1.setText("");
                t2.setText("");
                t3.setText("");
                t4.setText("");
            }
        });

        // 회원 목록 출력
        btn2.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent arg0) {
                ta.setText("");
                ArrayList<Model> arr = new ArrayList<Model>();
                arr = dao.readMember();

                ta.append("\t" + "name" + "\t" + "birth" + "\t" + "tel\n");
                ta.append("\t" + "------------------------------------------------------------\n");

                for (int i = 0; i < arr.size(); i++) {
                    ta.append("\t" + arr.get(i).getName() + " \t " + arr.get(i).getBirth() + " \t " + arr.get(i).getTel()
                            + "\n");
                }
            }
        });

        // 회원 수정
        btn3.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent arg0) {
                ta.setText("");

                String name = t1.getText();
                String birth = t2.getText();
                String tel = t3.getText();

                dao.updateMember(name, tel);
                ta.append("수정 완료 \n");

                t1.setText("");
                t2.setText("");
                t3.setText("");
                t4.setText("");
            }
        });

        // 회원 삭제
        btn4.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent arg0) {
                ta.setText("");

                String name = t1.getText();
                dao.deleteMember(name);
                ta.append("삭제 완료 \n");

                t1.setText("");
                t2.setText("");
                t3.setText("");
                t4.setText("");
            }
        });

        // 회원 검색
        btn5.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent arg0) {
                ta.setText("");
                String content = t4.getText();

                ArrayList<Model> arr = new ArrayList<Model>();
                arr = dao.searchMember(content);
                ta.append(" \n");

                ta.append("\t" + "name" + "\t" + "birth" + "\t" + "tel\n");
                ta.append("\t" + "------------------------------------------------------------\n");

                for (int i = 0; i < arr.size(); i++) {
                    ta.append("\t" + arr.get(i).getName() + " \t " + arr.get(i).getBirth() + " \t " + arr.get(i).getTel()
                            + "\n");
                }

                t1.setText("");
                t2.setText("");
                t3.setText("");
                t4.setText("");
            }
        });
    }
}
```
<br>

## Main 전체코드
```java
package memberInfo;

public class Main {
    public static void main(String[] args) {
        View gui = new View();
    }
}
```