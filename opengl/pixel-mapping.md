# Pixel mapping

Pixel map을 이용하여 color를 미리 정의하고 이를 이용하여 texture mapping하는 방법을 설명한다. 

먼저 2x2 checkerboard texture mapping을 하는 아래 코드를 보자. 이 예제는 texture image의 총 4개 pixel을 각 pixel당 R, G, B에 해당하는 3개의 값으로 설정해주고 있다. 이렇게 단순한 예제는 괜찮지만 여러가지 색깔, 또 매우 많은 pixel을 항상 3개의 값으로 나타내야 한다는 것은 여간 불편한 일이 아닐 수 없다.  

```cpp
void texture_mapping() {
  ...
  static const int width = 2;
  static const int height = 2;
  static const float pixels[3 * width * height] = {
    0.0f, 0.0f, 0.0f,
    1.0f, 1.0f, 1.0f,
    1.0f, 1.0f, 1.0f,
    0.0f, 0.0f, 0.0f
  };
  glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_FLOAT, pixels);
  ...
}
```

OpenGL에서는 `Color Index`라는 개념을 이용해서 Color를 미리 등록해두고 index 즉, 하나의 번호를 가지고 색을 지정할 수 있는 유용한 기능이 있다. 다음 사용 예를 보자. 4x4 texture image에 5개의 color를 pixel별로 번갈아가며 입힌다. 이 때 pixel에 저장되는 값이 color index이며, 3개의 값이 아닌 단 하나의 unsigbed byte 값을 이용한다.  

```cpp
id texture_mapping() {
  ...
  static const int width = 4;
  static const int height = 4;
  static GLubyte pixels[width * height];
  for (int i = 0; i < width * height; i++) {
    pixels[i] = i % NUM_OF_COLORS;
  };
  glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_COLOR_INDEX, GL_UNSIGNED_BYTE, pixels);
  ...
}
```

이와 같이 color index를 이용하기 위해서는 먼저 pixel mapping 과정을 수행해야 한다. index값은 특정 색을 지정하는 고정값이 아니라 프로그래밍적인 사용자 정의값이다. pixel mapping을 하는 다음 예제 코드를 보자.

```cpp
enum ColorType {
	eCOLOR_RED = 0,
	eCOLOR_GREEN,
	eCOLOR_BLUE,
	eCOLOR_YELLOW,
	eCOLOR_WHITE,

	NUM_OF_COLORS
};

void pixel_mapping() {
	glPixelTransferi(GL_MAP_COLOR, GL_TRUE);

	static const GLsizei MAP_SIZE = 8;
	static_assert((MAP_SIZE & (MAP_SIZE - 1)) == 0, "pixel map size must be power of 2!");
	static_assert(MAP_SIZE >= NUM_OF_COLORS, "pixel map size must be more than or equal to the number of colors!");

	COLORREF colors[NUM_OF_COLORS];
	colors[eCOLOR_RED] = RGB(255, 0, 0);
	colors[eCOLOR_GREEN] = RGB(0, 100, 0);
	colors[eCOLOR_BLUE] = RGB(0, 0, 255);
	colors[eCOLOR_YELLOW] = RGB(255, 255, 0);
	colors[eCOLOR_WHITE] = RGB(255, 255, 255);

	GLushort pixelmap_R[NUM_OF_COLORS];
	GLushort pixelmap_G[NUM_OF_COLORS];
	GLushort pixelmap_B[NUM_OF_COLORS];

	for (int i = 0; i < NUM_OF_COLORS; i++) {
		pixelmap_R[i] = GetRValue(colors[i]) << 8;
		pixelmap_G[i] = GetGValue(colors[i]) << 8;
		pixelmap_B[i] = GetBValue(colors[i]) << 8;
	}

	glPixelMapusv(GL_PIXEL_MAP_I_TO_R, MAP_SIZE, pixelmap_R);
	glPixelMapusv(GL_PIXEL_MAP_I_TO_G, MAP_SIZE, pixelmap_G);
	glPixelMapusv(GL_PIXEL_MAP_I_TO_B, MAP_SIZE, pixelmap_B);
}
```

glPixelTransfer() 함수에 GL_MAP_COLOR 인자를 넣어주고 TRUE값을 주면 pixel mapping을 활성화한다. 여기서는 COLORREF를 이용하여 빨강, 초록, 파랑, 노랑, 하양 5가지 색에 대한 값을 저장하였고 각각의 색을 표현하는 RGB값을 GetRValue(), GetGValue(), GetBValue() 함수를 가지고 뽑아내는 방법을 사용하였다. 저장하는 타입은 GLushort를 사용하였고 우리가 사용할 glPixelMap**usv**() 함수에 적용하기 위해 상위 8비트에 저장되도록 bit 연산하였다. 실제 등록을 수행하는 함수가 glPixelMap() 류의 함수이며 뽑아낸 각 R, G, B별로 index에 등록하고 있다. 

이 때 한 가지 주의할 사항은 glPixelMap() 함수의 인자인 mapsize 값은 반드시 2의 지수승 값을 가져야 하는데, 그렇지 않을 경우 제대로 색이 표현되지 않을 수 있다. 이를 위해서 예제에서는 static_assert를 이용하였고 사용할 color 개수보다 크거나 같고, 2의 지수승 값을 가져야 한다는 것을 명확히 했다. (2의 지수승임을 확인하는 방법을 처음 알았는데 매우 흥미로웠다.)  

