# Introduction to roscore

roscore는 ROS를 기반으로 하는 시스템의 필수적인 몇 가지 노드들의 모음이다. ROS 노드들의 통신을 위해서는 반드시 roscore가 먼저 실행되고 있어야 한다. roscore는 크게 3가지로 구성된다.

- ROS `Master`
- ROS `Parameter Server`
- ROS logging node: `rosout`

## Master

ROS Master의 역할은 각 개별 노드들의 위치를 정확히 찾아주는 것이다. 노드끼리 한번 위치를 찾아내면 그 다음부터는 peer-to-peer로 통신할 수 있게 된다. ROS Master는 XMLRPC 기반의 API를 제공하는데, roscpp나 rospy가 바로 이 API를 구현한 client 라이브러리이다. API를 이용하여 Master로 요청하거나 정보를 얻어 각 언어별로 노드의 구현을 가능하게 한다. 다음 그림은 Master 역할을 단순화시킨 모델이다.

![Image Not Found](http://wiki.ros.org/Master?action=AttachFile&do=get&target=ROS_master_example_english_1.png)
![Image Not Found](http://wiki.ros.org/Master?action=AttachFile&do=get&target=ROS_master_example_english_2.png)
![Image Not Found](http://wiki.ros.org/Master?action=AttachFile&do=get&target=ROS_master_example_english_3.png)

## Parameter Server

ROS Parameter Server는 노드 세계에서 공유되는 다양한 타입의 dictionary 집합으로, network API를 통해 접근 가능하다. 노드는 이 서버를 이용하여 runtime에 파라미터들을 저장하거나 얻을 수 있는데, 마찬가지로 XMLRPC를 통해 접근할 수 있고 ROS Master 안에서 함께 구동된다.

## rosout

rosout은 ROS 안에서 console log를 담당하는 노드의 이름이다. 다른 노드들은 console 출력을 위해서 rosout으로 message를 publish하는데, 노드 안에서 흔히 print를 위해 쓰는 ROS_INFO가 대표적인 예이다. console 출력을 위해서 5가지 Verbosity Level(DEBUG, INFO, WARN, ERROR, FATAL)을 가지고 있다.
