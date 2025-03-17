---
layout: post
title: task 2 - Kiosk / 제출용 회고
date: 2025-03-14 12:00:00 +0900
description: 두번째 과제. Kiosk 제작 # Add post description (optional)
img: # software.jpg # Add image post (optional)
tags: [TIL, Java, Kiosk] # add tag
---

### 회고


시작일 : 25.03.07.\
제출일 : 25.03.14. 14:00


계산기 과제. level 1~5 / 도전 1~2로 나눠져 있다.


어려운 과제는 아닌데, 시간을 좀 제대로 못 썼다.\
주말 그냥 푹 쉬어버리고. 월, 화 이틀 걸쳐서 레벨 5까지 하고.\
수요일은 Maven, Jackson 알아본다고 뻘짓했고\
목요일은 몸살 두통으로 앉아만 있고 뭘 하질 못했다.


다 핑계지. Lv.1 ~ 5 까지도 솔직히 하루면 넉넉했던건데. 집중을 제대로 못했다.\
이번에도 영 아쉽네.

<br>

이번에 한 것 중 이전 과제에서 안했던 거라면
1. Package 나누기
2. Jackson 외부 라이브러리
3. Maven 적용 (진짜 딱 적용만 한 수준이지만)

정도가 아닐까.

그 외는 대부분 복습이고. 람다는 쓰긴 했는데, 지금 보니 Jackson 적용하며 어느 새 지워놨고. General은 또 안 썼네. 왜 이거 쓸 각이 안보이지. 람다 스트림은 forEach나 iterator가 현재 index를 못읽으니까 그게 불편해서 그런가, for문 위주로 쓰게 되고.

이번 과제 해설은 좀 잘 봐야겠네.

<br>

계산기 과제도 제출한 뒤로 안건드렸는데 얘는 다시 건드릴지 모르겠다.\
마음 같아서는 목표치까지 완성해보고 싶긴 한데. 영 집중이 안되어 될지 모르겠고.

<br>
<hr>

[github](https://github.com/le951/task2-kiosk)


### 목표
- 과제 Lv1~5
    - Scanner를 이용한 입출력
    - List Collection을 사용한 Item 관리
    - Menu 클래스와 Kiosk 클래스의 분리
    - 카테고리 부여
    - Getter / Setter와 접근 제한자를 활용한 캡슐화

- 도전 Lv1~2
    - 장바구니 기능
    - 장바구니에 Generic / Stream / Lambda / Enum 활용

- 데이터 저장 / 불러오기

- 웹을 활용한 GUI
    - JSON 입 / 출력
    - 소켓 통신
    - 검증
    - 프론트 구현
    

<hr>

### 달성
- 과제 Lv1~5
    - Scanner를 이용한 입출력
    - List Collection을 사용한 Item 관리
    - Menu 클래스와 Kiosk 클래스의 분리
    - 카테고리 부여
    - Getter / Setter와 접근 제한자를 활용한 캡슐화

- 도전 Lv1~2
    - 장바구니 기능

- JSON 입 / 출력


<hr>

### 이후 목표
- 데이터 저장 / 불러오기

- 웹을 활용한 GUI
    - JSON 입 / 출력
    - 소켓 통신
    - 검증
    - 프론트 구현

- 주석 정리
    
<hr>

### 기록
    list.forEach((name, menuItem) -> {
        out.add("\"" + name + "\" : " + menuItem.toString());
    });

했더니 IDE가

    for (Map.Entry<String, MenuItem> entry : list.entrySet()) {
        String name = entry.getKey();
        MenuItem menuItem = entry.getValue();
        out.add("\"" + name + "\" : " + menuItem.toString());
    }

로 변경할지 물어보던데. 차이가 있나? 위가 더 낫지 않나?

entrySet은 처음 알긴 했네.

ㅡ

도메인 이름 역순 표기법.\
많은 패키지들이 com으로 시작하는 이유.\
대충 짐작은 했는데 몇 년 전의 의문을 확실하게 보니까 꽤나 후련하다.\
아닌가? 당시에는 알았나? 시간 지나며 까먹은건가?

ㅡ

<br>

    public static String getList() throws JsonProcessingException {}

이렇게 함수 자체에 throws를 넣어주는 방법이 있었구나. IDE 추천 보고 알게 되었다.
좀 더 이해할 필요가 있어보인다. 이게 try catch를 필수로 요구하는 방법인가?
함수 위에 @ 뭐시기 하는 걸줄 알았는데.

<br>

### 트러블 슈팅

Maven \
Module (project name) SDK 23 does not support source version 1.5.

Java Compiler 옵션에서 23으로 변경.\
참고한 사이트 : [tistory](https://zion830.tistory.com/114)

ㅡ

artifactId는 공백을 포함하지 않음. - _은 가능.\

작성한 프로젝트 이름인(Task2 - Kiosk)에 공백이 있어서, 이거 때문에 dependency 추가해도 빨갛게 떴네.\
수정하고 IntelliJ 내 Maven 탭에서 reload. 정상화.

ㅡ

    Exception in thread "main" java.lang.ClassCastException: class java.util.LinkedHashMap cannot be cast to class challenge.UI.Item (java.util.LinkedHashMap is in module java.base of loader 'bootstrap'; challenge.UI.Item is in unnamed module of loader 'app')

분명 값을 읽을 때 아래처럼 했는데도     
    list = mapper.readValue(Menu.getList(), HashMap.class);

오류 나서 디버그 돌려보면 대입된 값이 LinkedHashMap 이었다.

짜증나게도 저기서는 경고만 뜨고, 일단 대입까지 되는데 이후에 list.get("ShackBurger").getPrice(); 처럼 호출하게 되면 그제서야 오류가 뜬다. 해서 무슨 오류인가 찾는데에 꽤나 헤메었다.

알아보면 Jackson이 LinkedHashMap으로 처리해서 그렇다고는 하던데, 왜 하라는대로 HashMap을 해도 저렇게 뜨는지는 잘 모르겠다. ReadMe 좀 더 읽어보면 알까.

일단 해결은 했다.
    list = mapper.readValue(Menu.getList(), new TypeReference<HashMap<String, Item>>(){});

ㅡ

No serializer found for class challenge.Menu.MenuItem and no properties discovered to create BeanSerializer

MenuItem의 getter를 default에서 public으로 바꿔줬다. 호출한 곳은 같은 패키지 내인데도 public 아니면 오류가 나네. Jackson 쓰려면 고려할 사항.

<hr>
<br>


