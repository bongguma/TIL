## Dio   
http와 같이 서버와 통신을 하기 위해 필요한 패키지이다.   

__1. Request & Response__    
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

// example
void getDioExam async {
	var dio = Dio();

	try {
		var response = await .get('/example', queryParameters: {'number':1, 'name': 'bongguma'}); 
   		print(response);
	} catch (e) {
		print(e);
	}
}

)
```   
-> 요청 메소드와 Url을 같이 작성해주면 된다.   
   
__2. Options__     
```dart

// 첫 번째 방법 - default config (Dio 객체에 바로 대입)
var dio = Dio();

dio.options.baseUrl = 'https://www.example.com/api';
dio.options.connectTimeout = 2000; 
dio.options.receiveTimeout = 2000;

// 두 번쨰 방법 - BaseOptions instance (변수 선언해서 Dio 객체에 대입)
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
-> options을 활용해 더욱 디테일한 서버 통신이 가능하다.    
-> Dio 객체를 생성한 후, BaseOptions를 통해 공통적이 파라미터 값을 사용할 수 있겠금 한다.    
   
__3. Interceptors__      
```dart
class InterceptorExample {

  void onRequest(		// 요청 핸들링 
    RequestOptions options,
    RequestInterceptorHandler handler,
  ) =>
      handler.next(options);

  void onResponse(	// 응답 핸들링 
    Response response,
    ResponseInterceptorHandler handler,
  ) =>
      handler.next(response);

  void onError(		// 오류 핸들링 
    DioError err,
    ErrorInterceptorHandler handler,
  ) =>
      handler.next(err);
}

```
-> interceptor 자체는 클라이언트와 서버 사이에서 통신하는 역할로, 활용하여 요청/응답/오류에 대한 핸들링 및 반복적인 작업 처리   
   (ex) 토큰 유효성 즉 자동로그인 또는 로그 처리   
   
   *Secure Storage*
   - flutter secure storage는 기기의 보안 저장소를 이용할 수 있겠금 하는 라이브러리이다.
   - SharedPreferences와 다르게 민감한 정보들을 기기에 저장해야할 때 사용되며, 흔히 자동로그인에 사용된다. (토큰 값을 저장해두고는 한다.)   
   
ex) 자동로그인 예제
```dart
Future<Dio> authLoginDio(BuildContext context) async {  // 자동로그인에 대한 비동기 처리  
  var dio = Dio();

  final secureStorage = new FlutterSecureStorage(); // 데이터를 보안해서 저장

  dio.interceptors.clear(); // interceptors 정의

  dio.interceptors.add(InterceptorsWrapper(onRequest: (options, handler) async {  // 요청 핸들링
    // secureStorage 안에서 key값 읽어오기
    final accessToken = await secureStorage.read(key: 'ACCESS_TOKEN');   

    // 비동기 요청이 진행될 때마다 header에 accessToken 추가
    options.headers['Authorization'] = 'Bearer ${accessToken}';
    return handler.next(options);

  }, onError: (error, handler) async {  // error 핸들링
    if(error.response?.statusCode == 401) {   // accessToken값 만료로 인해 인증 오류가 발생했을 경우 
      
      // 기기에 저장된 accessToken과 RefreshToken 로드
      // 클라이언트는 두 개의 토큰 값을 가지고 있다. 사용자가 로그인 정보 필요 시, accessToken값을 보내 확인하고, 이 accessToken 값 만료 시 refreshToken값으로 다시 accessToken값을 새로 받아 요청하기 위해 필요
      final accessToken = await storage.read(key: 'ACCESS_TOKEN');
      final refreshToken = await storage.read(key: 'REFRESH_TOKEN');

      var refreshDio = Dio(); // 토큰 갱신 요청을 담당할 dio 객체 구현

      refreshDio.interceptors.clear();

      refreshDio.interceptors
          .add(InterceptorsWrapper(onError: (error, handler) async {

        // RefreshToken값의 만료로 인해 다시 인증 오류가 발생했을 경우 
        if (error.response?.statusCode == 401) {
          await secureStorage.deleteAll();  // 인증오류로 인한 자동 로그인 정보 삭제
          
          // 로그인 페이지로 이동

        }
        return handler.next(error);
      }));

        // 토큰 갱신 API 요청 시 125,126줄에서 읽어왔던 만료된 AccessToken, RefreshToken header에 추가
        refreshDio.options.headers['Authorization'] = 'Bearer $accessToken';
        refreshDio.options.headers['Refresh'] = 'Bearer $refreshToken';

        final refreshResponse = await refreshDio.get('/token/refresh'); // accessToken이 만료되었기 때문에 토큰 갱신 API 요청

        // response로부터 새로 갱신된 accessToken과 refreshToken 파싱
        final newAccessToken = refreshResponse.headers['Authorization']![0];
        final newRefreshToken = refreshResponse.headers['Refresh']![0];

        // 기기에 저장된 accessToken과 RefreshToken 갱신 await으로 갱신 데이터가 넘어오기 전까지 hold
        await storage.write(key: 'ACCESS_TOKEN', value: newAccessToken);
        await storage.write(key: 'REFRESH_TOKEN', value: newRefreshToken);

        // accessToken의 만료로 수행하지 못했던 API 요청에 담겼던 accessToken 갱신
        error.requestOptions.headers['Authorization'] = 'Bearer $newAccessToken';

        /* 수행하지 못했던 API 요청 복사본 생성
          이 곳에서 컨트롤 하는 error는 accessToken 만료로 인한 error 값을 말함.
          결국 access 연결 실패를 재요청하는 API를 생성하는 것- 
        */
        final clonedRequest = await dio.request(error.requestOptions.path,
            options: Options(
                method: error.requestOptions.method,
                headers: error.requestOptions.headers),
                data: error.requestOptions.data,
                queryParameters: error.requestOptions.queryParameters);
        
        return handler.resolve(clonedRequest);  // API로 재요청 
    }  

    return handler.next(error);
  }));

  return dio;
}
```
