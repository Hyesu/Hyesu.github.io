---
layout: default
title: "유니티::게임 오브젝트"
date: 2025-03-08
categories: [unity]
---

# 게임 오브젝트 / MonoBehaviour
- DontDestroyOnLoad(gameObj)
    - 게임 오브젝트를 씬이 변경되더라도 파괴되지 않게 함

## 특수 메소드 어트리뷰트
- RuntimeInitializeOnLoadMethod
    - 런타임의 특정 시점에 불리게 할 수 있음
    - ` [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]`를 통해서 씬 로드 이전에 초기화를 하게 할 수 있음
    - 게임 인스턴스 싱글턴 등을 만들때 좋아보임

## MonoBehaviour를 상속한 게임 오브젝트 이벤트 흐름
- Awake
    - 오브젝트 생성 시 실행 (가장 먼저 호출)
- Start
    - 첫 번째 Update() 전에 실행 (다른 오브젝트와 연계 가능)
- Update
    - 매 프레임 실행 (게임 로직)
- FixedUpdate
    - 물리 연산 전용 (고정 시간 간격)
- LateUpdate
    - Update() 이후 실행 (카메라 처리 등)
- 충돌 이벤트
    - OnCollision(물리 충돌) vs OnTrigger(트리거)
- 오브젝트 종료 이벤트
    - OnDisable(비활성화), OnDestroy(삭제)

## Expensive method invocation
- 유니티 스크립트 작업을 하다보면, (라이더 기준) 빨간 밑줄이 생기고 `Expensive method invocation` 이라는 메시지가 표시되는 경우가 있음
    - 유니티에서 비용이 많이 들수도 있는 것들을 표시해줌

#### getter가 정의된 static 변수 참조
- 싱글톤 구현 등에 주로 사용되는 기법인데, getter가 get 이상의 것을 하기 때문에 발생한다고 함


## 게임 오브젝트간 렌더 순서
- Sorting Layer로 오브젝트 타입별 소팅 적용
- 각 게임 오브젝트의 renderer 컴포넌트에서 order in layer를 수정하여 조정
    - order가 높을수록 위에 그려짐