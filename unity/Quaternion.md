# Rotation and Quaternion

유니티에서의 회전과 쿼터니언 개념에 대해서 설명한다.

## Why do Unity use Quaternion?

물체의 방향를 표현하는 방법으로 오일러 각과 쿼터니언이 있다. 예를 들어 3차원 공간을 가정하면 오일러 각은 X, Y, Z축을 이용하여 방향를 표현하기 때문에 직관적으로 이해하기 쉬운 반면 쿼터니언은 복소수를 이용한 방식으로 방향을 표현하며 이해하기 쉽지 않은 개념이다. 그럼에도 유니티에서는 쿼터니언을 사용하여 각도를 표현하고 있다. 그 이유는 오일러 각이 가진 짐벌락이라는 취약점 때문이다.

## Gimbal Lock?

오일러 각 회전은 각 축을 기준으로 순서대로 회전을 계산한다. 그런데 만약 90도 회전이 일어나면 축이 겹쳐지는 현상이 발생하면서 한 축의 정보가 소실되고 3차원이 아닌 2차원 정보 밖에 표현하지 못하는 상황이 발생하는데, 이를 짐벌락 현상이라 부른다. 이해를 돕기 위해 아래 링크를 추가한다.

https://www.youtube.com/watch?v=zc8b2Jo7mno

## How to rotate objects in Unity?

유니티는 이러한 문제점을 해결하기 위해 회전을 표현할 때 쿼터니언을 이용하지만 사용자가 이를 쉽게 사용할 수 있도록 편의 기능을 제공한다. 유니티 Quaternion 클래스의 함수를 이용하면 오일러 각을 통해 쿼터니언을 구하거나 쿼터니언으로부터 오일러 각을 구할 수 있게 해준다. 사용자는 이러한 함수들의 사용법만 익히면 된다.

## Then, how to rotate objects?

* rotation 속성을 이용
  - transform.rotation 속성은 World 좌표계를 기준으로 이동한다.
  - transform.localrotation 속성은 Local 좌표계를 기준으로 이동한다.

* 쿼터니언을 구하는 방법

	```cs
	// Euler 함수: target 오일러 각 위치로 회전
	Quaternion targetRotation = Quaternion.Euler(new Vector3(45, 0, 0));
	transform.rotation = targetRotation;

	// LookRotation 함수: target 방향을 향하도록 회전
	Vector3 direction;
	targetRotation = Quaternion.LookRotation(direction);
	transform.rotation = targetRotation;

	// Lerp 함수: 두 값 사이 중간값으로 회전
	Quaternion a = Quaternion.Euler(new Vector3(30, 0, 0));
	Quaternion b = Quaternion.Euler(new Vector3(60, 0, 0));
	targetRotation = Quaternion.Lerp(a, b, 0.5f);
	transform.rotation = targetRotation;

	// 쿼터니언으로부터 오일러 각 구하기
	Vector3 currentEulerAngle = transform.rotation.eulerAngles;
	currentEulerAngle += new Vector3(30, 0, 0);
	transform.rotation = Quaternion.Euler(currentEulerAngle);

	// 쿼터니언 연산
	Quaternion originalRotation = Quaternion.Euler(new Vector3(45, 0, 0));
	Quaternion plusRotation = Quaternion.Euler(new Vector3(30, 0, 0));
	targetRotation = plusRotation * originalRotation;
	transform.rotation = targetRotation;
	```

* transform.Rotate 함수를 이용
  - 기본적으로는 Local 좌표계를 기준으로 이동한다.
  - World 좌표계를 기준으로 이동하고 싶으면 2번째 인자에 좌표계를 지정하여 줄 수 있다.

	```cs
	// Rotate 함수를 이용하여 회전
	transform.Rotate(new Vector3(30, 0, 0));
	```
