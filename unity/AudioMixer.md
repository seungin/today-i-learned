# Audio Mixer

사운드 출력을 믹싱하여 최종 사운드를 만드는 기능을 제공하는 Audio Mixer에 대해 설명한다.

## How to use Audio Mixer

아래 설명은 Audio Mixer 는 구성하는 한 예를 보여준다.

- `Windows` → `Audio` → `Audio Mixer` 를 선택한다.
- Audio Mixer 창이 뜨면 Mixers 탭의 + 버튼을 눌러 `Audio Listener` 를 생성한다.
- 생성된 Listener를 선택하고 Groups 탭의 + 버튼을 눌러 Master 하위에 `Audio Mixer Group` 을 2개 생성한다.
- 하나는 BGM, 다른 하나는 SFX 라고 이름 짓는다.

## Interaction between Audio Mixer Group

예를 들어, SFX 사운드 효과가 출력될 때 BGM 음악을 줄이려면 Audio Mixer Group 사이에 상호 작용이 필요하다.

- BGM을 선택하고 Inspection 창에서 `Add Effect` 버튼을 눌러 `Duck Volume` 을 선택한다.
- SFX를 선택하고 Inspection 창에서 `Add Effect` 버튼을 눌러 `Send` 을 선택한다. Receive 에 `BGM\Duck Volume` 을 선택한다.

## Set Audio Output for Audio Sources

Inspection 창에서 `Audio Source` 를 확인하면 Output 항목이 있다. 여기에서 Select 버튼을 클릭하면 `Audio Mixer Group` 을 선택할 수 있다. SFX를 선택해주자. 다른 Audio Source 를 열어 BGM을 선택해주자. 선택을 완료하면 해당 SFX가 선택된 Audio Source 가 발동되었을 때 BGM이 선택된 Audio Source의 음량이 줄어드는 것을 확인할 수 있다. 즉 이것으로 확인할 수 있는 것은, Audio Source의 출력이 Audio Mixer Group을 통해서 나가게 됨으로써 Group 간 시나리오에 따른 출력 제어 등의 Mixing 효과를 줄 수 있게 되는 것이다.
