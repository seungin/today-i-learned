# Action

유니티의 Action 개념을 설명한다.

# Action is void delegate

Action은 in/out parameter가 void인 delegate 이다. 자주 사용되는 delegate 패턴이므로 유니티에서 미리 구현하여 제공하는 클래스인 것이다. Action을 사용하려면 System 네임스페이스를 추가해야 한다.

그러므로 delegate와 마찬가지로 event 키워드 사용 유무에 따른 효과도 동일하다.

```cs
using System;

public class Worker : MonoBehaviour
{
	public event Action work;

    // same as below:
    //public delegate void Work();
    //public event Work work;

	private void Start()
	{
		work();
	}
}
```
