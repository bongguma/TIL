**상태관리 변천사**   
*bloc -> provider -> GetX*

## GetX
GetX는 같은 기능도 더욱 간편하고 간결하게 표현하는 *생산성*과   
최소한의 리소스 소비와 *성능*을 중요시하며   
비지니스로직과 종속성 주입 및 네비게이션 분리 등 *조직화*를 중요시하게 여긴다.   

__GetX 상태관리 초기 세팅__   

*GetMaterialApp*   
- GetX가 프로젝트 모든 것을 관리할 수 있게 설정   
- 라우트 관리, 스낵바, 바텀시트 등 그 밖에 기능을 활용 가능   
   
   
__GetX 상태관리__   
1. 단순 상태 관리자(GetBuilder)   
```dart
class ExampleController extends GetxController{
	int number = 1;
	
	void plusNumber(){
		number ++;
		update();
	}
}

Getbuilder<ExampleController>(
	builder: (controller) {
		return Text( '${controller.number}');
	}
)
```

단순하게 상태를 관리해주는 관리자로써,   
ExampleController에서 GetxController를 상속받아 컨트롤러를 생성한 후      
변경된 ExampleController 상태를 Getbuilder로 전달 받아 UI 활용   
   
   
2. 반응 상태 관리자(GetX/Obx)   
단순 상태 관리자와 다르게 만약 number 안에 들어있는 기존 값과 동일하면 Obx는 리소스를 아끼기 위해   
값을 무시하고 재빌드를 진행하지 않는 것을 이야기한다.
     
반응형 변수 선언 3가지 방법
1. Rx{Type}
```dart
final number = RxInt(1);
```
2. Rx<Type>
```dart
final number = Rx<Int>(1);
```
3. 변수 선언 초기화 뒤 '.obs' 속성 추가
```dart
final number = 1.obs;
```

*하지만 단순 상태 관리 update()를 통해 수정되는 것은 observable이 아니기에 변경되지 않는다.*
         
~위 단순 상태 관리자 코드를 반응형 상태 관리자로 수정~~
```dart
 class ExampleController extends GetxController{
	RxInt number = 1.obs;
	
	void plusNumber(){
		number ++;
	}
}

Obx(() => Text( '${controller.number}'))
```
     

__GetX 라우터 관리__
  ```dart
/* 이름없는 페이지 이동 */
Get.to(ExamplePage());	// Getx에서 일반적인 페이지 이동

Get.to(ExamplePage(), transition: Transition.downToUp);	// 페이지 전환 효과를 이용한 페이지 이동

/* 인수 받아오기 */
Get.to(ExamplePage(), arguments: 'Hello');	// 페이지 이동 시, 인수도 함께 전달
print(Get.argument); 	// 다음 페이지에서 전달받은 인수 사용하는 방

/* 동적 인수 url로 받아오기 */
Get.offAllNamed('/ExamplePage?device=phone');
print(Get.parameters[device]);	// 다음 페이지에서 받아오기

/* 동적 인수 namedParameters로 받아오기 */
GetPage(
	name: '/exmaplePage/:device'
	page: () => ExamplePage(), 
)
print(Get.parameters[device]);	// 다음 페이지에서 받아오기

/* 이름있는 페이지 이동 */
Get.toNamed('/ExamplePage');

/* 이전 한 페이지만 삭제 후 페이지 이동 */
Get.off(() => ExamplePage());

/* 이전 모 페이지만 삭제 후 페이지 이동 */
Get.offAll(() => ExamplePage());

/* GetX route 정의 */
GetMaterialApp(
	initialRoute: '/',
	getPages : [
		GetPage(name: '/', page: () => ExamplePage()),
	]
)
``` 

__GetX 종속성 관리__   
GetX는 context에 의존하지 않아 Controller 사용 시에만 선언을 진행한다.   
종속성 인스턴스 방법에는 총 4가지가 있다.   

``` dart
Get.to(GetPutPage(), binding: BindingsBuilder(() {	// binding을 통한 인스턴스 전달 
	/* Get.put() 방식 */
	Get.put(ExampleController()); // 전환되는 페이지에 controller 전달 
	Get.put<ExampleController>(ExampleController())	// 전환되는 페이지에 controller 전달
	
	/* Get.lazyPut() 방식 */
	Get.lazyPut<ExampleController>(
       () => ExampleController());	// 페이지 전환이 일어나도 인스턴스 사용되기 전까지는 생성되지 않는다. 
       							// Get.find()가 실행 될 때 인스턴스가 생성된다.
       							
	/* Get.putAsync() 방식 */
     Get.putAsyn<ExampleController>( () async{	// 비동기로 인스턴스를 생성하기 원할 때 사용
	  final number = 1;
	  await number ++;
	  return number;
	});
}));

``` 
