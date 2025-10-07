---
layout: default
title: "유니티::프로젝트 생성하기"
date: 2025-03-06
categories: [unity]
---

# 2D 프로젝트
## 유니티 프로젝트 생성
- core template에서 두 가지 옵션이 있다.
    - 2D(Built-In Render Pipeline)
    - Universal 2D
- 빌트인은 가장 심플하고 기본적인 렌더 파이프라인을 지원하고(장점: 안정적), Universeal 2D(URP)는 라이팅이나 커스텀 쉐이더 등을 지원한다고 한다.
- 어떤 걸 선택해야 할까?(gpt는 최고다)
    - ✅ Built-in Render Pipeline을 선택해야 하는 경우
        - 기존 프로젝트와의 호환성이 중요한 경우
        - 플러그인 사용이 많아서 URP와의 충돌이 걱정되는 경우
        - 심플한 2D 게임을 만들고, 추가적인 그래픽 기능이 필요 없는 경우
    - ✅ 2D URP를 선택해야 하는 경우
        - 최신 2D 라이팅 시스템을 사용하고 싶은 경우
        - 쉐이더 그래프를 활용한 커스텀 이펙트를 적용하고 싶은 경우
        - 모바일/저사양 기기에서도 최적화된 성능을 원할 경우
        - 새로운 프로젝트를 시작하는 경우 (향후 유니티에서 URP를 계속 발전시킬 가능성이 높음)

# 라이더 프로젝트 연결
- Unity에서 Rider 설정
    1. [Edit] > [Preferences]
    2. [External Tools]에서 External Script Editor를 JetBrains Rider로 변경
    3. [Generate .csproj files for] 항목의 체크 설정
        - Embedded packages
        - Local packages
        - Registry packages

# 탑뷰 카메라로 프로젝트 생성
- 카메라 오브젝트 인스펙터 값 설정
    - Camera.Projection = Orthographic
        - 탑 다운으로 볼 때 왜곡을 없애기 위함    
    - Camera.Size = 보이는 화면의 절반 높이(유닛 기준)
    - Transform
        - 직교로 내려다보는 세팅
            - Position = (0, 10, 0)
            - Rotation = (90, 0, 0)
        - 45도 비틀어서 내려다보는 세팅        
            - Position = (0, 10, 0)
            - Rotation = (30, 45, 0)

# 셀과 디버깅용 라인 렌더
- 디버깅 기능이 필요할 때 구현 예정
- Gizmo를 이용해서 하는 방법이 나을 것 같음
    - 안그러면 라인별로 게임 오브젝트를 만들어야 되는지도..? 건설 작업할 때 다시 확인해볼 예정

# 마우스 호버되는 곳의 월드 좌표 가져오기
1. `Camara.main.ScreenPointToRay(Input.mousePosition)`을 통해 Ray를 획득
2. 획득한 Ray를 터레인에 `Pysics.Raycast`하여 히트 지점을 획득
    - Pysics.Raycast(ray, out var hit, 1000.0f, terrainMask)
    - terrainMask = LayerMask.GetMask("Terrain")
    - scene에 Layer=Terrain로 지정된 게임 오브젝트(aka.터레인)가 필요함
        - Terrain 컴포넌트 가지고 있음
        - Terrain Collider 컴포넌트 가지고 있음

# Quad와 Plane의 차이?
- 얼핏 바닥을 표현하기에는 plane이 적절하다고 생각되지만 실제 프로토타이핑하려고 보면 plane은 상당히 크다.
- 비교
    - Plane은 세분화된 고해상도 평면 메쉬
        - 10x10개의 세그먼트(총 121개의 버텍스)로 이루어진 세분화
        - 디폼, 물리효과, 그림자 처리, 셰이더 효과에 적합
        - 기본 노멀의 방향은 위쪽(+Y)
        - 지형, 충돌이 필요한 바닥, 그림자 받는 표면 등에 주로 사용
    - Quad는 단순한 저해상도 평면 메쉬
        - 단순히 2개의 삼각형으로 구성된 1장의 사각형(총 4개의 버텍스)
        - 가볍고 효율적이어서 2D 텍스처 표시, UI 요소, 간단한 평면 표시 등에 적합
        - 기본 노멀의 방향은 앞쪽(+Z)
        - 텍스처 출력, 스프라이트 효과, UI 위젯 배경, 간단한 디버그 렌더링 등에 주로 사용
- 프로토타이핑에는 quad로 충분한듯


# Trouble Shooting
## Trouble Shooting: URP 프로젝트 생성 직후 log4net 관련 에러 발생
- com.unity.collab-proxy 패키지 제거하기
    1. [Window] > [Package Manager]
    2. Unity Collaborate 또는 Version Control 패키지를 찾는다.
    3. "Remove" 버튼을 눌러 제거
    4. Unity를 재시작
    
## Newtonsoft Json을 사용할 수 없다.
- package manager를 이용해서 패키지를 추가해줘야 하나봄
    1. 유니티 에디터 상단 메뉴에서
    2. [windows] > [package manager] > [install package by name …]
    3. com.unity.nuget.newtonsoft-json

## Immutable collection을 사용할 수 없다.
- [microsoft article](https://learn.microsoft.com/en-us/visualstudio/gamedev/unity/unity-scripting-upgrade?view=vs-2019)
    1. 유니티 에디터 상단 메뉴에서
    2. [Edit] > [Project Settings] > [Player] > [Other Settings]
    3. 팝업에서 [Configuration] > [Api Compatibitliy Level]
        - .NET 4.x
