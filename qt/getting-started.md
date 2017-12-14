# Basic Qt

## Hello Qt

Qt를 이용한 hello world 프로그램이다. QApplication 객체는 application 자원을 관리한다. Qt에서는 UI 내에 있는 visual element들을 `widget` 이라고 부르는데 여기에 사용된 QLabel은 많은 widget들 중 하나이다. Application window로 QMainWindow 또는 QDialog를 대부분 사용하기는 하나 어떤 widget이라도 window가 될 수 있다. app.exec() 호출은 Qt에게 application 제어를 넘긴다는 의미이다. 이 함수가 호출되면 프로그램은 event loop로 들어가 사용자 입력 등의 이벤트를 처리한다.

```cc
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    QLabel* label = new QLabel("Hello, Qt!");
    label->show();
    return app.exec();
}
```

## Making connections

생성한 button을 클릭하면 application을 종료하도록 하는 프로그램이다. Qt의 widget은 사용자 액션이나 상태 변화를 알리기 위해 `signal`을 발생(`emit`)시킨다. signal을 `slot`이라는 함수에 연결(connect)하면 signal이 발생했을 때 자동으로 연결된 slot 함수가 실행된다. signal과 slot을 연결하기 위해서는 `QObject::connect()` 함수를 사용한다. 여기에서는 button을 클릭하면 clicked() signal이 발생하고 여기에 연결된 app의 quit() 함수가 호출되어 application이 종료하게 된다.

```cc
#include <QApplication>
#include <QPushButton>

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    QPushButton* button = new QPushButton("Quit");
    QObject::connect(button, SIGNAL(clicked()), &app, SLOT(quit()));
    button->show();
    return app.exec();
}
```

## Laying out widgets

Qt의 `layout`은 window 안에서 widget들의 위치 관계(`geometry`)를 관리한다. 수평과 수직 구성을 위한 `QHBoxLayout`과 `QVBoxLayout`, 그리고 격자 구성을 위한 `QGridLayout`이 있다. 또한 아래 예제에서는 signal과 slot을 이용해서 2개의 widget간 데이터를 동기화시키는 방법이 사용되고 있다.

```cc
#include <QApplication>
#include <QHBoxLayout>
#include <QSlider>
#include <QSpinBox>

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    QWidget* window = new QWidget;
    QHBoxLayout* layout = new QHBoxLayout;
    QSpinBox* spinbox = new QSpinBox;
    QSlider* slider = new QSlider(Qt::Horizontal);

    window->setWindowTitle("Enter your age");
    window->setLayout(layout);
    layout->addWidget(spinbox);
    layout->addWidget(slider);
    spinbox->setRange(0, 130);
    slider->setRange(0, 130);

    QObject::connect(spinbox, SIGNAL(valueChanged(int)), slider, SLOT(setValue(int)));
    QObject::connect(slider, SIGNAL(valueChanged(int)), spinbox, SLOT(setValue(int)));

    spinbox->setValue(35);

    window->show();
    return app.exec();
}
```
