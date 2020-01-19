# Delegate

어떤 기능들을 리스트에 등록을 하면 등록된 기능들을 대신 수행해주는 역할을 하는 것을 delegate 라고 한다. C++ 에 비교하자면 마치 함수포인터 리스트를 사용하는 것 같은 느낌을 준다. delegate 가 동작하는 방식을 아래 예를 들어 설명한다.

## How to Use Delegate

먼저 delegate를 선언하고 delegate 객체를 만든다.

```cs
delegate float Calculate(float a, float b);

Calculate onCalculate;
```

delegate 객체에 형식에 맞는 함수를 더하거나 빼거나 할 수 있다. 아래 예시에서 Subtract 함수는 delegate 에 의해 수행되지 않는다.

```cs
float Sum(float a, float b);
float Subtract(float a, float b);
float Multiply(float a, float b);

onCalculate = Sum;
onCalculate += Subtract;
onCalculate -= Subtract;
onCalcluate += Multiply;
```

마지막으로 호출만 해주면 등록된 모든 함수가 호출되는데, 마지막 등록된 함수의 return값을 반환한다.

```cs
Debug.Log("Result: " + onCalculate(1, 10));
```

## Example Code

전체 코드는 아래와 같다.

```cs
public class Calculator : MonoBehavior
{
    delegate float Calculate(float a, float b);

    Calculate onCalculate;

    public float Sum(float a, float b)
    {
        Debug.Log(a + b);
        return a + b;
    }

    public float Subtract(float a, float b)
    {
        Debug.Log(a - b);
        return a - b;
    }

    public float Multiply(float a, float b)
    {
        Debug.Log(a * b);
        return a * b;
    }

    private void Start()
    {
        onCalculate = Sum;
        onCalculate += Subtract;
        onCalculate -= Subtract;
        onCalculate += Multiply;
    }

    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Debug.Log("Result: " + onCalculate(1, 10));
        }
    }
}
```
