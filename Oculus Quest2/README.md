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