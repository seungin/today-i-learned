# Coroutine

유니티에서 제공하는 Coroutine은 크게 2가지 특징을 지닌다. 하나는 함수 호출 중에 delay를 줄 수 있다는 것, 또 다른 하나는 Async로 동작시킬 수 있다는 것이다. Coroutine을 실행하는 방법에 따라 실행 중인 Coroutine을 정지시킬 수도 있다.

## Delay in the middle of Function Call

함수 호출이 진행되는 중에 지연 시간을 줄 수 있다. yield return 으로 함수를 빠져나간 후 해당 시간이 경과되면 이전 상태로 다시 돌아와 작업을 계속한다.

```cs
public Image image;

private void Start()
{
    StartCoroutine(FadeIn());
}

IEnumerator FadeIn()
{
    for (int i = 0; i < 100; ++i)
    {
        Color fadeColor = image.color;
        fadeColor.a -= 0.01f;
        image.color = fadeColor;

        yield return new WaitForSeconds(0.01f);
    }
}
```

## Async Operation

Coroutine은 비동기적으로 실행된다. 그러므로 Coroutine 안에 while loop가 존재하더라도 Coroutine 다음 실행이 연속해서 진행된다.

```cs
private void Start()
{
    StartCoroutine("Tick");
    Debug.Log("Start");
}

IEnumerator Tick()
{
    while (true)
    {
        yield return new WaitForSeconds(1);
        Debug.Log("Tick");
    }
}
```

## Start/Stop Coroutine

Coroutine 실행 방법은 2가지가 있다. Coroutine 함수를 넣는 방법과 함수 이름을 넣는 방법이다. 함수 이름을 넣어 실행하는 경우에는 Coroutine 실행을 중단할 수 있다.

```cs
private void Start()
{
    StartCoroutine("Tick");
}

private void Update()
{
    if (Input.GetMouseButtonDown(0))
    {
        StopCoroutine("Tick");
    }
}
```
