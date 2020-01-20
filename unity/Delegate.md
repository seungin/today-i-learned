# Delegate and Event

어떤 기능들을 리스트에 등록을 하면 등록된 기능들을 대신 수행해주는 역할을 하는 것을 delegate 라고 한다. C++ 에 비교하자면 마치 함수포인터 리스트를 사용하는 것 같은 느낌을 준다. delegate 가 동작하는 방식을 아래 예를 들어 설명하고 C#의 event 키워드를 delegate 와 함께 사용할 때 어떤 이점이 있는지 알아본다.

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

## Using Delegate with Event Keyword

delegate를 사용할 때 사실 event 키워드는 안 써도 무방하다. 그러면 왜 event 키워드를 함께 사용해야 하는가? 그것은 사용자의 실수를 방지해주는 2가지 기능을 제공하기 때문이다.

한 가지 기능은 등록된 모든 subscriber 들을 지워버릴 수 있는 실수를 방지하기 위함이다. 보통 delegate를 등록하려면 publisher 외부에서 += 연산을 통해 subscriber를 등록한다. 그런데 이 때 = 연산을 사용하게 되면 그 때까지 등록했던 모든 subscriber 들을 날려버리는 결과를 낳게 될 수 있다. event 키워드를 사용하면 delegate를 소유한 클래스 외부에서 = 연산을 사용하지 못하도록 컴파일 에러를 발생시키기 때문에 이러한 실수를 방지할 수 있다.

또 다른 한 가지는 subscriber를 등록하는 쪽에서 delegate의 호출을 방지하기 위함이다. delegate의 호출은 delegate를 소유하고 있는 event publisher에 의해서만 이루어져야 하는데 그렇지 않으면 여기저기서 남발하며 호출될 수 있다. event 키워드를 사용하면 이러한 delegate의 호출이 클래스 외부에서는 막히게 되어 또한 잘못된 사용을 막을 수 있다.

```cs
public class Calculator : MonoBehavior
{
    public delegate float Calculate(float a, float b);

    // Declaring delegate object with 'event' keyword.
    public event Calculate onCalculate;

    private void Start()
    {
        onCalculate(1f, 2f);
    }
}

public class User : MonoBehavior
{
    private void Awake()
    {
        Calculator calc = FindObjectOfType<Calculator>();

        calc.onCalculate += Sum;
        calc.onCalculate += Subtract;

        // Error! assignment is not allowed.
        //calc.onCalculate = Subtract;

        // Error! direct invocation is not allowed.
        //calc.onCalculate(1f, 2f);
    }

    private float Sum(float a, float b)
    {
        Debug.Log(a + b);
        return a + b;
    }

    private float Subtract(float a, float b)
    {
        Debug.Log(a - b);
        return a - b;
    }
}
```
