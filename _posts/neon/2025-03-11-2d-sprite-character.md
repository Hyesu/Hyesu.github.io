---
layout: default
title: "작업일지::2D 스프라이트 캐릭터 만들기"
date: 2025-03-11
categories: [neon]
---

# 스프라이트 캐릭터
## 스프라이트 애니메이션
- 스프라이트 애셋 준비
    - [출처](https://m.blog.naver.com/kch8246/221012502429)
    1. 아틀라스 이미지를 애셋 폴더에 추가
    2. [Inspector] 에서 임포트 세팅 변경
        - Texture Type => Sprite (2D and UI)
        - Sprite Mode => Multiple
        - 우하단 [Sprite Editor] 버튼 클릭
    3. [Sprite Editor] > 상단 툴바 > [Slice]
        - Type => Automatic
        - 우하단 [Slice] 버튼 클릭
    4. 작업 완료 후 [Sprite Editor] > 상단 툴바 > [Apply]
- 간단한 Idle 애니메이션을 하는 스프라이트 캐릭터 만들기
    1. 애니메이션 창을 열어두기
        - [Window] > [Animation] > [Animation]
    2. 빈 스프라이트 오브젝트 생성
        -  [Hierarchy] > 우클릭 > [2D Objects] > [Sprites] > [Square]
    3. 2번에서 생성한 오브젝트 선택하고 애니메이션 창에서 [Create] 버튼을 눌러서 애니메이션 클립 생성
    4. 타임라인으로 애니메이션 작업
        - [Sprite Renderer] > [Sprite] 속성을 변경하여 키프레임 생성하면 됨
    5. 애니메이션 컨트롤러 생성하고, entry 이후 애니메이션 실행하게 하기
        - [Create] > [Animator Controller]
    6. 2번에서 만든 오브젝트에 [Animator] 컴포넌트 추가 및 5번에서 생성한 컨트롤러 연결