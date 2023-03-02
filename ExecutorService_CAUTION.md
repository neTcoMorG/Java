# ExecutorService.newFixedThreadPool(int) 주의점

newFixedThreadPool(int N)는 N개의 Thread를 생성하여 **Thread pool**를 생성해주는 객체이다. 하지만 이 부분을 알지 못하면 **심각한 메모리 낭비와 프로그램이 죽는** 경험이 발생할 수 있다.



## Thread pool ?

![asd](https://user-images.githubusercontent.com/24227385/222470649-75aa9910-fa78-40c6-93a4-266fc9ddbbda.jpg)

- 사전에 원하는 만큼 N개의 `Thread`를 만들어두고 담아두는 바구니다.



N개의 요청에 `thread`를 1:1로 처리한다면 Thread 생성에 대한 비용, 너무 많은 요청에 의해 Thread가 많아지면 Context switch의 빈도가 높아 오버헤드가 발생하여 미리 Thread를 만들어두고 요청이 오면 대기중인 Thread를 할당해 처리하고 처리가 끝나면 다시 재사용 함으로써 오버헤드를 줄인다.



## Thread pool의 구성

1. Queue

   - 모든 스레드가 사용중이면 그 뒤의 요청을 담기 위한 큐이다.

     

2. pool

   - 만들어둔 Thread이 담겨져 있는 바구니다.



## 자바에서의 Thread pool 생성과 문제

`ExecutorService.newFixedThreadPool(int)` 사용하여 N개의 스레드를 담은 pool을 쉽게 생성할 수 있다.

하지만 주의해야한다.

![a1](https://user-images.githubusercontent.com/24227385/222470684-c7b8ca47-413d-4386-8964-609956b5e3b0.png)

newFixedThreadPool은 얼마큼의 스레드를 생성할지 파라미터로 전달 받아 Thread pool을 생성한다.
생성과 동시에 `new LinkedBlockingQueue` 객체를 생성해 스레드를 기다리는 요청의 큐를 생성한다.

![a2](https://user-images.githubusercontent.com/24227385/222471080-8ebdab76-8b50-4d69-9e61-7161ae541beb.png)

하지만 LinkedBlockingQueue의 생성자를 보면 Queue 사이즈를 int 자료형의 크기만큼 할당받는다.

![a3](https://user-images.githubusercontent.com/24227385/222471310-81095450-3937-49dc-9ac6-777894df48ac.png)

Queue의 사이즈를 지정해주지 않고 int 자료형의 최대 크기만큼 할당 받기에 너무 많은 요청이 들어오면 메모리 낭비가 발생하며
프로그램이 죽을 수 있다.



