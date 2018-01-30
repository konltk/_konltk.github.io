+++
title = "hugo"
description = ""
+++
# hugo의 몇몇 사용법

# hugo의 이미지 삽입하기
## 마크다운 이미지 삽입

```
![이미지 설명](이미지 위치)
```

## hugo에서 이미지 삽입
이미지를 사용하기 위해서 이미지를 static 폴더에 위치를 하고 이미리 링크를 걸때 고정적인 위치를 링크하면 된다.

```
github.io
|--...
|--...
|--static
|  |--a.png
|--...
|--web
|  |--docs
|  |--...
```

위와 같이 디렉토리구조가 되어 있으면 이미지를 최상위 폴더의 `static` 폴더안에 위치하며, 이미지 링크를 `![image test](/static/a.png)` 고정 위치를 사용하면 된다.

# css override하기
[docdock theme](https://docdock.netlify.com/)

custom css를 사용하기 위해서 html에 css를 link하는 작업을 우선 해야한다.

```
github.io
|--...
|--web
|  |--layouts
|    |--partials
|      |--custom-head.html
```

`web/layouts/partials/custom-head.html` 파일에 자신이 만들 css파일을 `link`를 한다.

```
<link rel="stylesheet" href="/css/custom.css">
```

`/css/custom.css`
```
h1 {
    text-transform: none;
}
```
