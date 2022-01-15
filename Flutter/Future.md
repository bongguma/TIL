## Future
싱글스레드 환경에서 비동기 처리를 위해 존재한다.
```
Future<int>
```
닫혀있는 틀이라 표현하며 틀 안에서 then 메소드를 통해 int 값을, catchError 메소드를 통해 error 값 반환을 준비한다.
아래와 같이 형식을 정의할 수 있다.
```dart
Future<int> future = getFuture();

future.then((value) => handleValue(value))
      .catchError((error) => handleValue(error));
```

## async / await
Future를 조금 더 용이하게 다루기 위한 비동기 처리로, 이 때 async 함수는 무조건 Future를 반환해야한다.
```dart
/*
  'async' 키워드가 붙어있는 함수는 진행되는 동작과 별개로 실행이 가능한 함수(메소드)
  -> 하지만 꼭  Future를 통한 return이 일어나야한다. Future는 Future<void> 생략아다.
  
*/
Future<CreateData> createData() async {
  final id = await _loadId();
  final data = await _fetchData(id);
  return loadData(data);
}
```
위와 같은 예제 로직을 살펴보면,

await이 사용된 로직에서는 잠시 멈추고 함수를 호출한 곳에서 Future를 return 하게 되는데 await 동작이 완료되기 전까지 다음이 진행되지 않는다.
await 동작이 완료될 시, return을 통해 Future에서 return 값이 나오게 된다.
