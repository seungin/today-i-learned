# Translation and Coordinate System

유니티에서의 평행이동과 좌표계 사이의 관계에 대해서 설명한다. 또한 오브젝트 간 부모 자식 관계에서 평행이동과 좌표계가 어떻게 다루어지는지 알아본다.

## Transform property

오브젝트의 컴포넌트는 일반적으로 GetComponent 함수를 이용하여 스크립트에서 얻어와 사용한다. 다만 유니티에서는 자주 사용하는 Transform 컴포넌트를 미리 Property로 제공하여 직접 접근하도록 편의를 제공한다.

## How to move objects

물체의 위치를 바꾸는 방법은 두 가지가 제공된다.

* transform 속성을 이용
  - transform.position 속성은 World 좌표계를 기준으로 이동한다.
  - transform.localPosition 속성은 Local 좌표계를 기준으로 이동한다.
  - 마찬가지로 회전에 대해서는 rotation, localRotation 속성이 있다.
  - 크기에 대해서는 lossyScale, localScale 속성이 있다.

* transform.Translate 함수를 이용
  - 기본적으로는 Local 좌표계를 기준으로 이동한다.
  - World 좌표계를 기준으로 이동하고 싶으면 2번째 인자에 좌표계를 지정하여 줄 수 있다.

## Parent and child objects

Hierarchy 상에서 부모 자식 관계에 있는 경우, 자식 오브젝트는 World 좌표계가 아닌 부모의 좌표계를 기준으로 position이 정해진다. position 뿐만 아니라 rotation, scale에 대해서도 동일하게 부모가 기준이 된다.
