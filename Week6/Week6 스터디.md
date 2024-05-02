### 날짜와 시간
#### 날짜와 시간 라이브러리 필요성
* 4년마다 하루를 추가하는 윤년, 달 일 수가 달라 날짜와 시간 차이 계산 복잡
* 일광 절약 시간(DST) 변환 -> 해외에서 사용
                           / 타임존 차이가 변화할 수 있음.
* 해외 타임존 계산 -> 대표적으로 UTC가 있음.

#### 자바의 날짜 라이브러리
![image](https://github.com/ChaeheunKim/Spring-Study/assets/123622993/1daceaf0-7370-481c-b3c0-2d0e1f826de6)
##### Local이 붙는 라이브러리는 타임존 고려 X
##### 모든 클래스는 불변이므로 새로운 객체를 생성해서 반환해야함
* LocalDate : 날짜만 표현(년,월,일)
  - now() : 현재 시간 기준 생성
  - of(..) : 특정 날짜 기준 생성
  - plusDays() : 특정 일 더함.
    
* LocalTime : 시간만 표현(시,분,초)
  - plusseconds() : 특정 초 더함.
    
* LocalDateTime : LocalDate + LocalTime
  - toXxx() : LocalDate와 LocalTime 분리
  - LocalDateTime.of(locaDate,localTime) : LocalDate와 LocalTime 합체
  - isBefore : 현재 날짜와 시간이 이전이면 true
  - isAFter : 현재 날짜와 시간이 이후이면 true
  - isEqual : 시간이 같으면 true
    
+)  왜 isEqual?
    => equals는 타임존이나 객체의 타입이 같아야 true를 반환하지만 isEqual은 시간만 같으면 true 반환

  
* ZonedDateTime : 타임존을 포함한 시간대를 고려한 날짜와 시간
  - ZoneID 클래스로 제공
  - ZoneId.systemDefault() : 기본 ZoneId 반환
  - ZoneId.of() : 타임존을 직접 제공해서 ZoneId 를 반환
  - 타임존 , 오프셋 정보 포함
  - withZoneSameInstant(ZoneId) : 타임존 변경
    
* OffsetDateTime : 고정된 오프셋(UTC와의 차이)만 포함한 시간대를 고려한 날짜와 시간
  - 일광 절약 시간 고려 X
    
* Instant : 날짜와 시간을 나노초 정밀도로 표현 -> 초 데이터만 들어 있다.
  - Epoch 시간을 다룸
    
* Epoch 시간?

  1970년 1월 1일 00:00:00: UTC부터 형재까지 경과된 시간을 초 단위로 표현한 것으로, 절대적인 시간 표현 방식
  - 장점 : 시간대 독립, 고정된 기준점
  - 단점 : 사용자 친화적이지 않고, 시간대 정보 부 
  - from() : 다른 타입의 날짜와 시간 기준으로 생성(LocalDateTime 사용 불가)
  - ofEpochSecond() : 에포크 시간을 기준으로 Instant 를 생성
  - plusSeconds() : 초를 더함
  - getEpochSecond() : 에포크 시간 반
     
* Period vs Duration
![image](https://github.com/ChaeheunKim/Spring-Study/assets/123622993/7f78bddd-a42c-4aa7-b19d-a5e59fc1da5a)
 - of() 로 생성
 - Periond/Duration.between(startDate/start, endDate/end) => 차이 반환

* 인터페이스
  * TemporalAccessor 인터페이스
     - 시간과 날짜 정보를 읽을 수 있는 최소한의 기능 인터페이스
     - TemporalAccessor.get(TemporalField field) -> ChronoField 인수로 전달하여 field 호출
     - Temporal with(TemporalField field, long newValue) -> 날짜와 시간의 특정 필드의 값만 변경

   * Temporal 인터페이스
     - 날자와 시간을 변경하거나 조작하는 인터페이스
     - plus(long amountToAdd, TemporalUnit unit) -> ChronoUnit 을 인수로 전달하여 Unit 전달 
     - 불변
=> 두 인터페이스 다 isSupported로 특정 시간 단위나 필드를 사용할 수 있는 지 확인 가능
=> 자주 쓰는 메서드는 편의 메서드를 제공한다.

  * TemporalAmount 인터페이스
     - 시간의 간격으로, 날짜와 시간 객체를 조정하느 ㄴ인터페이스

   * TemporalUnit(ChronoUnit) 인터페이스
     - 날짜와 시간을 측정하는 단위

   * TemporalField(ChronoField) 인터페이스
     - 날짜와 시간의 특정 부분을 나타내는데 사용되는 열거형(연도, 월, 일, 시간 , 분 ...)

  * DayOfWeek
     - 월, 화, 수, 목, 금, 토, 일을 나타내는 열거형
 
#### 파싱과 포매팅
* 포매팅
  - 데이터를 문자열로 변경(Date -> String)
* 파싱
  - 문자열을 데이터로 변경(String -> Date)

  => 둘 다 DateTimeFormatter 사용
---

### 중첩클래스
* #### 정적 중첩 클래스
  - ##### 중첩(Nested) : 다른 것이 내부에 위치하는 구조적인 관계 -> 바깥 클래스의 인스턴스 소속 X
  - 정적 변수와 같은 위치에 생성(static)
  - 자신 멤버에는 접근 가능
  - 바깥 클래스의 인스턴스 멤버에 접근 불가능
  - 바깥 클래스의 클래스 멤버 접근 가능
    ##### private 가능
  - new 바깥클래스.중첩클래스()로 생성 가능
  - 바깥클래스.중첩클래스로 접근 가능
  => 그냥 클래스 2개인데 중첩해둔 것일 뿐.
  => 바깥 클래스의 안에서만 사용하는 것


* #### 내부 중첩 클래스
* - ##### 내부(inner) : 내부에 있는 나를 구성하는 요소 -> 바깥 클래스의 인스턴스에 소속
  * 내부 클래스
  - 인스턴스 변수와 같은 위치에 생성(non-static)
  - 자기 자신의에 접근
  - 바깥 클래스의 인스턴스 멤버에 접근 가능(private도 가능)
  - 바깥 클래스의 클래스 멤버에 접근 가능(private도 가능)
  - ##### new 바깥클래스의 인스턴스 참조.내부클래스() 로 생성 -> 인스턴스 정보를 알아야 생성가능
```
public class Car {
   private String model;
   private int chargeLevel;
   private Engine engine;
     public Car(String model, int chargeLevel) {
           this.model = model;
           this.chargeLevel = chargeLevel;
           this.engine = new Engine();
 }

public void start() {
      engine.start();
      System.out.println(model + " 시작 완료");
 }

 private class Engine {
       public void start() {
          System.out.println("충전 레벨 확인: " + chargeLevel);
          System.out.println(model + "의 엔진을 구동합니다.");
        }
    }
}
```
- model과 chargeLevel 직접 사용 가능

  * ##### 지역 클래스
  - 지역 변수와 같은 위치에 생성
  - 지역변수, 매개변에 접근 가능
  - 접근 제어자 사용 불가

  * ##### 변수의 생명 주기
  ![image](https://github.com/ChaeheunKim/Spring-Study/assets/123622993/c2307c88-be2a-4f34-815b-c6998c355e6a)
  - 클래스 변수: 프로그램 종료 까지, 가장 김(메서드 영역)
  - 인스턴스 변수: 인스턴스의 생존 기간(힙 영역)
  - 지역 변수: 메서드 호출이 끝나면 사라짐(스택 영역)
```
public class LocalOuterV3 {
  private int outInstanceVar = 3;
    public Printer process(int paramVar) {
          int localVar = 1; //지역 변수는 스택 프레임이 종료되는 순간 함께 제거된다.
            class LocalPrinter implements Printer {
               int value = 0;

              @Override
              public void print() {
                System.out.println("value=" + value);
                 //인스턴스는 지역 변수보다 더 오래 살아남는다.
                System.out.println("localVar=" + localVar);
                System.out.println("paramVar=" + paramVar);
                System.out.println("outInstanceVar=" + outInstanceVar);
                 }
          }
     Printer printer = new LocalPrinter();
      //printer.print()를 여기서 실행하지 않고 Printer 인스턴스만 반환한다.
 return printer;
 }

 public static void main(String[] args) {
        LocalOuterV3 localOuter = new LocalOuterV3();
        Printer printer = localOuter.process(2);
        //printer.print()를 나중에 실행한다. process()의 스택 프레임이 사라진 이후에 실행
        printer.print();
    }
}
```
* 사라진 변수를 참조할 수 있는 이유? => 캡쳐
  ![image](https://github.com/ChaeheunKim/Spring-Study/assets/123622993/665980c0-caeb-4112-83e0-41ac4648990e)


    ##### => 지역 클래스는 지역 변수를 캡쳐에서 인스턴스에 보관한뒤 캡쳐 변수에 접근한다.
* ##### 지역클래스가 접근하는 지역변수는 값이 변하면 안됨(파이널이어야 함)
  => 값이 변하면 캡쳐한 변수와 변수의 값의 차이가 생기는 동기화 문제가 발생하기 때문.
  
  * ##### 익명 클래스
  - 지역 클래스의 종류 중 하나로, 클래스 이름이 없음
    ```
    Printer printer = new Printer(){
    //body
    }
    ```
 - 지역 클래스를 선언하면서 동시에 생성
 - ##### 부모 클래스를 상속받거나, 인터페이스를 구현해야 함.
 - 생성자를 가질 수 없음
 - 여러 개의 인스턴스를 생성해야 할 때는 부적합.
 - 변하는 부분과 변하지 않는 부분을 분리하여 변하는 부분을 외부에서 전달받으면 재사용성 증가.

   -> 코드 조각이 변하는 부분 일떄는, 인스턴스를 전달하고 해당 인스턴스에 있는 메서드를 호출하는 방식으로 함.
* 람다 : 메서드를 인수로 전달
     - 코드 블럭을 직접 전달

* 중첩클래스를 사용하는 경우와 이유
   - 특정 클래스가 다른 하나의 클래스 안에서만 사용되거나, 둘이 긴밀하게 연결되어 있는 경우에 사용
   - 논리적 그룹화 / 캡슐화 때문에 사용

* #### 같은 이름의 바깥 변수 접근
  - 섀도잉(Shadowing) : 다른 변수를 가려서 보이지 않게 하는 것
```
public class ShadowingMain {
    public int value = 1;
        class Inner {
           public int value = 2;
             void go() {
               int value = 3;
               System.out.println("value = " + value);
               System.out.println("this.value = " + this.value);
               System.out.println("ShadowingMain.value = " +
              ShadowingMain.this.value);
          }
   }
```

    

   
