# SmoothDamp

어떤 값이 변화할 때 한 순간에 딱 하고 변하는 게 아니라 목표값까지 부드럽게 점점 가까워지도록 할 때 사용되는 함수이다. 예를 들면 어떤 위치로 카메라를 부드럽게 이동해야 할 때 사용할 수도 있고 확대나 축소 시 부드러운 시야 변화를 위해 사용할 수도 있다.

## Move

목표 위치로 부드러운 이동을 할 때는 `Vector3.SmoothDamp` 함수를 이용할 수 있다.

```cs
private Vector3 target;
private Vector3 currentVelocity;
private float smoothTime;

private void Move()
{
    Vector3 current = transform.position;
    Vector3 nextPosition = Vector3.SmoothDamp(current, target, ref currentVelocity, smmothTime);
    transform.position = nextPosition;
}
```

## Zoom

목표 Zoom 크기로 부드러운 확대/축소를 할 때는 `Mathf.SmoothDamp` 함수를 이용할 수 있다.

```cs
private Camera camera;
private float target;
private float currentVelocity;
private float smoothTime;

private void Zoom()
{
    float current = camera.orthographicSize;
    float nextZoomSize = Mathf.SmoothDamp(current, target, ref currentVelocity, smoothTime);
    camera.orthographicSize = nextZoomSize;
}
```
