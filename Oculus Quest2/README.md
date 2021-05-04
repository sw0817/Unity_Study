# Oculus Quest 2용 Unity 앱 개발일지



## VR 게임 개발

> Oculus Quest 2 환경설정

- https://www.oculus.com/setup/ 에 접속해 `Oculus Link` 소프트웨어를 다운로드 받는다.
- 오큘러스 퀘스트 2 하드웨어와 PC를 연결한다.
- 미러링 중인 핸드폰 오큘러스 앱에서 오큘러스를 개발자 모드로 전환한다.
- 전환시 부여받은 Pin code를 오큘러스 VR 기기 내에서 입력한다.
- PC에서 Side Quest 어플리케이션을 설치한다.
- 개발자모드로 전환된 오큘러스 퀘스트 2를 PC와 연결하면, VR 환경 내에서 해당 PC에서 Side Quest를 사용할 것인지 권한을 설정할 수 있다.
- 권한 설정을 하고 나면 정상적으로 Side Quest(PC)와 오큘러스 퀘스트2 (VR 하드웨어)가 연결되어 APK파일을 등록 및 삭제할 수 있다.



> Oculus Quest 2 Tilt Brush 연동하기

- 오큘러스 퀘스트 2 하드웨어와 PC를 연결한다.
- 이 상태로 Tilt Brush Open Source 유니티 앱을 실행한 뒤, 기본 Standalone 플랫폼의 Player Setting, Other Setting 에서 Scripting Define Symbols 에 `OCULUS_SUPPORTED`를 추가한다. 그 뒤, Android 플랫폼으로 Switch 하여 다시 같은 작업을 진행한다.
- Package Manager에서 Oculus desktop package를 설치한다.
- Unity Oculus plugin을 upgrage한다는 메시지가 뜨면 Accept한다.
  - 이 메시지가 뜨지 않는다면 유니티 프로젝트를 재실행하여 메시지를 확인한다.
- 여기까지 완료하면, Unity window 창의 Tilt > Build > SDK : OVR, Tilt > Build > Platform : Android, Tilt > Build > Runtime : Mono 를 설정할 수 있게된다.
- Tilt > Build > Do Build를 통해 apk파일을 생성할 수 있으나, 현재 2019.4 버전에서는 실패했다. 2018 버전과 2020 버전에서의 확인이 필요하다.
  - 2020 버전은 불가능, 2019 버전 성공. (2021.04.26)



> Open Brush

- Tilt Brush를 Oculus Quest2 에서 실행하기 위해 APK파일로 빌드하여 적용 시 사용자의 시선을 가리는 Canvas가 생성되고, 이미지가 깨지는 오류를 겪어 해결하기 위해 대안으로 사용한 Open Source 
- https://github.com/icosa-gallery/open-brush 에 접속해 `Tilt brush` 의 클론 버전인 `Open brush`를 clone 했다.
- Tilt brush와 같은 방식으로 Unity를 세팅하지만, Player Settings의 `Color Space` 를 `Gamma`로 세팅한다. (Steam VR 에서 Linear를 권장하지만, 오큘러스 퀘스트 2 용 apk파일 빌드를 위해 Gamma를 선택한다.)
- XR Setting 의 Virtual Reality SDKs의 Oculus에서 Low Overhead Mode와 V2 Signing (Quest)를 체크한다.
- 빌드 시 Tilt Brush APK 파일에서 발생하던 플레이어의 시야를 가리는 Canvas 오류를 해결할 수 있다.
- Unity는 `File` -> `Build Settings` 메뉴에서 기본 build를 할 수 있지만, `Open brush`는 독자적인 build 방식을 제공한다.
- 상단 메뉴의 `Tilt` -> `Do Build` 기능을 사용해 build를 한다.
- 이 경우 Open brush를 처음 clone한 상태에서 주어지는 Loading scene과 Main scene이 build 되는데, 다른 scene을 추가하거나 Loading scene과 Main scene을 제거한 상태에서 build를 해도 두 scene이 build 되는 현상이 있었다.
- `File` -> `Build Settings` 메뉴에서 기본 build를 사용하는 경우, 원하는 scene이 build되었지만, Open brush에서 제공하는 brush가 포함된 scene에서 화면이 제대로 출력되지 않고, VR 기기를 탐색하지 못하는 error가 발생하였다.
- `Do Build` 기능은 Open brush에서 새롭게 만든 기능이고, 상단 메뉴탭은 `Editor` 내 script에서 관리하기에 해당 경로의 script들을 분석해, `BuildTiltBrush.cs` script에서 Loading scene과 Main scene을 선택하여 build하는 코드를 찾을 수 있었다.
- 해당 코드에 새롭게 추가한 scene을 추가해 `Do Build` 기능을 사용하는 것으로, 해당 error는 fix되었다.



> Oculus Player

- Oculus Integration을 Import 하여 비교적 쉽게 구현할 수 있다.
- OVR Player를 사용한다.
  - 이 때, 기본적으로 CharacterController와 OVRCameraRig가 생성되는데, Build 후 이동 시 카메라만 움직이고 Character는 움직이지 않는다.
  - 이를 해결하기 위해, 최상단 오브젝트에 Character Camera Constraint 스크립트를 넣고, Camera Rig를 등록할 수 있다.
- Tracking Space에서는 카메라로서 CenterEyeAnchor를 사용하게 되고, OVRCameraRig에서 Tracking Origin Type을 Floor Level로 설정해 눈 높이를 맞출 수 있다.
- 사용자는 VR 환경에서 직접 회전할 수 있지만, 제자리 회전 기능 역시 필요해 구현되어있다.
  - 회전 방식으로는 해당 버튼 클릭 시 45도 회전하는 Snap Rotation 방식과, 실제적인 회전 방식인 Enable Rotation 방식이 있다.
  - 기본 세팅은 Snap Rotation 방식이고, 이를 수정하기 위해 OVR Player Controller 스크립트 public 변수 창의 Snap Rotation을 체크 해제 후 Enable Rotation을 체크하여 사용하면 된다.
    - 기본 세팅에서 회전 외에 이동 방식이 전후좌우 방향으로 모두 가능하나, Enable Rotation 방식의 회전을 사용할 때 직진을 의도해도 옆으로 움직이는 불편함이 있어 스크립트 내에서 좌우 이동을 없애주고 사용하도록 했다.
- LeftHandAnchor / LeftControllerAnchor 및 RightHandAnchor / RightControllerAnchor라는 오브젝트가 오큘러스 퀘스트의 컨트롤러 위치를 인식한다.
  - 해당 위치에 컨트롤러 혹은 손 모양의 오브젝트를 추가할 수 있고, 손 모양이 보이게 하는 방법은 LocalAvatar와 그 아래 OVRHandPrefab을 추가하는 방법, 그리고 LeftHandAnchor, RightHandAnchor 아래에 각각 CostomHandLeft, CostomHandRight를 추가하는 방법이 있다.
  - CostomHandLeft, CostomHandRight는 Runtime에서 Render를 해제하게 되어있어, 강제로 다시 enable 시켜줘야 렌더링이 된다.
    - 손으로 물체를 잡기 위해서 물체에 OVR Grabbable 스크립트를 추가하고, 해당 물체를 RigidBody로 설정한다.
    - Use Gravity와 Is Kinematic을 체크하고, 손으로 잡혀 있는 동안은 각각 false, true 값을, 놓은 뒤에는 각각 true, false/true 값을 가지게 한다.



> HandMotion Trigger

- `Oculus Integration`의 `Custom Hand`를 사용해 물건 집기, 던지기 외 다양한 상호작용 기능을 구현하였다.
- Oculus Quest2의 컨트롤러의 기본 버튼(button 0 ~ 3) 을 클릭해, 손바닥 위에 메뉴가 토글되도록 구현하였다.
- 메뉴에는 몇 가지 버튼이 나타난다.
  - 오른쪽 컨트롤러의 중지 버튼을 누르고 있으면, 검지만 핀 상태가 되는데, 이 상태에서 버튼을 가리키면, 버튼의 설명과 함께 메뉴 위로 올라오는 인터랙션을 구현하였다.
    - 기능을 구현하기 위해 IndexFinger로부터 발사되는 Raycast를 구현하였다. OVRInput.Get(OVRInput.Axis1D.SecondaryHandTrigger) 값을 통해 중지 버튼의 토글 상태를 확인하고, ray를 쏘아 button만이 가지고 있는 script를 발견했을 때만 유효한 함수를 부르도록 설계하였다.
    - 버튼은 비활성화 상태, 미리보기 상태, 활성화 상태로 나뉘어, 각 단계의 값을 ray가 읽어 각각 함수를 수행하도록 하였다.
  - 미리보기 상태의 버튼을 IndexFingerTip으로 누르면, 버튼은 활성화가 된다.
    - IndexFingerTip과 button의 충돌 여부를 판단해야하고, 실제 컨트롤러를 잡고 있는 사용자의 손은 충돌하지 않고 지나칠 수 있기 때문에, `OnColliderEnter` 함수 대신 `OnTriggerEnter` 함수를 사용하여 통과하는 오브젝트의 충돌 시작 여부를 확인해, 버튼을 활성화하도록 하였다.
- 인물과의 상호작용 기능을 추가하였다.
  - 상호작용 가능한 인물을 알아보기 쉽도록, 상호작용 가능한 인물이 플레이어로부터 일정 거리 안으로 들어오면 해당 인물 머리 위로 인물의 이름이 표시되도록 하였다.
  - 이름이 표시된 인물은 상호작용이 가능한 상태라는 의미로 `infoIsActivated` 값을 true로 반환하게되고, 이 상태에서 중지 버튼을 토글하여 ray를 쏘았을 때, focused라는 bool 값을 true로 반환하도록 설계하였다.
  - focused가 true가 되면, 노란색 화살표가 인물 머리 위에 떠 다니게 되어 선택한 상태임을 알기 쉽도록 하였다.
  - focused 상태에서 IndexFinger 버튼을 클릭하게 되면, 인물과 상호작용하게 된다. 



> Walking(Running) Motion

- Player가 앞, 뒤로 걷는 동안 유령처럼 미끄러지는 움직임을 실제와 같이 자연스러운 움직임으로 만들기위해 코드를 작성하였다.

- Oculus Integration을 활용해 구현한 Player는, `OVRPlayerController.cs` 의 `UpdateTransform` 함수에서 현재 보고 있는 카메라의 방향과 위치를 갱신한다.

- 이 갱신을 무시하고 시선의 위치를 바꿀 수 있는 Object는 `Tracking Space`이다.

- OVRPlayerController.cs 스크립트 내 Oculus 컨트롤러의 움직임을 감지하는 `UpdateMovement` 함수 내 `Vector2 primaryAxis = OVRInput.Get(OVRInput.Axis2D.PrimaryThumbstick)` 를 통해 왼쪽 ThumbStick(앞 뒤 움직임 컨트롤러 스틱)의 y축 움직임을 감지한다.
  - 0~1 까지 Stick을 꺾은 정도를 입력받고, 의도된 움직임에서만 걷는 모션을 취하기 위해, Stick을 반 이상 꺾은 0.5이상의 입력값을 받는 경우에만 모션을 적용해준다.

- 모션은 왼발 오른발 차례로 움직이고, 걷는 동안 머리가 위 아래로 움직인다는 가정하에 적용하였다.

  - 한 발의 움직임에서 머리의 위 아래 이동 사이클이 이루어진다.

  - 이 가정에 따라 걷는 모션중에 플레이어의 시선은 좌우와 상하로 동시에 흔들리며, 중앙 -> 좌측 -> 중앙까지의 좌우 흔들림 사이클 시간동안 아래 -> 위 -> 아래의 움직임 사이클이 동시에 이루어진다.

- 일정 거리의 위, 아래, 좌측, 우측 움직임 이후 방향을 반대로 바꿔주도록 설정하였다.

- 뒤로 걷는 중에는 앞으로 걷는 방향인 경우와 반대로 위 아래, 좌우 움직임이 발생한다.

