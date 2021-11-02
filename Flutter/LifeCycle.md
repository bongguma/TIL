## StatefulWidget LifeCycle

1. createState()
statefulWidget이 만들어질 경우에 바로 호출된다.
```
class ExmaplePage extends StatefulWidget {
  @override
  _ExmaplePageState createState() => new _ExmaplePageState();
}
```

2. initState()
처음 메소드 실행할 때에 바로 호출된다.
initState가 필요할 때는,
    - BuildContext를 이용해 데이터 초기화가 필요할 시,
    - 위젯에서 부모 위젯 속성 초기화 필요 시,
    - 스트림 구독 시,

```
@override
initState() {
    super.initState();
    .
    .
}
```

3. didUpdateWidget(Widget oldWidget)
initState 다음으로 호출되며, 이 위젯을 다시 만들 경우 업데이트가 이루어진다.
주의할 점은 Flutter는 state를 재사용하므로 initState 값을 초기화해야 할 수도 있다. 

```
@override
void didUpdateWidget(Widget oldWidget) {
    .
    .
}
```

4. dispose()
state가 영구적으로 삭제될 때 호출되므로 Stream 해제 시 자주 이용된다.
