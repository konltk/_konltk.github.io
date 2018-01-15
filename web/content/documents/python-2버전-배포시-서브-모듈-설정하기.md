---
title: "python 2버전 배포시 서브 모듈 설정하기"
categories: 
 - extend-python
tags: [python, c]
---
# python 2버전 배포시 서브 모듈 설정하기
# python 서브모듈
파이썬에서는 폴더별로 서브모듈이 나뉘어진다.
```
foo
|--tmp
|  |--__init__.py
|  |--tmp.py
|
|--foo.py
```
위와 같이 파일구조로 되어있다면 실제로 파이썬에서는 
```python
import foo.tmp.tmp
```
디렉토리 구조로서 서브모듈을 접근이 가능하다.

# python extending with c 서브모듈 설정
서브모듈을 설정하기 위해서 우선 c code의 `InitModule`을 할 때 서브 모듈명을 명시를 해야한다.
`Py_InitModule("foo.tmp.tmp", methods, "")`
서브 모듈명은 자신이 사용하고 싶은 모듈명을 명시하면 된다.

파이썬3버전에서는 위의 단계만하여도 실제로 사용하는데에 있어서 문제가 없다.
하지만 파이썬2버전에서 각 폴더에 `__init__.py`가 존재해야지 서브모듈로서 사용이 가능하다.

python extending with c를 install을 하면 각 파이썬 파일이 python package위치에 복사가 된다. 여기서 `__init__.py`파일은 복사가 되지 않기 때문에 서브모듈이 적용이 안된다.

서브 모듈을 적용 시키기 위해서 setup함수의 인자 중 `packages` 와 `package_dir`를 사용하면 `__init__.py`파일도 같이 복사가 된다.(아직 복사인지 생성인지 잘 모르겠다.)

만약에 initmodule에서 명시한 서브모듈중에서 물리적으로 폴더가 존재하지 않는다면 물리적인 폴더를 생성하여서 같이 포함하면 된다.