# Instantiate

유니티에서 씬 상에 실시간으로 게임 오브젝트를 생성하는 것을 인스턴스화라고 부른다.

## Prefab?

유니티에서는 게임 오브젝트를 미리 정의해 두고 필요할 때 실시간으로 씬 상에 찍어내 사용하는데, 이렇게 미리 만들어둔 게임 오브젝트를 Prefab 이라고 한다.

## How to create an instance

생성하고자 하는 게임 오브젝트를 인자로 넣어 Instantiate 함수를 호출하면 인스턴스가 하나 생성된다.

```cs
public GameObject targetObject;
public Transform targetTransform;

private void Start()
{
	Instantiate(targetObject);
	Instantiate(targetObject, targetTransform);
	Instantiate(targetObject, targetTransform.position, targetTransform.rotation);
}
```

Instantiate 함수는 생성한 게임 오브젝트를 반환한다. 인자로 넣는 타입이 컴포넌트 타입이면 반환값도 컴포넌트 타입을 반환한다.

```cs
public GameObject targetObject;
public Rigidbody targetBody;

private void Start()
{
	GameObject obj = Instantiate(targetObject);
	Debug.Log(obj);
	
	Rigidbody body = Instantiate(targetBody);
	body.AddForce(Vector3.up);
}
```
