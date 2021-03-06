# Daily report

## ' 3.31\(수\) 공부 및 보고내

* ROS란?

**ROS는 ROS 내부의 패키 ROS msg, ROS service, ROS node등을 통해 로봇을 관리하게 해주는 운영체제이다.**

**ROS의 구성요소로는 msg, node, service 등이있다.**

**ROS node란 정확히는 ROS패키지의 내의 실행파일이다.**

**ROS node는 masternode와 publish, subscribe노드로 이루어지고 master node는 오직 1개이다.**

n**ode간에 msg를 통해 통신하게 되고 msg 통신의 방식은 topic, service, action이 있는데 주로 topic이나 service를 사용한다.**

**topic은 단방향이고 이다**. \(publish node가 메세지를 보내면 subscriber가 받는 방식\)

**service는 양방향이고 동기화방식이다. 서비스 서버와 클라이언트를 통해 통신하며 client의 리퀘스트가 있을때만 server가 응답한다.** 

드론 자율주행 시스템에 우리가 쓸 **하드웨어는 Pixhawk 입니다.** 

**pixhawk가 FC로 사용되며 pixhawk는 PX4, Ardupilot등의 소프트웨어 펌웨어를 지원한다.**

**펌웨어인 PX4나 Ardupilot을 FC에 설치하기 전에 Mission Planner나 QGroundControl등의 소프트웨어를 설치해야한다.** 

**QGC는 Pixhawk에 Firmware를 설치해주는 GCS이다.**

GCS란 지상통제소이다.

**QGroundControl은 MAVLink를 통해 비행체와 통신하며 비행체 설정 및 비행상태등을 표시하는 역할을 한다.**

그럼 mavlink는 뭔가?

  
‌  
mavlink를 알기전에 mavros부터 알아보면서 mavlink를 알아보자.  
  
‌  
**MAVROS는 mavlink를 통해 ROS에서 동작하는 node를 만든다.**  
  
‌  
**Onboard system\(rasp pi\)에 설치되며 fc &lt;-&gt;  onboard &lt;-&gt; gcs의 통신을 담당한다.**  
  
‌  
**MAVROS가 FC와 GCS를 이어주는 다리역할을 하는데 이 다리를 정확히 말하면 MAVROS가 생성한 ROS와 통신이 가능한 node가 그것이다.**  
  
**그 노드사이의 통신을 MAVLink protocol type인 메세지를 통해 통신한다.**

정리하자면 FC\(Pixhawk\) &lt;-mavlink type message-&gt; Onboard\(Rasp pi\) &lt;- mavlink type message -&gt; GCS

실제 드론 운용은 위와같은 system으로 이루어지고, FC가 없을때 시뮬레이팅 하는 방법은 **다음 두 가지가 있다. \(매번 드론을 날릴수 없으니 시뮬레이팅을하는데 \)**

**SITL은 FC없이 드론의 비행을 가상 시뮬레이터인 가제보 안의 가상환경에서 드론을 비행시키며 PX4 소프트웨어만으로 비행 시뮬레이팅하는 방식이다.**

**SITL하면 컴퓨터만으로 시뮬레이션**

**HITL이란 FC를 시뮬레이터\(Gazebo\)에 연결하여 시뮬레이션 하는 방식으로  FC에서는 PX4가 작동한다.**

**HITL은 FC\(Pixhawk\)를 컴퓨터에 연결해서 시뮬레이션**    

\*\*\*\*

**MAVLINk 안에서 드론의 비행정보들, 그리고 비행을 제어하기위한 신호들 protocol** 







FC\(Pixhawk with PX4 installed\) &lt;-&gt; Onboard\(Rapsberry Pi\) &lt;-&gt; GCS\(QGroundControl\) == desektop??



## ' 4.1\(목\) 공부 및 보고내용



* 오늘 공부할거 : MAVLINk 안에서 드론의 비행정보들, 그리고 비행을 제어하기위한 신호 protocol들

MAVLink에는 두가지의 메세지 통신모드가 있는데 Topic과 Point to Point이다. Topic은 메세지의 타겟 시스템과 component ID를 보내지 않고 여러 Subscriber가 이 메세지를 받을 수 있다. PTP모드에서는 타겟 ID와 타겟 타겟 컴퍼넌트를 사용한다. 대부분의 경우 위 필드\(타겟 ID, 타겟 컴퍼넌트\)를 사용하고 이 방식은 메세지의 전송을 보장한다.

Topic모드를 제외하고 각 MAvlink메세지는 System ID와 Component ID필드를 포함한다. 추가로 SET\_POSITION\_TARGET\_GLOBAL\_INT를 포함한 몇몇 메세지는 target\_system과 target\_component 필드를 포함하는데 어떤 system이나 component가 명령어를 실행해야 하는지 알려준다.

비행제어를 위한 명령어로는 SET\_POSITION\_TARGET\_LOCAL\_NED, SET\_POSITION\_TARGET\_LOCAL\_NED, SET\_POSITION\_GLOBAL\_INT가있다. MAVLink의 명령어이고 이 명령어를 기체가 받는다. 보통 이 명령어는 GCS나 Compuer에서 발신된다.

* + 원래 하던공부 복습, 마브링크 \(피치 롤 요\)

드 론 조종기는 2가지 모드가있는데 과거에는 모드1을 더 많이썼다고 한다.\(일본식\)

모드2기준으로 설명하겠습니다.

모드1 : throttle이 오른쪽 모드2 : throttle이 왼쪽

피치 : 드론의 X축으로 드론의 전후진을 담당하는 축이다.. 피치축을 기준으로 앞으로 기울어지면 전진을하게되고 뒤로 기울어지면 후진하게된다  

롤 : 드론의 Y축으로 드론의 우진, 좌진을 담당하는 축이다.. 롤축을 기준으로 왼쪽으로 기울어지며 좌진, 오른쪽으로 기울어지면 우진하게된다. 

요 : 드론의 Z축으로 드론의 회전을 담당하는 축이다. 왼쪽으로 회전하려면 오른쪽에 있는 모터의 출력을 높이고 오른쪽으로 회전하려면 왼쪽에있는 모터의 출력을 높이면 된다.  

## ' 4.2\(금\) 보고 내

오늘 한거 : 3D프린터 공부, 홈페이지 이미지  찾기 \(객체인식, 객체추적, 군집화, 탐지비행, 회피비행, 유도비행\)

3D프린터 - 소프트웨어 : ideamaker\(슬라이싱 프로그램이다.\)

.stl, .obj등의 3D 모델링 파일을 ideamaker로 열면 출력을 위한 서포트\(라프트, 브림\)등을 설정한 후 슬라이싱하여 출력하게 해준다. 슬라이싱 내부 명령어는 GCODE 형태이다.



3D프린터의 하드웨어적 분류

: TP에 있는 Pro2 plus는 FFF방식이고 헤드와 필라멘트가 결합된채로 공급된다. 따라서필라멘트를 교체하기가 쉽다. 필라멘트는 익스트루더라는 기계적 장치에 들어가게되고 익스트루더는 coldend와 hotend로 나뉜다. 익스트루더는 필라멘트를 압출하는 기능을 한다.

필라멘트 종류는 매우많으며 주로 PLA나 ABS를 사용하며 그중에서도 가장 대중적인것이 PLA이다.

PLA는 합성플라스틱 소재의 레진이며 PLA 필라멘트를 출력할때는 노즐온도를 210~220도정도로 설정한다.



## '4.3 공부한것들 / 보고내용

### 공부한것들 

SolidWorks 사용법 - 스케치 -&gt; 돌출 -&gt; 모델링 

solidworks 체험판 설치하고 사용법 강의보고 간단한 스케치부터 돌출까지 예습해



### 보고내용 

PX4 자율주행 시스템에 대해서 찾아보기 위해 PX4 Autopilot홈페이지를 살펴보다가 관련 프로젝트를 살펴보니 pixhawk, ROS, QGroundControl, MAVLink등이 있고 이외에 처음보는 프로젝트인 MAVSDK, UAVCAN등이 있엇다. 

UAVCAN이란?

호스트 컴퓨터의 도움없이 컨트롤러와 디바이스간 통신이 가능하도록 설계된 컴퍼넌트 연결 표준을 말한다.\(프로토콜이다\) 메세지기반 프로토콜로서 자동차 내부 전기배선 설계에서 출발했으며 다른영역에서도 쓰인다. Public/Subscribe 또는 Request/Request 교환 방식이고 사이즈가 큰 data를 효율적으로 교환한다. 프로토콜을 구현하기 쉽다는 특징이 있다.

UAVCAN의 핵심 데이터 교환방식은 직렬화시킨 자료구조를 CAN bus를 통해 다른 node로 전송하는 '메세지 브로드캐스팅'이다.



MAVSDK란 FC\(Pixhawk\)와 통신하는 MAVROS와같은 역할을 하는 API이며 

MAVROS 또는 MAVSDK중하나를 택한다.

MAVROS와 동일하게 MAVLink 를 사용하여 FC와 통신한다. 

UAVCAN, MAVSDK두가지 프로젝트 모두 대중적으로 쓰이지는 않는것같아

이런것들이 있구나 정도의 학습만 하였다.



\(민제, 우성이형이 하던 가제보로 시뮬돌리면서 Drone의 변속시 디테일한 제어\(멈추거나 진동, 앞으로갔다가 뒤로오는것 등의 오류 제거\) 를 위한 시뮬 및 코드수정 - 최종\)





‌  
  
‌  
















 

## 

