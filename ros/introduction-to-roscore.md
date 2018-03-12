# Introduction to roscore

roscore는 ROS를 기반으로 하는 시스템의 필수적인 몇 가지 노드들의 모음이다. ROS 노드들의 통신을 위해서는 반드시 roscore가 먼저 실행되고 있어야 한다. roscore는 크게 3가지로 구성된다.

- ROS Master
- ROS Parameter Server
- ROS logging node: rosout

## Master

ROS Master의 역할은 각 개별 노드들의 위치를 정확히 찾아내는 것을 가능하게 한다. 노드끼리 한번 위치를 찾아내면 그 다음부터는 peer-to-peer로 통신할 수 있게 된다. ROS Master는 XMLRPC 기반의 API를 제공하는데, roscpp나 rospy가 바로 이 API를 구현한 client 라이브러리이다. API를 이용하여 Master로 요청하거나 정보를 얻어 각 언어별로 node의 구현을 가능하게 한다. 다음 그림은 Master 역할을 단순화시킨 모델이다.

!(Image Not Found)[http://wiki.ros.org/Master?action=AttachFile&do=get&target=ROS_master_example_english_1.png]
!(Image Not Found)[http://wiki.ros.org/Master?action=AttachFile&do=get&target=ROS_master_example_english_2.png]
!(Image Not Found)[http://wiki.ros.org/Master?action=AttachFile&do=get&target=ROS_master_example_english_3.png]
