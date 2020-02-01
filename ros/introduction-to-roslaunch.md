# Introduction to roslaunch

roslaunch는 ROS에서 여러 개의 노드를 한꺼번에 실행하고 종료하는 것을 도와주는 subprocessing 도구이다. ROS1에서는 roscore가 반드시 먼저 실행되어야 하기에 roscore가 미리 실행되어 있지 않으면 먼저 roscore를 실행하는 것까지 대신해준다.

- ROSLaunch Parent
  * Process Monitor
  * XML-RPC Server
  * ROSLaunch Runner

- ROSLaunch Runner
  * Launch Master
  * Launch Nodes

- Launch Node
  * Local Process


### ROSLaunch Parent

ROSLaunch는 자체 Parent 노드를 가지고 있다. 여러가지 역할 중 하나는 Process Monitor를 백그라운드로 실행시키고 shutdown 될 때까지 Spinning을 도는 것이다.

### ROSLaunch Runner

ROSLaunch Parent가 Spinning을 돌기 전에 ROSLaunch Runner를 생성하여 *.launch 파일에 정의된 모든 노드를 차례대로 실행시킨다. 이 때 만약 rosmaster가 실행되어 있지 않으면 가장 먼저 rosmaster를 실행하고 이후에 다른 노드들을 실행한다.

### Local Process

각 노드를 실행할 때는 Python subprocess 모듈의 Popen 함수를 이용한다.
