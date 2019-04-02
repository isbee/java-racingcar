# java-racingcar
n대의 자동차 중 가장 멀리 움직인 자동차가 우승하는 경주 게임

## 프로그래밍 요구사항
* 주어진 Car class를 활용하여 경주 게임을 구현한다. **단, 아래의 제약 조건을 준수해야 한다.**
    * name, position 변수의 접근 제어자를 변경할 수 없다
    * Car 기본 생성자를 추가할 수 없다
    * 가능한 SetPosition(int position) 메소드를 추가하지 않는다

```$java
 public class Car {
    private final String name;
    private int position = 0;
    
    public Car(String name) {
        this.name = name;
    }
    // 추가 기능 구현
 }
```
* 클래스, 메소드를 최대한 분리하여 구현한다

## 기능 요구사항
* 주어진 횟수 동안 n대의 자동차는 전진 또는 멈출 수 있다.
* 각 자동차에 이름을 부여할 수 있다. 전진하는 자동차를 출력할 때 자동차 이름을 같이 출력한다. •자동차 이름은 쉼표(,)를 기준으로 구분하며 이름은 5자 이하만 가능하다.
* 사용자는 몇 번의 이동을 할 것인지를 입력할 수 있어야 한다.
* 전진하는 조건은 0에서 9 사이에서 random 값을 구한 후 random 값이 4 이상일 경우 전진하고, 3 이
하의 값이면 멈춘다.
* 자동차 경주 게임을 완료한 후 누가 우승했는지를 알려준다. 우승자는 한 명 이상일 수 있다.

## 구현된 기능 목록
1. 0~9 사이의 난수 생성 기능
    * 0~9을 초과하는 난수가 발생할 경우 실패하는 테스트 작성 후, 테스트를 성공시키는 **난수 생성 클래스 구현**
    * 난수 생성에는 Random 사용
    * **Nested class**를 사용하여 난수의 상한선(9)/하한선(0)을 따로 저장
    
2. 난수값이 4 이상이여야 전진하는 자동차 조종 기능
    * 난수가 4 미만인데 전진하거나, 4 이상인데 멈추는 경우 실패하는 테스트 작성. 이후 테스트를 성공시키도록 **Car 클래스 구현.**
    * 자동차의 이전 위치를 저장하는 prevPosition을 사용해 doesCarMoved()를 구현하고, 이를 test에 활용한다.
    
3. 플레이어로부터 n대의 자동차 이름을 입력 받는 기능
    * 입력을 검증할 클래스에 대한 테스트 작성
        * 자동차 이름을 나타내는 String이 null이거나, 자동차 이름의 길이가 상한선(5)을 초과하는 경우 실패하는 테스트 작성
    * **위 테스트를 성공시키는 클래스**와, 해당 클래스를 사용해 **입력을 처리하는 클래스 구현**
    * 플레이어의 입력은 정규식을 사용해 "," 단위로 split하여 저장한다

4. 플레이어로부터 자동차 이동 횟수를 입력 받는 기능
    * 이동 횟수가 음수인 경우 검증에 실패하는 테스트 작성 후, 테스트를 성공시키는 **입력 검증 클래스 구현**
    * 자동차 이동 횟수 검증, 자동차 이름 검증은 '검증'이라는 행위를 공유한다. **따라서 검증 인터페이스를 추출한다.**
    
5. 경주 게임을 진행하는 기능
    * 지금까지 구현한 기능들을 활용해 **경주 게임을 진행하는 클래스**를 구현한다
        * **게임 진행 사항, 결과를 출력하는 클래스**는 따로 추출한다.
    * 경주 중인 **모든 자동차를 확인**한 뒤 가장 멀리 나간 자동차를 우승자로 결정한다
        * O(N). (N = 자동차 갯수)
    * **게임 진행 사항을 '-'로 출력하면 진행 횟수가 많을 경우 보기 불편하므로, *숫자*로 출력한다.**
    
6. 리팩토링
    * 같은 기능을 가진 모듈들을 패키지로 묶어낸다 (e.g. CarNameValidator, MoveNumValidator)
    * 생성자 대신 static factory method를 사용한다
    * String.split() 대신 Pattern.split()을 사용하여 성능을 최적화한다
    * 자동차 이동 횟수로 *문자*를 입력 받는 예외를 처리한다 