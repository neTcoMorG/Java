# JVM

- Java Virtual Machine의 약자 자바 가상 머신이라고 불림
- Java와 OS의 중계자 역할을 하며 플랫폼에 종속적이지 않음
  <br/>
  <br/>

## JVM의 구성요소

---

1. Class Loader (클래스 로더)
   - `.class` 파일(Byte code)을 묶어서 `JVM`이 운영체제로 부터 할당받은 메모리 영역인 `Runtime Data Area`의 `method Area`에 올립니다.

2. Execution Engine (실행 엔진)
   - JVM은 Method Area에 있는 바이트 코드를 Execution Engine에 옮겨 실행합니다
     한마디로 바이트 코드를 실행시키는 주체입니다. 바이트 코드를 실행시키는 방식으로 `Interpreter` 방식과 `JIT compiler` 방식이 있습니다 `interpreter`는 명령어 한줄식 바이너리 코드로 변환하고 실행하여 속도가 느린반면 `JIT compiler`는 바이트 코드 전체를 바이너리 코드로 변환하여 실행합니다.

3. Runtime Data Area
   - `Runtime Data Area`는 JVM의 메모리 영역으로 자바 프로그램을 실행시키기 위한 데이터들이 위치하는 영역입니다.
   <br/>
![Runtime Data area](https://user-images.githubusercontent.com/24227385/222168381-9fc28ae7-fd10-49ac-829d-2db82d016dad.png)
<br/>

   - `Method Area`와 `Heap Area`는 모든 스레드들이 공유하는 메모리입니다
   - `Stack, PC Register, Native Method Stack`은 스레드별로 독립적인 메모리 입니다.
     <br/>
     <br/>

## Runtime Data Area 구조

___




### **Method Area**

클래스 필드명, 데이터 타입, 접근 제어자, 메소드 정보 데이터 정보, Constant Pool, static 변수, final class 등 클래스와 관련된 정보가 담긴 영역입니다.
<br/>
<br/>

### **Heap Area**

new 키워드로 생성된 데이터 (객체, 배열) 들이 위치한 영역입니다.
주기적으로 GC가 관리하는 영역입니다.



**Heap 구조**

- `Young Generation`

  1. `Eden`, `Survivor1,2`(16MB)

     객체가 최초로 Heap에 할당되는 영역

     만일 Eden영역이 가득 찼다면 **Eden 영역의 객체가 Survivor1 혹은 Survivor2로 옮겨진다**

     *(옮겨지는 객체들은 어딘가에서 참조되고 있는 객체들)*

     `Minor GC`가 발생하는 위치다 **Eden영역 또는 Survivor1 혹은 Survivor2에서 사용되지 않는 객체들을 삭제**한다.

  2. `Old`

     **Survivor1, Survivor2에서 오랫동안 살아남은 객체들은 Old 영역에 위치한다**.

     *(Eden영역에서 바로 Old로 오는 경우도 있는데 이 경우 객체의 크기가 큰 경우다. 20MB)*

     `Young Generation`영역보다 공간이 크며 그렇기에 Young 영역보다 GC가 적게 일어남.

     

     > **여기서 잠깐!** "오랫동안 살아남은 기준"은 뭘까

     ​	**`Minor GC`가 발생하는 동안 얼마나 오래 살아남았는지**를 기준으로 한다

     ​	각 객체는 `Minor GC`에서 얼마나 살아남았는지 횟수를 기록하는 `age bit`를 값을 가지고 있다

     ​	`Minor GC`가 발생할 때마다 `age bit`는 1씩 올라가며 이 값이 `MaxTenuringThreshold` 라는 설정 값	을 초과하게 된 경우 `Old` 영역으로 이동된다.

     ​	(`MaxTenuringThreshold`를 초과하기 전 survivor 영역이 부족하면 Old로 옮겨질 수 있다.)

     

     Old 영역에서는 `Major GC`가 일어나며 GC가 진행되는 동안 GC를 수행하는 스레드를 제외한 모든 스레드가 멈춘다. 이 상태를 `stop-the-world` 라고 하며 어떤 GC 알고리즘을 사용하더라도 이 상태를 해갈 수 없다.

<br/>
<br/>

### **Stack Area**

지역변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값이 위치한 영역입니다.
<br/>
<br/>


### **PC Registe**

Thread가 생성될 때 마다 생성되는 영역으로 현재 스레드가 실행되는 부분의 주소와 명령 즉 스레드와 관련 데이터가 위치한 영역입니다.

### **Native Method Stack**

자바 이외의 언어로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역입니다.

