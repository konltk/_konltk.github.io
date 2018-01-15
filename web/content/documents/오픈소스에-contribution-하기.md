---
layout: post
title: "오픈소스에 contribution 하기"
categories: 
 - open source
tags: [open source, github, controbution]
---
# github에서 오픈소스 컨트리뷰션 하기
## 프로젝트 fork하기
우선 컨트리뷰션하고 싶은 오픈소스의 깃허브에 들어가서 fork를 해야합니다.

여기서는 [konlp](https://github.com/konltk/konlp)로 예시를 들어보겠습니다.

[konlp](https://github.com/konltk/konlp) 여기에 들어가 우측 상단에 있는 `fork`버튼을 눌러 자신의 github 계정에 프로젝트를 복사를 한다.

![fork]({{site.url}}/assets/images/open source contribution/1.png)

## 흐름
여기서는 브랜치를 따로 만들어서 하는것이라니라 `master` 브랜치에서 진행한 것을 예로 듭니다.
1. 내용을 수정하여서 커밋한다.
2. 자신의 GitHub 프로젝트에 Push를 한다.
3. Pull Request를 한다.
4. 프로젝트 소유자가 Requset를 Merge한다.

흐름은 위와 같이 진행이 된다.

## Pull Request 하기
자기가 원하는 프로젝트를 fork하였다면 `github.com/<자신의 github>/<fork 한 프로젝트 명>`에 프로젝트가 복사되었다.

수정할 부분을 수정을 하고 request를 하고 싶다면 자신이 복사한 프로젝트의 github로 들어가서 `New Pull Request`버튼을 눌러 Pull Request 요청을 한다.

![pull request]({{site.url}}/assets/images/open source contribution/2.png)

다시 중간에 `Create pull request` 버튼을 눌러 request를 생성한다.

![pull request]({{site.url}}/assets/images/open source contribution/3.png)

앞으로 생성할 request의 제목과 설명을 적은 다음 밑의 `Create pull request`의 버튼을 눌러 최종 request생성을 한다.


이제 프로젝트 소유자가 request의 내용을 확인하고 merge를 하거나 close를 한다.

![pull request]({{site.url}}/assets/images/open source contribution/4.png)



# 출처
[출처](https://git-scm.com/book/ko/v2/GitHub-GitHub-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-%EA%B8%B0%EC%97%AC%ED%95%98%EA%B8%B0)