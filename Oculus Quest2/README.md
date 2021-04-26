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