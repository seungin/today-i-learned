# Lambda

Lambda 표현식은 익명 함수라고도 불리는데, 일반 객체 즉 값 타입처럼 사용할 수 있다. 기본적인 사용법은 '(', ')' 괄호와 '{', '}' 중괄호를 이용하지만 축약하여 사용할 수도 있다.

```cs
public class Messenger : MonoBehaviour
{
    public delegate void Send(string receiver);

    Send onSend;

    private void Awake()
    {
        onSend += SendMail;
        onSend += SendMoney;

        // abbreviated syntax
        onSend += receiver => Debug.Log("Send Assassin to " + receiver);

        // base syntax
        onSend += (string receiver) => { Debug.Log("Send Bird to " + receiver); };
    }
 
    private void Start()
    {
        onSend("Gaius");
    }

    void SendMail(string receiver)
    {
        Debug.Log("Send Mail to " + receiver);
    }

    void SendMoney(string receiver)
    {
        Debug.Log("Send Money to " + receiver);
    }
}
```

## Class Method as Lambda

클래스의 메서드를 람다로 선언할 수도 있다. 특히 한 두줄 정도의 짧은 구현인 경우에는 코드를 매우 간결하게 표현할 수 있다는 장점이 있다. 예를 들면 아래와 같다.

```cs
private int health = 0;

// Before
public void RestoreHealth(int amount)
{
    health += amount;
}
public bool IsDead()
{
    return health <= 0;
}

// After
public RestoreHealth(int amount) => (health += amount);
public bool IsDead() => (amount <= 0);
```

또한 함수 뿐 아니라 Property에 대해서도 적용 가능하다. 특별히 get 함수만 존재하는 경우에는 더욱 축약된 버전도 가능하다.

```cs
private int health = 0;

// Before
public int Health
{
    get { return health; }
    set { health = value; }
}
public bool IsDead
{
    get { return (health <= 0); }
}

// After
public int Health
{
    get => health;
    set => health = value;
}
public bool IsDead => (health <= 0);
```
