---
layout: default
title: "작업일지::건설 1차"
date: 2025-05-11
categories: [salmon]
---

## 마우스 호버되는 곳의 월드 좌표 가져오기
1. `Camara.main.ScreenPointToRay(Input.mousePosition)`을 통해 Ray를 획득
2. 획득한 Ray를 터레인에 `Pysics.Raycast`하여 히트 지점을 획득
    - Pysics.Raycast(ray, out var hit, 1000.0f, terrainMask)
    - terrainMask = LayerMask.GetMask("Terrain")
    - scene에 Layer=Terrain로 지정된 게임 오브젝트(aka.터레인)가 필요함
        - Terrain 컴포넌트 가지고 있음
        - Terrain Collider 컴포넌트 가지고 있음


## 바닥 표현 방식
- 우선은 프로토타입 구현을 위해 가장 간단한 방식으로 처리
    - 터레인 위에 바닥 오브젝트 생성하여 처리
    - 추후 최적화하면서 더 좋은 방법 R&D해서 적용해야 함
        - Graphics.DrawMeshInstanced() 등으로 직접 그리는 방식 확인
        - dots
        - 배칭
        - terrain에 직접 shader로 해결하는 방식

### Quad와 Plane의 차이?
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


### 터레인 위에 Quad 오브젝트를 생성하지만 터레인 아래로 바닥이 묻힌다.
- ![terrain과 floor간의 z-fighting](/assets/images/construction-zfighting-1.gif)
- 해결 방법
    - 방법 1: 머티리얼 shader에서 ZWrite/Depth 설정 조정
    - 방법 2: 머티리얼의 Render queue 값을 조정
    - 방법 3: 카메라 후처리
        - Terrain은 Main Camera로 렌더링하고, Quad는 Overlay Camera로 렌더링하여 Stacked Camera를 이용하는 방식
        - 카메라 스택이 있는 URP에서 유용하다고 하는데, 사용하게 되면 좀 더 자세히 공부하고 적용
- 일단 가장 간단한 방법인 두 번째 방법으로 해결
    - 터레인 렌더 큐보다 높은 값으로 지정하면 됨
    - 아무리 프로토타입이라고 하더라도 디폴트 머티리얼을 계속 인스턴싱하기는 좀 그러니까 코드에서 수정하지 않고, 바닥용 머티리얼 분리해서 처리
    - Enable GPU Instancing 체크박스도 있는데, 최적화에 도움이 될 것 같음. 일단 키고, 추후 좀 더 자세히 알아보기로 함
- 머티리얼의 renderQueue를 수정해도 z-fighting이 발생함
    - 커스텀 shader를 만들고 offset을 지정하는 것으로 수정
    - 생성된 shader에 오프셋 지정 코드 추가
        ```
        Offset {Factor}, {Units}
        ```
        - Factor => 폴리곤의 기울기(slope)에 따른 깊이값 조정 팩터
        - Units => 깊이값에 정수 단위의 오프셋 추가
        - `SubShader` 블록에 추가하되, `CGPROGRAM` 이전에 넣어야 함
            - CG = C for Graphics
            - 즉 코드 이전에 상수를 정의하는 영역에 추가해서 사용
        - Units를 통해 깊이값에 대한 힌트를 줄 수 있으나, 너무 큰 값은 권장되지 않는다고 함
- ![해결하였다](/assets/images/construction-zfighting-2.gif)


## 참고: Unity 기본 렌더 큐 순서

| Render Queue 이름 | 값    | 설명                         |
| --------------- | ---- | -------------------------- |
| Geometry        | 2000 | 대부분의 오브젝트 기본값 (Terrain 포함) |
| AlphaTest       | 2450 | Cutout 처리용                 |
| Transparent     | 3000 | 반투명 오브젝트용                  |


## 참고: GPU Instancing
다음 조건을 모두 만족해야 함

| 조건                           | 설명                                           |
| ---------------------------- | -------------------------------------------- |
| **동일한 메시**                   | 예: 모두 Cube                                   |
| **동일한 머티리얼**                 | 머티리얼이 완전히 같아야 함 (속성 포함)                      |
| **Enable GPU Instancing 체크** | 이게 켜져 있어야 GPU가 인식함                           |
| **쉐이더가 인스턴싱 지원**             | 대부분의 Unity 표준 쉐이더(`Standard`, `URP/Lit`)는 지원 |
| **SRP Batcher와 충돌 없음**       | (URP에서 경우에 따라 충돌 가능)                         |

- 풀밭, 군중, 반복되는 환경 구조물 등에서 GPU Instancing은 Draw Call 수를 대폭 줄여준다고 함
- `Graphics.DrawMeshInstanced()` 혹은 `Graphics.DrawMeshInstancedIndirect()` 같은 API로도 수동 사용 가능
- 작동 여부 확인 방법
    - Frame Debugger (Window → Analysis → Frame Debugger) 를 켜면, Draw Mesh (instanced) 로 표시됨
    - 일반적인 Draw Mesh 와 다르게 하나의 호출로 여러 개가 그려지는 것을 볼 수 있음


## 참고: Unlit Shader vs. Standard Surface Shader
바닥을 표현하기 위한 간단한 shader를 만들 때, 비교가 되는 것들

| 항목             | **Unlit Shader**            | **Standard Surface Shader** |
| -------------- | --------------------------- | --------------------------- |
| **조명 반영 여부**   | ❌ 조명 영향 안 받음                | ✅ 조명 영향 받음                  |
| **그림자, 광원 처리** | 없음                          | 있음 (빛, 그림자, 반사 등 전부 계산)     |
| **퍼포먼스(성능)**   | 빠름 (계산 적음)                  | 느림 (광원 계산 등 복잡함)            |
| **용도**         | HUD, UI, 이펙트, 마커, 투명 오브젝트 등 | 일반 3D 모델, 캐릭터, 배경 오브젝트 등    |
| **셰이더 구조**     | 단순함                         | Unity의 표준 라이팅 파이프라인 사용      |



## 여담
material은 머티리얼이라고 읽는게 공식이라고 함. 어색하다.. 나는 매터리얼이라고 읽었는데.. 촌스러운 사람이었다..

머티리얼..머티리얼..머티..리얼.....

