# OpenGL graphic pipeline

OpenGL을 제대로 활용하기 위해서는 파이프라인과 관련된 필요한 지식이 많이 있어야 한다. 지금은 간단히 어떤 단계들을 거치는지 정도로 간단히 넘어갈 것이므로 이해가 되지 않는다고 하더라도 괜찮다. 추후 하나씩 자세히 살펴볼 것이다.  

## Vertex shader

`Vertex shader`는 OpenGL 파이프라인의 첫번째 프로그래밍 가능 stage이며 유일한 필수 stage이다. 하지만 그 전에 `Vertex fetching` 또는 `Vertex pooling` 이라는 고정 함수 stage가 실행될 수 있는데, 이 작업은 Vertex shader에 입력을 제공하는 일을 수행한다. 제공된 입력은 Vertex shader의 `in` 키워드로 선언되는 `Vertex attribute`에 자동으로 채워진다.  

glVertexAttrib4fv() 함수의 첫번째 인자 값과 동일한 location 값을 가지는 Vertex attribute에 그 값이 자동으로 채워진다.  

```cpp
GLfloat attrib[] = { ... };
glVertexAttrib4fv(0, attrib);
```

```glsl
layout (location = 0) in vec4 offset;
```

## Data trasfer between stages

stage 간 데이터 전달은 `in`, `out` 키워드를 가진 입출력 변수를 통해 가능하다. 전달되는 경로는 정해진 파이프라인 경로를 따른다. 보내주는 쪽에서는 out 변수를 선언하고 받는 쪽에서는 in 변수를 선언하는데 보통 변수명을 동일하게 가져가는 것 같다. 예를 들면 아래 코드는 Vertex shader에서 Fragment shader로 vs_color 변수를 전달하는 코드이다. (무관한 코드는 모두 생략했다)  

```glsl
layout (location = 1) in vec4 color;

out vec4 vs_color;

void main() {
    vs_color = color;
}
```

```glsl
in vec4 vs_color;
out vec4 color;

void main() {
    color = vs_color;
}
```

`Interface Block` 이라는 것으로 전달할 데이터를 구조체처럼 묶을 수도 있다는 것을 알아두면 유용할 것이다. 사용법은 필요할 때 구체적으로 파보도록 하자.  

## Tessellation

`Tessellation`, 번역하면 `조각화` 라는 뜻으로 고차 primitive를 더 작고 단순한 여러 개의 렌더링 가능한 primitive, 예를 들면 삼각형과 같은 것으로 분할하는 작업이다. Tessellation 단계는 Vertex shading stage 바로 다음에 위치하며, `Tessellation control shader`, `Tessellation engine`, `Tessellation evaluation shader`의 세 부분으로 구성된다. 각각에 대한 상세한 내용은 추후 다루도록 하자.  

## Geometry shader

`Geometry shader`는 primitive 당 한 번 수행되며 primitive를 구성하는 모든 vertex에 대한 입력 vertex 데이터에 접근할 수 있다. 또한 프로그래밍을 통해 파이프라인 내 데이터 흐름의 양을 증감시킬 수 있는 유일한 shader stage이다. 또 다른 고유한 기능으로 파이프라인 중간에 primitive 모드를 변경하는 기능도 있다. 예를 들면 삼각형을 입력으로 받아 점이나 선으로 출력한다든지, 반대로 개별 점들을 입력받아 삼각형을 생성할 수 있다. 이 부분도 상세 내용은 추후 다루도록 하자.  

## Primitive assembly, Clipping, Rasterization

용어들만 간단히 정리하고 가자. `Primitive assembly`는 vertex들을 선이나 삼각형으로 그룹화하는 단계를 말하며, `Clipping`은 보이는 영역에 대해서 공간을 잘라내는 것을 말한다. Clipping 후에 얻게되는 정규화된 디바이스 좌표를 윈도우 영역으로 넣는 작업을 `Viewport transformation`이라고 하며, 삼각형의 방향을 결정하여 이를 토대로 그릴지 말지를 결정하게 하는 단계는 `Culling`이라고 한다. `Rasterization`은 어떤 fragment들이 primitive에 의해 채워지는지를 결정하는 작업으로, 예를 들면 어떤 한 점이 삼각형 안쪽에 있는지 바깥쪽에 있는지를 보고 색을 채울지 결정하는 것을 말한다.  

## Fragment shader

`Fragment shader`는 마지막 프로그래밍 가능 stage로써 각 fragment들의 색상을 결정하여 나중에 프레임버퍼로 보내져 최종 윈도우에 그려지도록 한다. 여기서 파이프라인의 작업량이 폭발적으로 증가하는데, 그 이유는 각 삼각형이 수백, 수천, 심지어 수백만 개의 fragment를 생산해내기 때문이다.  

## Frame buffer

`Frame buffer`는 파이프라인의 마지막 스테이지이다. 화면에 보이는 영역을 나타내는데 색상 이외에도 픽셀당 값을 저장하기 위한 여러 추가적인 메모리 영역에 해당한다. 이러한 여러가지 상태들은 `Frame buffer object`에 저장되는데, Frame buffer object에 저장되지 않는 `Pixel operation` 상태라는 것도 있다. 이 개념은 추후에 보도록 하자.  

## Compute shader

`Compute shader`는 OpenGL이 제공하는, 다른 그래픽스 관련 stage와는 별개로 동작하는 파이프라인이다. 이것은 그래픽스 프로세서의 계산 파워를 사용하는 방법 중 하나인데 유용하게 사용하기 위해서는 OpenGL에 대해 더 알아본 후에 다루도록 하자.  
