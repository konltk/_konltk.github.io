+++
title = "Installing konltk"
description = "Installing konltk"
+++
Installing konltk
konltk는 맥을 제외한 리눅스 계열에서 사용이 가능합니다.

## 코드 다운받기
### 명령어로 코드를 다운받는 방법
```
git clone https://github.com/konltk/konltk
```

### Github페이지로 들어가서 코드를 다운받는 방법
[konltk github 주소](https://github.com/konltk/konltk)에 접속하여 코드를 다운 받습니다.
![konltk](www.konltk.org/static/installing/1.png)

## 설치하기
설치는 터미널의 명령어로 진행을 합니다.
설치전 python-dev(파이썬 3.xx 버전을 사용시 python3-dev)이 설치가 되어 있어야 합니다.

코드를 다운 받은 후(Github 페이지에서 다운을 받았으면 unzip도 해야합니다) 가장 최상위 폴더로 이동을 합니다.
이동 후 `sudo python setup.py install`(파이썬 3.xx 버전을 사용시 `sudo python3 setup.py install`) 명령어를 통해서 설치를 진행합니다.

```
$ cd konltk
konltk$ sudo python setup.py install
```

## 사용하기전
설치하고 나서 바로 사용하실경우 `source /etc/profile.d` 명령어를 터미널에서 실행을 한 후 사용하시면 문제없이 사용이 가능합니다.
이 명령어는 피씨를 재부팅을 하면 다시 하실 필요 없습니다.
다만 재부팅을 하지 않을 경우는 사용할 터미널에서 위의 명령어를 실행 후 사용해야합니다.