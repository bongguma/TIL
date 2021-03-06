## Null-Safety   
예상하지 못한 null을 대응하는 함수로 Flutter 2.0 에서 제공   
null-safety에 있어서    
! -> 느낌표가 붙는 변수는 '무조건 null이 아니다'라고 단정    
? -> 물음표가 붙는 순간 'null일 수도 있고 null이 아닐 수도 있다.'로 의미된다.   

+ null-safety 적용 이후,   
초기화 되어 있지 않으면 컴파일러가 해당 변수가 사용 가능하다는 것을 보증할 수 없으므로 에러이다.   

```dart
...
int firstNum = 0; // 이와 같이 초기화를 진행해주지 않으면 에러 발생

class ExampleClass {
	static int staticVar = 1;	// 위와 동일하게 초기화 필요 
					// someClass 생성자 진입 전 초기화 진행 시에는 초기화를 별도로 진행하지 않아도 됨.
	int ExampleFunc(int num) {
		int numResult;	// 지역변수 경우에는 변수 사용 전에만 초기화 하면 된다.
		numResult = 3; 
	}
}

```

+ Nullable Type : 변수 타입 뒤 ?   
but, 이 경우에도 null이 되는 경우가 존재한다.   

```dart 
class ExampleClass{
	String? name;
}

```
+ Not Nullable Type : 변수에 !   
non-nullable 변수 값을 가진다는 확신이 생길 때 Nullable 변수에 느낌표 추가 

```dart 
class ExampleClass{
	String? name;
	int age;
	
	print("name : ${nmae}  age : &{age!}")
}

```

+ late 연산자
변수 초기화를 지연   

```dart
class ExampleClass{
	/* 인스턴스 변수가 생성 시 초기화를 하지 않아 컴파일 에러 발생 */
	String exampleStr;
	void ExampleFunc() => exampleStr = "flutter";
	
	String ExamplePrint() => exampleStr;
	
	/*late 연산자 적용 */
	late String exampleStr;
	void ExampleFunc() => exampleStr = "flutter";
	
	String ExamplePrint() => exampleStr;
}
```
   
 + required
Null-Safety 이후,
```dart
void main() {
	print(add());
}

/* + 인자값이 null에 대해 따로 대응을 해주지 않아 컴파일 에러
   + 변수타입 앞에 required 키워드를 추가해주어야 한다.	
 */
 
// 아래와 같은 인자 값 설정 시 오류 
	int sum = a + b;
	return sum;
}


int add({required int a, required int b}){
	int sum = a + b;
	return sum;
}
```


 
