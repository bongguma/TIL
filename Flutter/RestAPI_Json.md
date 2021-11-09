## RestAPI   

1. http 라이브러리 안에 http.get을 이용해 restAPI 호출    
```dart
void RestApi GetApi() async {
    http.Response response = await http.get(
        Uri.encodeFull('restAPI 주소'),
        headers: {"Accept": "application/json"});
    Map<String, dynamic> responseBodyMap = jsonDecode(response.body);

    // {'String : dynamic'} 출력
    print(response.body);

    // dynamic 데이터 출력
    print(responseBodyMap["String"]);
}

OR

Future<타입> GetApi() async {
    http.Response response = await http.get(
        Uri.encodeFull('restAPI 주소'),
        headers: {"Accept": "application/json"});
    Map<String, dynamic> responseBodyMap = jsonDecode(response.body);

    // {'String : dynamic'} 출력
    print(response.body);

    // dynamic 데이터 출력
    print(responseBodyMap["String"]);
}
```
- 서비스 호출 시, initState() 또는 didChangeDependencies() 안에서 진행
- 첫 번째 print문과 같이 response를 받아오게 될 때에는 Json 직렬화가 필요

## Json Serialization (Json 직렬화)   
1. 수동 Json 직렬화
2. 모델 클래스 내 Json 직렬화 (추천X)
```dart
class ExampleList{
    final String exam;
    final String examName;

    ExampleList(this.exam, this.examName);

    ExampleList.fromJson(Map<String, dynamic> json)
      : name = json['exam'],
        email = json['examName'];

  Map<String, dynamic> toJson() =>
    {
      'exam': exam,
      'examName': examName,
    };

}
```
3. json_serializable 방식으로 클래스 모델 (강추)
```dart
/*
 * json ExampleList 클래스 안에 private 멤버에 접근을 허용하는 *.g.dart
 * 명령어 - flutter pub run build_runner build
*/
part 'feeling_list_model.g.dart'; 

// Json화가 필요하다고 알려주는 어노테이션
@JsonSerializable() 

class ExampleList {
  String exam;

  ExampleList({
    required this.exam,
  });

  factory ExampleList.fromJson(Map<String, dynamic> json) =>
      _$ExampleListFromJson(json);

  Map<String, dynamic> toJson() => _$ExampleListFromJson(this);
}

```