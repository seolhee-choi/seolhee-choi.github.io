---
title : "스택(Stack)의 구조"
categories:
 - Algorithm
tags:
 - [ algorithm ] 

toc: true
toc_sticky: true

date: 2022-04-06
last_modified_at: 2022-04-06
---

<h3>스택(Stack)</h3>

스택은 LIFO(Last In, First Out) 또는 FILO(First In, Last Out) 데이터 관리 방식을 따름<br><br>

- LIFO : 마지막에 넣은 데이터를 가장 먼저 추출함
- FILO : 처음에 넣은 데이터를 가장 마지막에 추출함
- 데이터를 스택에 넣는건 push() / 데이터를 스택에서 꺼내는건 pop()
<br><br>
💡큐(Queue)가 FIFO 라면, 스택(Stack)은 LIFO
<br><br><br><br>
🤩대표적인 장점
- 구조의 단순화/ 데이터의 저장,읽기 속도 빠름<br>
<br>
😥대표적인 단점
- 데이터 최대 개수를 미리 정해야 하고, 미리 정한만큼 쓰지 못하면 저장공간의 낭비 발생
<br><br><br><br><br>
push와 pop을 이용한 예제코드

```java
import java.util.Stack;

public class Test_stack {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<Integer>();

        stack.push(1); //1이 추가됨
        stack.push(2); //2가 추가됨
        stack.push(3); //3이 추가됨

        stack.pop(); //3이 제거됨
        stack.pop(); //2가 제거됨
        System.out.println(stack);
    }
}
```
<br><br><br><br>
ArrayList를 이용해서 가변적으로 Stack(스택)에 데이터를 넣고 빼보자


```java
import java.util.ArrayList;

public class Test_stack<T> {
   private ArrayList<T> stack = new ArrayList<T>();

   public void push(T i) { 
       stack.add(i); //add는 ArrayList의 객체
   }

   public T pop() {
      if(stack.isEmpty()) {
         return null;
      }
      //stack의 사이즈를 알 수 없기때문에 size 메소드로 값을 구한 뒤 -1을 해주면 끝자리 값이 삭제됨
      return stack.remove(stack.size() - 1);
   }

   public boolean isEmpty() {
      return stack.isEmpty();
   }

   public static void main(String[] args) {
      Test_stack<Integer> ts = new Test_stack<Integer>();
      ts.push(1); //1을 넣고
      ts.push(2); //2를 넣고
      System.out.println(ts.pop()); //2가 출력되고 값이 빠짐
      ts.push(3); //3을 넣고
      System.out.println(ts.pop()); //3이 출력되고 값이 빠짐
      System.out.println(ts.pop()); //남은 값은 1로, 1이 출력됨
	//2 3 1 순으로 출력된다.
   }
}
```
