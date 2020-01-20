# Singleton

유니티에서 Manager를 다룰 때 Singleton 패턴을 매우 자주 사용한다.

## Requirements

Scene 상에 존재하는 유일한 Manager를 어디서든 가져와서 사용해야 한다.

```cs
public class ManagerUser : MonoBehaviour
{
    private void Update()
    {
        Manager.GetInstance().SomeMethod();
    }
}
```

```cs
private static Manager instance;

public static Manager GetInstance()
{
    if (instance == null)
    {
        instance = FindObjectOfType<Manager>();
    }

    return instance;
}
```

만약 Scene 상에 Manager 게임 오브젝트가 존재하지 않으면 생성해야 한다.

```cs
public static Manager GetInstance()
{
    if (instance == null)
    {
        instance = FindObjectOfType<Manager>();

        if (instance == null)
        {
            GameObject container = new GameObject("Manager");
            instance = container.AddComponent<Manager>();
        }
    }

    return instance;
}
```

만약 Scene 상에 Manager 게임 오브젝트가 여러 개 존재하면 단 하나만 남기고 삭제해야 한다.

```cs
private void Start()
{
    GetInstance();

    if (instance != null)
    {
        if (instance != this)
        {
            Destroy(gameObject);
        }
    }
}
```

## Full Code

위 조건을 만족하는 Singleton 클래스의 예를 아래와 같이 구현할 수 있다.

```cs
public class Manager : MonoBehaviour
{
    private static Manager instance;
    
    public static Manager GetInstance()
    {
        if (instance == null)
        {
            instance = FindObjectOfType<Manager>();

            if (instance == null)
            {
                GameObject container = new GameObject("Manager");
                instance = container.AddComponent<Manager>();
            }
        }

        return instance;
    }

    private void Start()
    {
        GetInstance();

        if (instance != null)
        {
            if (instance != this)
            {
                Destroy(gameObject);
            }
        }
    }
}
```
