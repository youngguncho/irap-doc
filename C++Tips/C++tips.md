## 접근 식별자의 의미

* public : public 식별자로 정의된 객체의 멤버함수 또는 멤버변수는 다른 코드에서 제한없이 호출하거나 읽고 쓸 수 있음.
* protected : 동일 클래스 안에서만 protected 식별자로 정의된 멤버 변수를 호출하거나 읽고 쓸 수 있음. 해당 객체를 상속받은 subclass에서도 protected 멤버변수나 멤버함수에 접근 가능.
* private : private 식별자로 정의된 메서드나 멤버 변수는 해당 클래스에서만 접근 가능하며 subclass에서도 접근 불가.

## 디폴트 생성자(Default Constructor)가 필요한 상황

* 객체 배열을 생성할 때
* STL container를 이용할 때
* 클래스 안에서 다른 클래스를 생성할 때
* 초기화를 default로 명시해주고싶을 때

## Constructor initializer 사용 시 주의사항

```cpp
ExClass::ExClass():mVar(1),mString("") {
}
```
_ **constructor initilizer는 멤버를 초기화할 때 위의 순서대로 초기화 되는것이 아니라 class 정의에서 등장하는 순서로 초기화 됨** _

### constructor initilizer로만 멤버 초기화가 가능한 경우
1. const 멤버 변수 : const 변수는 한번 생성된 이후에 값을 변경할 수 없어 변수 생성시점에 초기화
2. 참조 타입 데이터 멤버
3. 디폴트 생성자가 없는 class
4. 디폴트 생성자가 없는 슈퍼클래스

## 대입연산자 예시
```cpp
ExClass& ExClass::operator=(const ExClass& rhs) {
	if (this == &rhs) {
    	return *this;
    }
    mVar = rhs.mVar;
    mString = rhs.mString;
}
```

## 생성자와 대입연산의 차이
```cpp
	ExClass exClass1(1.0);
    ExClass exClass2;
    exClass2 = exClass1;
    ExClass exClass3 = exClass2;
```
첫번째 라인의 경우 명시적으로 선언된 생성자 호출, 두번째 라인은 디폴트 생성자, 세번째 라인의 경우 대입연산, 마지막 라인의 경우 복제 생성자 호출

## Constructor initilizer와 복제생성자와의 관계
```cpp
ExClass::ExClass(const ExClass& src) 
	:mString(src.mString)
{
	mVar = src.mVar;
}
```
위와 같은 복제 생성자의 경우 mString(src.mString)에서는 복제 생성자가 호출되지만 mVar=src.mVar에서는 복제생성자가 아니라 대입연산자가 생성. constructor initilizer에서 빠진 부분에 대해서는 컴파일러가 디폴트 생성자를 호출해서 이미 mVar는 메모리에 할당이 된 상태이므로 {}블럭 내에서는 대입연산이 이루어짐.
