## Shared Preferences   
shared Prefernes를 통해 데이터를 저장하게 된다면 앱 어플리케이션이 종료되어도 데이터는 앱 안에 남아있게 된다.   
key - value 형태로 데이터가 저장되게 되며, 간단한 데이터 저장 정도는 좋다.   
(ex - alert창 닫기 또는 다시 보지 않기 등 ..)
   
먼저 Shared Preferences 기능 사용을 위해 pubspec.yaml 추가   
   
```dart
dependencies:
	shared_preferences:
```
   
1. Shared Preferences 초기화   
```dart
SharedPreferences pref = await SharedPreferences.getInstance();
```
   
SharedPreferences 구조는 파일 입출력이기에 비동기 방식인 await를 사용   

2. 다양하게 데이터 가져오거나 또는 데이터 저장하기   
```dart
/* 앱 안 저장된 key-value 형식 데이터 중 모든 키값 가져오기 */
pref.getKeys();

/* key-value 형식으로 앱 안에 데이터 저장하기 */
pref.setBool('key값 명칭', true);
pref.setInt('key값 명칭', 0);
pref.setString('key값 명칭', 'value');
pref.setStringList('key값 명칭', List<String>['a', 'b']);

/* key값 명칭을 통해 해당 값 삭제 */
pref.remove('key값 명칭');


/* SharedPreferences 저장된 데이터 전체 삭제 */
pref.clear();

```

