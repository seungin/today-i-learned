# PlayerPrefs

유니티에서는 Registry 같은 개념으로 PlayerPrefs 라는 기능을 제공한다. 이 기능은 Key-Value Pair를 이용하여 마치 간단한 DB처럼 동작하는데, 그래서 프로그램을 재시작해도 그 값이 유지된다.

## GetValue and SetValue

아래 예제에서는 GetBestScore 함수에서 저장된 값을 가져오고 UpdateBestScore 함수에서는 새로운 값을 저장한다.

```cs
private int score;

public void AddScore(int value)
{
    score += value;
    UpdateBestScore();
}

private void UpdateBestScore()
{
    if (score > GetBestScore())
    {
        PlayerPrefs.SetInt("BestScore", score);
    }
}

private int GetBestScore()
{
    return PlayerPrefs.GetInt("BestScore");
}
```
