---
layout: default
title: "작업일지::2D 스켈레탈 캐릭터 만들기"
date: 2025-05-05
categories: [neon]
---

## 캐릭터 애셋 준비
1. 각 파츠가 레이어로 분리된 psd 파일 준비
2. 유니티 패키지 매니저에서 2D PSD Importer와 2D Animation 패키지 활성화
3. psb 파일을 애셋으로 임포트
    - psd 파일의 확장자만 바꿔도 되는 것 같음
    - 배경 레이어가 투명으로 되어야 함에 주의

## 캐릭터 본 작업
1. 임포트한 애셋 클릭 > [Inspector] > [Open Sprite Editor] 누르면 레이어가 분리되어 있는 것을 확인할 수 있음
    - ![스프라이트 에디터](/assets/images/2d-project-character-01.png)
2. 스프라이트 에디터의 상잔 메뉴에서 [Skinning Editor]를 선택하고 [Create Bone]을 통해 본을 생성
3. [Geometry] > [Auto Geometry]를 눌러서 스프라이트 메시 생성
4. [Weights] > [Auto Weights] > [Generates All]으로 본에 대한 가중치까지 설정
5. 프리뷰 보면서 본이 적당한지 확인하고, 상단 [Apply] 버튼 눌러서 적용
    - ![본 움직여보기](/assets/images/2d-project-character-02.gif)

## 애니메이션 작업
1. 애셋을 씬에 배치하고 애니메이션 창을 열어서 한땀한땀 본에 대한 키프레임 잡아준다.
    - ![idle 모션](/assets/images/2d-project-character-03.gif)
