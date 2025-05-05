---
layout: default
title: "라이더 관련 설정"
date: 2025-03-29
categories: [dev]
---

# 개발 환경 구축

## 라이더
#### Unity에서 라이더 프로젝트 연결
- Unity에서 Rider 설정
    1. [Edit] > [Preferences]
    2. [External Tools]에서 External Script Editor를 JetBrains Rider로 변경
    3. [Generate .csproj files for] 항목의 체크 설정
        - Embedded packages
        - Local packages
        - Registry packages


#### 라이더 ReSharper 빌드 해제
- 리샤퍼에서 증분 빌드 옵션이 켜져있으면, 대체로 변수값 바꾸거나 하는게 반영이 안될 때가 많음
- 해제하려면 다음과 같이 처리
    1. [File] > [Settings]
    2. [검색창] > "Use ReSharper Build"
    3. 체크 해제!
