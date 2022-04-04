---
title : "List와 ArrayList의 차이"
categories:
 - Algorithm
tags:
 - [ algorithm ] 

toc: true
toc_sticky: true

date: 2022-04-04
last_modified_at: 2022-04-04

---

<h3>List와 ArrayList의 차이</h3>

List는 인터페이스고 ArrayList는 클래스이다.

- 클래스는 일반클래스와 클래스 내에 추상메서드가 하나 이상있건, abstract로 정의된 추상 클래스로 나뉨
- 인터페이스는 모든 메서드가 추상 메서드인 경우를 의미 / 인터페이스를 상속받는 클래스는 인터페이스에서 정의된 추상 메서드를 모두 구현해야함
(따라서 다양한 클래스를 상속받는 특정 인터페이스는 결국 동일한 메서드를 제공함)


하나의 예를 보자.

~~~java
List<Integer> list1 = new ArrayList<Integer>();  // list1을 ArrayList객체로 만듦
list1 = new LinkedList<Integer>(); // 그리고 이렇게 LinkedList 객체로도 대체가 가능함
~~~

이유는 List<Integer>가 인터페이스인데, 이런 인터페이스의 추상메소드를 구현하는게 ArrayList, LinkedList 둘 다 가능하기 때문이다.

<br><br>

예제
~~~java
ArrayList<Integer> list1 = new ArrayList<Integer>();
~~~

- list1에 아이템 추가시에는 add를 사용 : list.add(1),list1.add(2);
- list1의 특정 아이템을 읽을시 get(인덱스번호)을 사용 : list1.get (0);
    ⇒ 1 
- list1의 특정 인덱스에 해당하는 아이템 변경시 set을 사용 : list1.set(0,5);
    ⇒ 1이었던 값이 5로 바뀜
- list1의 특정 인덱스에 해당하는 아이템 삭제시 remove를 사용 : list1.remove(0);
    ⇒ 2(0번째의 인덱스값이 사라지고 1번째에 있던 값이 0의 자리로 옴)
- list1의 배열 길이 확인은 : list1.size();
    ⇒ 1

<br><br>

3차원 배열 출력 연습

~~~java
public class Test {
    public static void main(String[] args) {
      Integer [][][] data_list = {
        {
            {1,2,3},
            {4,5,6}
        },
        {
            {7,8,9},
            {10,11,12}
        }
      };
      //3차원 배열에서 8, 10, 2를 순서대로 출력해보기
      System.out.println(data_list[1][0][1]);
      System.out.println(data_list[1][1][0]);
      System.out.println(data_list[0][0][1]);
    }

}
~~~

<br><br>

- 배열.length : 배열에 들어있는 아이템 개수
- 문자열.indexOf(String key) : String key가 해당 문자열에 있으면 해당 문자의 위치(index값)를 리턴하고, 없으면 -1을 리턴함

~~~java
public class Test {
    public static void main(String[] args) {

        String str = "HelloSpring";
        System.out.println(str.indexOf("S"));
        //"S"의 index 위치는 5번째이므로 값은 5가 된다.
    }
}
~~~