---
layout: default
title: "작업일지::카메라 컨트롤러"
date: 2025-05-09
categories: [salmon]
---

## WASD 입력을 받아 카메라 이동
- 인풋을 입력받아 커맨드화하기 위한 인풋 컨트롤러 추가
- 인풋을 컨슘하고, 카메라를 컨트롤하기 위한 카메라 시스템 추가
- 씬이 로드되고나서 메인 카메라를 찾아와서 세팅
    - 유니티 SceneManager.sceneLoaded 이벤트를 등록하는 형태
    - MonoBehaviour의 OnEnable에서 등록하고, OnDisable에서 해제하면 됨
        - 메인 카메라는 `Camera.main`을 통해 쉽게 가져올 수 있으나, 내부적으로 태그를 통한 게임 오브젝트를 찾기 때문에 캐시해두고 사용
- 카메라를 잘 이동시켜주어 보았다.
    - ![간단한 탑뷰 카메라 이동 구현](/assets/images/camera-controller.gif)
    - 프레스 타임에 따라 카메라 이동 속도 이징 적용하는 것은 이후에 개선 예정
