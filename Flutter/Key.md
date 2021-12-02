## Key  
키가 필요한 경우는 흔하지 않다.   
하지만 동일한 상태 안에서 같은 위젯을 재정렬 또는 특정 위젯의 계층이 변경되는 상황이   
생기더라도 그 값이 유지되어 전달 될 수 있다.  
~ Flutter를 처음 접한 나에게도 생소한 개념이었다. ~    
   
__매개변수 Key 사용시점__    
   
1. 화면 간 이동 시, 단순히 데이터를 전달하고, 화면 변경이 발생하면서 생기는 이슈 대비   
매개변수 Key를 가지고 간단한 화면 간 이동에 있어 데이터를 주고 받을 수 있다.   
하지만 이 부분은 워낙 화면 간 이동에 있어서 param 값을 넘겨주는 방법이 다양하기도 해서 화면 간 이동을 위해 사용되는 경우는 드물다.   
   
2. 동일한 상태나 동일한 위젯에서 데이터를 재배치하거나 수정하는 경우   
동일한 위젯이 상황에 따라 데이터가 수정되는 경우가 발생할 수 있다.   
Flutter는 내부적으로 하나의 위젯마다 Element를 가지게 되는데 이 때, Widget과 Element가 상관관계에 있어야 하게 된다.   
   
Key를 사용하게 될 때에는 위젯 트리 최상단의 키를 정의한다.
   
     
__다양한 Key 종류__   
Key는 상황과 어떠한 위젯을 만드느냐에 따라 다양한 Key 유형 중 하나를 선택하게 되는데   
대표적으로는 ValueKey, ObjectKey, UniqueKey, GlobalKey 등이 존재한다.   

위젯 트리 간 상태를 유지하고 데이터를 유지하고 싶을 때 Key를 사용하게 된다.   
key가 사용되는 상황은 한 화면에 동일한 유형 위젯이 존재 시 발생하게 된다.   
이 때에는 어떠한 데이터나 위젯을 유지할 것인지에 따라 Key 종류가 변화하게 된다.  