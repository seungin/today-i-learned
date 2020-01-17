# Unity Event

어떤 조건이 발동되었을 때 처리해야 하는 일들이 많아질 때 상호의존성이 높아지는 단점을 해결하는 유니티의 좋은 기능으로 Unity Event 가 있다. 이에 대해 설명한다.

## If Not Using Unity Event

Unity Event 기능을 사용하지 않으면 Player 가 죽었을 때 관련된 기능 수행을 위해 필요한 모든 객체를 Reference로 가져와야 한다. 그 예시는 아래와 같다. Player 는 죽었을 때의 처리를 위해 GameManager, UiManager, ChallengeSystem 을 모두 참조할 수 밖에 없다.

```cs
public class Player : MonoBehavior
{
    public GameManager gameManager;
    public UiManager uiManager;
    public ChallengeSystem challengeSystem;

    private void Dead()
    {
        gameManager.dead();
        uiManager.dead();
        challengeSystem.dead();

        Destroy(gameObject);
    }

    private void OnTriggerEnter(Collider other)
    {
        Dead();
    }
}
```

## With Unity Event

Unity Event 를 사용하면 Player는 GameManager, UiManager, ChallengeSystem, 그리고 나중에 추가될 그 어떤 것도 참조할 필요가 없다. 그저 Player 가 죽었다는 것을 나타내는 이벤트인 onPlayerDead 만 호출해주면 끝이다.

실제 Event가 발동되었을 때 수행할 처리를 추가하고 싶으면 Inspector 창에서 해당 Event 에 대상 객체와 그 객체의 함수를 연결시켜주는 것으로 가능하다.

```cs
using UnityEngine.Events;

public class Player : MonoBehavior
{
    public UnityEvent onPlayerDead;

    private void Dead()
    {
        onPlayerDead.Invoke();

        Destroy(gameObject);
    }

    private void OnTriggerEnter(Collider other)
    {
        Dead();
    }
}
```
