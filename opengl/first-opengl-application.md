# First OpenGL application

OpenGL에서 화면을 갱신하는 방법을 알아보고 쉐이더를 사용하는 기본적인 방법을 알아보자.  
그리고 쉐이더를 이용해서 대표적인 Primitive인 점, 선, 삼각형을 그려보도록 하자.  
삼각형 그리기는 그래픽스 프로그래밍의 "Hello, World!" 와 같은 것이다.  

예제 코드에서는 sb6 네임스페이스를 제공하여 이를 이용하고 있다.  
sb6::application을 상속받아 어플리케이션 클래스를 만들어 사용할 수 있다.  
제공하는 startup(), shutdown(), render() 함수를 오버라이드하여 사용할 수 있다.  

## Clear Buffer

`glClearBufferfv()` 함수를 이용하면 화면을 갱신할 수 있다.  
여기에서는 하나의 버퍼만 사용하므로 두번째 인자는 index 0으로 설정하였다.  

```cpp
void first_app::render(double currentTime)
{
    const GLfloat red[] = { 1.0f, 0.0f, 0.0f, 1.0f };
    glClearBufferfv(GL_COLOR, 0, red);
}
```

## How to use shaders

OpenGL은 `shader`라고 불리는 여러 작은 프로그램을 고정 함수로 연결시켜 작동한다. 그래픽 프로세서는 작성한 shader를 실행시키고 그 입출력을 파이프라인으로 이동시켜 최종 픽셀을 그린다. 화면에 무언가라도 그리려면 적어도 shader 몇 개는 작성해야 한다.  

OpenGL shader는 `GLSL`(OpenGL Shading Language)로 작성한다. 작성한 shader 소스 코드는 `Shader Object`로 바뀌어 컴파일되고 여러 shader object 들이 하나의 `Program Object`로 링크된다. 웬만한 파이프라인은 적어도 하나의 `Vertex Shader`로 이루어지며 화면에 픽셀을 그리려면 `Fragment Shader`가 필요하다.  

아래 vertex shader 소스 코드를 보자. `#version 430 core`는 GLSL version 4.3을 사용하며 core profile 기능만 사용한다는 것을 의미한다. 일반적인 C언어와 유사하게 main() 함수로 시작하지만 인자가 없는 void 타입이다. 기본적으로 `gl_...로 시작하는 모든 변수`는 OpenGL의 일부이며 shader와 다른 부분들을 연결하는 역할을 한다. 여기 vertex shader에서는 `gl_Position`은 vertex의 출력 위치를 나타낸다. OpenGL의 `Clip Space`의 중앙에 위치하게 된다. Clip space는 OpenGL 파이프라인의 다음 스테이지에서 적용되는 좌표계라는데 그 의미는 아직 불명확하니 추후 알아가보도록 하자.  

```glsl
#version 430 core

void main(void)
{
    gl_Position = vec4(0.0, 0.0, 0.5, 1.0);
}
```

다음은 fragment shader 소스 코드를 보자. `out` 키워드를 사용하여 color를 출력 변수로 선언한다. fragment shader에서는 출력 변수값을 윈도우나 화면으로 보낸다. default로 이 값들이 직접 화면에 전달되며 glClearBufferfv() 함수에서처럼 각각 r, g, b, a에 해당한다. 여기서는 vec4(0.0, 0.8, 1.0, 1.0) 값을 사용했으므로 청록색을 출력하게 된다.  

```glsl
#version 430 core

out vec4 color;

void main(void)
{
    color = vec4(0.0, 0.8, 1.0, 1.0);
}
```

shader가 준비되었으면 이제 컴파일하고 OpenGL에서 실행딜 수 있게 program으로 링크시킬 차례다.  
- glCreateShader() : 빈 shader object를 생성
- glShaderSource() : shader 소스 코드를 shader object로 전달해서 복사본을 유지
- glCompileShader() : shader object에 포함된 소스 코드를 컴파일
- glCreateProgram() : 빈 program object를 생성
- glAttachShader() : shader object를 program object에 부착
- glLinkProgram() : program object에 부착된 모든 shader object를 링크
- glDeleteShader() : shader object를 삭제, 링크가 완료되면 program이 바이너리 코드를 보관하여 shader는 더 이상 필요 없게 됨

화면에 그리기 전에 마지막으로 할 일은 `VAO`(Vertex Array Object)를 생성하는 것이다. 이는 OpenGL 파이프라인에서 `Vertex Fetch` 스테이지를 나타내는 객체로써 입력을 vertex shader에 공급하기 위해 사용된다. 여기 예제에서는 vertex shader가 지금은 입력을 가지고 있지 않으므로 VAO에 대해 할 일은 없다. 하지만 OpenGL이 그릴 때 사용할 수 있도록 VAO를 생성해 주기는 해야 한다. VAO를 생성하는 방법은 아래 두 함수를 호출하는 것이다.
- glGenVertexArrays()
- glBindVertexArray()

program이 준비되었으면 사용하면 된다.
- glUseProgram() : OpenGL에 해당 program object를 사용하여 rendering 시킴
- glDrawArrays() : 인자로 주어진 primitive 타입에 따라 실제 화면에 그림
  - GL_POINTS : 점
  - GL_LINES : 선
  - GL_TRIANGLES : 삼각형

```cpp
GLuint first_app::compile_shaders()
{
    GLuint vertex_shader;
    GLuint fragment_shader;
    GLuint program;

    // Create and compile a vertex shader
    vertex_shader = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertex_shader, 1, vertex_shader_source, NULL);
    glCompileShader(vertex_shader);

    // Create and compile a fragment shader
    fragment_shader = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragment_shader, 1, fragment_shader_source, NULL);
    glCompileShader(fragment_shader);

    // Create a program and then attach, link shaders
    program = glCreateProgram();
    glAttachShader(program, vertex_shader);
    glAttachShader(program, fragment_shader);
    glLinkProgram(program);

    // Delete shaders because the program owns them
    glDeleteShader(vertex_shader);
    glDeleteShader(fragment_shader);

    return program;
}
```

## Draw a point
```glsl
#version 430 core

void main()
{
    gl_Position = vec4(0.0, 0.0, 0.5, 1.0);
}
```

혹시 화면에 점이 너무 작다면 OpenGL에 한 픽셀보다 더 크게 그리도록 할 수 있다.
- glPointSize()

```cpp
glDrawArrays(GL_POINTS, 0, 1);
glPointSize(40.0f);
```

## Draw a line
선이나 삼각형의 경우 `둘 이상의 vertex가 동일한 위치에 있으면 primitive가 취소`된다. 선의 길이나 삼각형의 면적이 0이 되기 때문이다. 이 문제를 해결하려면 vertex shader를 수정하여 각 vertex들이 다른 위치에 그려지게 해야 한다.  
GLSL은 vertex shader에 `gl_VertexID`라는 특별한 입력을 사용할 수 있다. 해당 시점에 사용될 vertex의 index를 의미하는데, glDrawArrays() 의 두번째 인자로 들어간 index부터 시작해서 세번쨰 인자로 들어간 count 개수까지 한 vertex에 대하여 한 번에 하나씩 증가한다. 이 index를 사용하여 각 vertex에 대해 다른 위치를 지정할 수 있다.

```glsl
#version 430 core

void main()
{
    const vec4 vertices[2] = vec4[2](
        vec4(0.25, -0.25, 0.5, 1.0),
        vec4(-0.25, -0.25, 0.5, 1.0));

    gl_Position = vertices[gl_VertexID];
}
```

```cpp
glDrawArrays(GL_LINES, 0, 2);
```

## Draw a triangle
선과 유사하지만 vertex를 3개로 늘렸고 glDrawArrays에 인자로 들어가는 값을 3으로 바꿔주어야 한다.

```glsl
#version 430 core

void main()
{
    const vec4 vertices[3] = vec4[3](
        vec4(0.25, -0.25, 0.5, 1.0),
        vec4(-0.25, -0.25, 0.5, 1.0),
        vec4(0.25, 0.25, 0.5, 1.0));

    gl_Position = vertices[gl_VertexID];
}
```

```cpp
glDrawArrays(GL_TRIANGLES, 0, 3);
```
