---
layout: post
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


# 기본적인 폴더 구조
```
your-repository/
│
├── _config.yml
├── _posts/
│   ├── example-post.md
├── _layouts/
│   └── default.html
├── index.md
└── other files...
```

## _config.yml
- Jekyll 블로그의 설정 파일로, 사이트 제목, URL, 레이아웃 등을 정의

## index.md
- 홈페이지