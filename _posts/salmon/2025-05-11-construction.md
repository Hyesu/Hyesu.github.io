---
layout: default
title: "작업일지::건설 1차"
date: 2025-05-11
categories: [salmon]
---

## 바닥 표현 방식
- 우선은 프로토타입 구현을 위해 가장 간단한 방식으로 처리
    - 터레인 위에 바닥 오브젝트 생성하여 처리
    - 추후 최적화하면서 더 좋은 방법 R&D해서 적용해야 함
        - Graphics.DrawMeshInstanced() 등으로 직접 그리는 방식 확인
        - dots
        - 배칭
        - terrain에 직접 shader로 해결하는 방식

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

## Trouble Shooting
### 설치 모드에서 제거 모드로 넘어갈 때 삭제 되상이 된 preview object가 트레이싱되면서 missing reference이 발생한다.
- `Object.Destroy(gameObject)`를 한다고 바로 삭제되는 것이 아니기 때문에, 삭제 모드에서 즉시 호버되는 오브젝트를 트레이싱하면서 문제가 발생함
    - Object.Destroy는 렌더가 끝나고 나서야 실제로 파괴되며, 파괴되기 전까지는 collider가 유효하기 때문에 Raycast로 검출되는 것
    - `Object.DestroyImmediate(gameObject)`를 호출하여 즉시 삭제하면 문제가 해결되지만, 유니티에서 비권장하는 방법이라고 함
- 가장 간단한 해결법으로는 `gameObject.SetActive(false)`를 호출해서 비활성화하는 것


### UI 버튼을 클릭할 때에도 3d world를 클릭했다고 인식하여, 프랍을 배치하려고 함
- `EventSystem.current.IsPointerOverGameObject()`를 통해 UI 클릭인지 알 수 있음


## 1차 작업 완료!
![완.료.](/assets/images/construction-complete-1.gif)
