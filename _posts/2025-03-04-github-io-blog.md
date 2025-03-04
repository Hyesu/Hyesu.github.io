---
layout: default
title: "github.io로 블로그 만들기"
date: 2025-03-04
---

## Jekyll 블로그
Jekyll은 제킬이라고 읽으며, 지킬 앤 하이드에서 따왔다고 함. 근데 제킬이라고 읽는다고 함.

### Jekyll 설치
1. Ruby
    - devkit 포함된 것을 다운로드 권장. gem 설치시 필수적으로 요구되는 경우가 많다고 함
    - 설치 완료 후 버전 확인을 통해 설치가 되었는지 확인
        ```bash
        ruby -v
        ```
    - 환경 변수 설정이 잘 되어도 git bash에서 안되는 경우가 있음
        - 이 경우 git bash에서 따로 path를 설정해줘야 함
            ```bash
            # git bash를 열고 git bash 내에서 홈(~)으로 이동
            cd ~

            # .bashrc 파일을 열기(없으면 생성)
            vi .bashrc
            ```
        - .bashrc 파일에 ruby 경로에 대한 환경 변수 추가
            ```
            export PATH=$PATH:/c/Ruby33-x64/bin
            ```
        - 변경 사항 적용
            ```bash
            source ~/.bashrc

            # 다시 루비 설치 확인
            ruby -v
            ```
2. Bundler 설치
    ```bash
    gem install bundler
    ```
3. Jekyll 설치
    ```bash
    gem install jekyll
    ```
4. Jekyll 사이트 생성
    ```bash
    jekyll new . --force
    ```
5. 포스트 추가
    - `index.md` 또는 `_posts/` 폴더에서 블로그 포스트 추가
6. 로컬 서버를 통한 미리 보기
    - 서버 띄우기
        ```bash
        bundle exec jekyll serve
        ```
    - 브라우저로 `http://localhost:4000`을 통해 미리보기

### 테마 적용하기
- [minimal mistakes 테마 가이드](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#gem-based-method)
    - Gem-based method를 따라하면 쉬움
- 스킨 적용
    - `_config.yml`에 `minimal_mistakes_skin`를 key로 값을 써야함
    - [minimal mistakes 스킨 목록](https://github.com/mmistakes/minimal-mistakes?tab=readme-ov-file#skins-color-variations)


#### Trouble Shooting
1. Gem-based method를 사용하는 경우 github에서 host해주는 url로 확인하면 테마가 적용이 안됨.
    - `_config.yml`에 remote_theme을 이용하는 방식으로 설정하라고 함

2. 개별 post에서 테마 적용안됨
    - `layout: default`이 빠져 있어서 그러함
    - 포스트 파일 최상단 프로퍼티 부분에 레이아웃을 명시해야 함


## 기본적인 폴더 구조
```
your-repository/
│
├── _config.yml
├── _posts/
│   ├── YYYY-MM-DD-title-of-post-with-dash.md
├── _layouts/
│   └── default.html
├── index.md
└── other files...
```

### _config.yml
- Jekyll 블로그의 설정 파일로, 사이트 제목, URL, 레이아웃 등을 정의

### index.md
- 홈페이지

### _posts
- 포스트들이 속한 폴더로 포스트의 이름 포맷은 다음과 같음
    - YYYY-MM-DD-{title}.md
    - 모두 대시(-)로만 구분함
- 파일 상단은 `---`를 통해 메타 데이터를 설정하게 되어 있음
    ```md
    ---
    layout: default
    title: "github.io로 블로그 만들기"
    date: 2025-03-04
    ---
    여기부터 본문
    ```

## 다음에 해보면 좋을 것..
- 사이드 바를 만들고, 카테고리 만들기