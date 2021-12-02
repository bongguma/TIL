## Dio   
http와 같이 서버와 통신을 하기 위해 필요한 패키지이다.   

1. __Request & Response__    
```dart
var dio = Dio();

// 첫 번째 response 방법 (쿼리로 서비스 연결)
final response_1 = await dio.get('/example', queryParameters: {'number':1, 'name': 'bongguma'})   
   
 
// 두 번째 response 방법 (body로 서비스 연결)
final response_1 = await dio.request(
	'/example',
	data: {'number':1, 'name': 'bongguma'},
	options : Options(method: 'GET'),
);

)
```   
-> 요청 메소드와 Url을 같이 작성해주면 된다.   
   
2. __Options__     
```dart

// 첫 번째 방법 (Dio 객체에 바로 대입)
var dio = Dio();

dio.options.baseUrl = 'https://www.example.com/api';
dio.options.connectTimeout = 2000; 
dio.options.receiveTimeout = 2000;

// 두 번쨰 방법 (변수 선언해서 Dio 객체에 대입)
var options = BaseOptions(
  baseUrl: 'https://www.example.com/api',
  connectTimeout: 2000,
  receiveTimeout: 2000,
);

Dio dio = Dio(options);


// BaseOptions
BaseOptions({
    String? method,	// 서버 통신하는 method 타입 
    String baseUrl = '',	 //요청 주소 설정 
    int? connectTimeout,		// 서버로부터 응답받는 시간 설정 
    int? receiveTimeout,		// 연결 지속 시간 설정 
    Map<String, dynamic>? headers,	// 요청 header 데이터 설정 
	.
	.
	.
  })

)
```    
-> Dio 객체를 생성한 후, BaseOptions를 통해 공통적이 파라미터 값을 사용할 수 있겠금 한다.    
   
3. __Interceptor__    
 

