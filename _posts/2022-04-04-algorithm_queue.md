---
title : "큐(Queue)의 구조"
categories:
 - Algorithm
tags:
 - [ algorithm ] 

toc: true
toc_sticky: true

date: 2022-04-04
last_modified_at: 2022-04-04

---


<h3>큐 구조</h3>

- 가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 구조
- FIFO(First in First out) / LILO(Last in Last out)방식으로 이루어짐
- 큐에는 Enqueue(add(value), offer(value))와 Dequeue(poll(), remove()) 종류가 있음

Queue를 사용하기 위해서는 LinkedList 클래스를 사용해야하고,<br>
어떤 데이터타입을 사용할건지 자료형 매개변수를 넣어서 지정해줘야함

```java
import java.util.LinkedList;
import java.util.Queue;

public class Test_queue {
    public static void main(String[] args) {

        Queue<Integer> q_int = new LinkedList<Integer>();
        Queue<String> q_str = new LinkedList<String>();
    }
}
```
<br><br>
💡Enqueue와 Dequeue

- 큐에 데이터를 추가하기 위해서는 add(value) , offer(value)를 사용<br>
    : q_int.add(1),q_int.offer(2);
- 큐에 들어있는 데이터를 출력할 때는 해당 인스턴스를 사용<br>
    : `System.out.println(q_int);` ⇒ [1, 2]<br>
- 큐에 들어있는 첫 번째 값을 반환하고, 해당 첫 번째 값을 삭제할때는 poll이나 remove를 사용<br>
    : q_int.poll(); , q_int_remove() <br>
    ⇒ 큐에 들어있는 첫 번째 값은 1이므로 1이 삭제되고 2만 남는다.
    

```java
import java.util.LinkedList;
import java.util.Queue;

public class Test_queue {
    public static void main(String[] args) {

        Queue<Integer> q_int = new LinkedList<Integer>();
        Queue<String> q_str = new LinkedList<String>();

        q_int.add(1);
        q_int.offer(2);

        q_str.add("m");
        q_str.offer("om");
        q_str.poll();

        System.out.println(q_str);
    }

}

```
<br><br><br>
📝Enqueue와 Dequeue를 사용해보자

```java
import java.util.ArrayList;

public class Test_queue<T> {
    private ArrayList<T> queue = new ArrayList<T>();

    public void enqueue(T i) {
        queue.add(i);
    }

    //데이터를 꺼내오는거라 별도의 인자가 없음
    public T dequeue() { 
        if (queue.isEmpty()) {
            return null;
        }
        return queue.remove(0);
    }

    //Test_queue 클래스가 데이터를 갖고있는지 여부를 확인하기 위한 메소드
    public boolean isEmpty() { 
        return queue.isEmpty();
    }

    public static void main(String[] args) {
        Test_queue<Integer> tq = new Test_queue<Integer>();
        tq.enqueue(1);  //1을 넣고
        tq.enqueue(2);  //2를 넣고
        tq.enqueue(3);  //3을 넣었다
        System.out.println(tq.dequeue()); //1을 빼고
        System.out.println(tq.dequeue()); //2를 빼고
        System.out.println(tq.dequeue()); //3을 뺌
    }
}

```

값은 순서대로 1 2 3 이 출력된다.