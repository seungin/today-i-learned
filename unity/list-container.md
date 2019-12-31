# List Container on C#

C#에서 제공하는 List 사용법을 알아본다.

## Basic Usage

```cs
using System.Collections.Generic;

public class ScoreManager
{
    private List<int> scores = new List<int>();

    private void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
			// 아이템 추가
            scores.Add(Random.Range(0, 100));
        }

        if (Input.GetMouseButtonDown(1))
        {
            // 아이템 삭제
            // ; index를 이용하여 삭제할 때는 RemoveAt 함수를 이용
            // ; value를 이용하여 삭제할 때는 Remove 함수를 이용, 중복이 있더라도 처음 발견된 하나만 삭제
            scores.RemoveAt(3);
            scores.Remove(60);

            // 모든 아이템을 삭제하는 구현을 이렇게 할 수도 있다.
            while (scores.Count > 0)
            {
                scores.RemoveAt(0)
            }
        }
    }
}
```
