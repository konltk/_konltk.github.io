+++
title="Python extending with C"
categories="extend-python"
tags=["python", "c"]
+++
This document is based on the Python 2.x version.

## As a python extending language c
### Python work when load module
파이썬에서 모듈을 로드를 할 때에 아래와 같은 순서로 모듈을 찾게 된다.
> * 기본 모듈(soket, time 등)
> * c로 만들어진 dll 파일
> * 자체 제작한 파이썬 모듈

예시로 만약 파이썬에서 import mylib을 수행한다면, 파이썬에서는 sys.module과 sys.path의 디렉토리를 검색하여 mylib이라는 모듈을 찾는다.
### Create module using language c
c언어로 된 코드를 파이썬에서 로드하기 위해서는 c code에 대한 라이브러리 파일이 필요하다. Python extendion에서는 .so(리눅스)라이브러리 파일과 .pyd(윈도우) 라이브러리 파일이 필요하게 된다. .so 파일이나 .pyd파일을 파이썬이 설치된 디렉토리의 'Lib/site-package' 폴더에 복사를 하면 된다.
간단히 정리를 하면
> 1. c code를 컴파일하여 .so or .pyd 라이브러리 파일로 빌드를 한다
> 2. 빌드하여 나온 라이브러리 파일을 파이썬이 설치된 폴더의 Lib/site-package 폴더에 복사를 한다.

위의 두 과정으로 진행된다.
### Sample code
```
//mylib.c
#include "Python.h"
#include <stdio.h>

static PyObject* ErrorObject;

// 실제 동작하는 함수
static PyObject* write_log(PyObject *self, PyObject *args) // 인자는 이와같이 고정된다.
{
char* msg;
    
   if(!PyArg_ParseTuple(args, "s", &msg))
   	return NULL;

    printf(“c import test\n”);    
    return Py_BuildValue("i", 0);    
}
 
/* methods 구조체 배열에 지정되는 정보는 {"실제사용할 메쏘드명", 메쏘드명에 대응하는 실제 동작하는 함수명, 인자 종류} */
static struct PyMethodDef methods[] =
{
    {"wlog", write_log, METH_VARARGS},
    {NULL, NULL}
};

void initmylib()
{
    PyObject* m;
    
// Py_InitModule("모듈명", 이모듈에 적용된 메쏘드들을 담을 구조체배열 포인터)
    m = Py_InitModule("mylib", methods); 

    ErrorObject = Py_BuildValue("s", "error");
}
```

파이썬이 c 모듈을 import 하게되면 초기화를 위해 'init+모듈이름'으로 된 함수를 호출한다. 모듈이름이 mylib이면 initmylib()을 호출한다.

즉, c의 main함수의 역할이 'init_모듈이름'으로 된 함수이다.

이 c파일을 컴파일하기 위해서 파이썬 파일이 필요하다.
```
# setup.py
from distutils.core import setup, Extension
 
setup(name = "mylib",
        version = "1.0",
        description = "print log",
        author = "sample",
        author_email = "sample@sample.com",
        url = "google.com",
        ext_modules = [Extension("mylib", ["mylib.c"])]
        )

```

### Compile
위에서 만든 c파일과 python파일을 같은 디렉토리에 넣러두고 아래의 명령어를 통하여서 컴파일을 하게된다.
> python setup.py install

위의 명령어를 입력하면 자동적으로 컴파일 후 .so파일이나 .pyd파일을 'Lib/site-package' 폴더에 복사가 된다. 컴파일을 할때는 리눅스에서는 gcc를 호출하고 윈도우에서는 visual c컴파일러를 호출한다.
#### compile on window
파이썬에서는 c를 컴파일할 때 vc++를 호출하기 때문에 컴퓨터에 비주얼 스튜디오가 설치되어 있어야하며, 설치된 비주얼스튜디로의 버전을 맞춰야한다.
> python\Lib\distutils\msvs9compiler.py

위의 path는 파이썬이 설치된 디렉토리이며 파이썬 파일은 파이썬 버전별로 달리질 수 있다.

파이썬 파일을 연 후 get_build_version 함수의 리턴값을 현재의 vc 버전에 맞춰서 적는다.
[vc 버전 정보](https://ko.wikipedia.org/wiki/%EB%B9%84%EC%A3%BC%EC%96%BC_C%2B%2B)

> visual C++ 2015 (14.0)
> visual C++ 2013 (12.0)
> visual C++ 2012 (11.0)
> visual C++ 2010 (10.0)

#### compule option
Setup.py를 install할때 파일안에서 정의한 c파일을 컴파일을 하는데 컴파일 옵션을 사용 할 수 있다.
평소 gcc를 사용할 때 쓰는 “-w –o –Wall” 등 여러 컴파일 옵션을 사용하는데 이 옵션들을 python extending에서도 사용할 수 있다.

```python
import os
import sys
from distutils.core import setup, Extension
# from setuptools import setup, Extension
 
setup(name = "konlp",
        version = "1.0",
        
        py_modules=["konlp.detector.detector", "konlp.ngram.ngram", "konlp.pos.index", "konlp.wordcount.wordcount"],
        ext_modules = [Extension("konlp.c.codescan", extra_compile_args=["-w"], sources=["konlp/detector/pydetector.c"]),
        Extension("konlp.c.wordcount", extra_compile_args=["-w"], sources=["konlp/wordcount/pywordcount.c", "konlp/wordcount/wordcount.c", "konlp/wordcount/wordcount-main.c", "konlp/wordcount/wordcount-sort.c"]),
	Extension("konlp.c.index", extra_compile_args=["-Wall"], extra_link_args=["-L.", "-lindex"], sources=["konlp/pos/pyindex.c"])]
)
```

위는 setup.py 예제 소스이다.
중간에 보면 `extra_compile_args`가 있는데 이 부분을 통하여서 각각의 컴파일 옵션을 줄 수 있다.

#### compile link
위 1.5.1 의 내용처럼 각각의 c파일에 대하여서 컴파일 링크를 할 수 있다.

```python
import os
import sys
from distutils.core import setup, Extension
# from setuptools import setup, Extension
 
setup(name = "konlp",
        version = "1.0",
        
        py_modules=["konlp.detector.detector", "konlp.ngram.ngram", "konlp.pos.index", "konlp.wordcount.wordcount"],
        ext_modules = [Extension("konlp.c.codescan", extra_compile_args=["-w"], sources=["konlp/detector/pydetector.c"]),
        Extension("konlp.c.wordcount", extra_compile_args=["-w"], sources=["konlp/wordcount/pywordcount.c", "konlp/wordcount/wordcount.c", "konlp/wordcount/wordcount-main.c", "konlp/wordcount/wordcount-sort.c"]),
	Extension("konlp.c.index", extra_compile_args=["-Wall"], extra_link_args=["-L.", "-lindex"], sources=["konlp/pos/pyindex.c"])]
)
```

위와 동일한 setup.py 예제이다.
중간에 보면 `extra_link_args`가 있는데 이 부분을 통하여서 각각의 컴파일 링크를 할 수 있다.

### Example
c 파일과 파이썬 파일을 한 디렉토리에 위치한다.
`python setup.py install`
위의 명령어를 실행한다.

그 후, 파이썬을 실행하여 확인한다.
```
>> import mylib
>> mylib.wlog("")
c import test
```

### sub module or sub package
파이썬에서는 폴더를 기준으로 sub module 혹은 sub package 가 정해진다.

```
dir
 |
 +-- test.py
 |
 +-- package
      |
      +-- __init__.py
      |
      +-- subpackage
           |
           +-- __init__.py
           |
           +-- module.py
```

파이썬에서 위와 같은 디렉토리 구조를 가지고 있다면 `test.py`에서 package/subpackage/module를 import하고 싶다면
`import package.subpackage.module`
위처럼 . 으로서 디렉토리를 하위 모듈 혹은 패키지를 접근이 가능하다.

하지만 c extending을 사용을 한다면 디렉토리 구조로서 하위 모듈 혹은 패키지를 사용을 못한다. 그러므로 c code 중 `Py_InitModule` 을 할때 자기가 사용하고 싶은 package명을 입력을 하면된다. 

```
...
m = Py_InitModule3("konlp.c.wordcount", "wordcount c code", methods);
...
```

위의 예제의 `konlp.c.wordcount` 처럼 사용하고 싶은 pacakge명을 입력을 하면 된다.

## Built-in method
initmylib에서 Py_initMoudle 할 때 인자로 PyMethidDef 구조페 배열을 넘긴다.
이 구조체 배열을 method table로서 파이썬에서 호출될 메서드들의 목록을 나타낸다.
구조는 {"메서드", 구현된 메서드, 호출규약, "설명"}으로 되어있다.
구조체 배열의 마지막 변수는 sentinel로서 {NULL, NULL}를 넣어준다.

구조체 배열은 아래와 같이 작성을 하면 된다.
```
static struct PyMethodDef methods[] ={
	{"get", get_sample_text, METH_VARARGS},
	{"wordcountToFile", wordCountToFile, METH_VARARGS},
	{NULL, NULL}
}
```
함수의 형식은
```
static PyObject* method(PyObject *self, PyObject *args){
    return Py_BuildValue("");    
}
```
위와 같이 되어있다.

### Parameter
함수를 호출할 때 인자를 사용하는데 인자는 PyArg_ParseTuple를 이용하여 파싱한다.

사용법 예시로는
```
if(!PyArg_ParseTuple(args,"s|s", &input_file, &output_file))
        return NULL;
}
```

위처럼 사용 할 수 있다.

`PyArg_ParseTuple`를 사용할 때 두번째 인자로 전해받을 인자의 형식을 정하는데 보통 `s - string, i - integer, f - float, d - double` 등의 여러 형식이 있다.
```
char *options_string, *input, *output = "output.txt";
char **argument;
int i = 0;
int num_of_input = 0;
if(!PyArg_ParseTuple(args, "sss", &option_string, &input, &output)){
	return NULL;
}
```

```
char *input, *output = "output.txt";
char *argument_list[3] = {0};
if(!PyArg_ParseTuple(args, "ss", &input, &output)){
	return NULL;
}
```

형식의 리스트는 [c-api](https://docs.python.org/2/c-api/arg.html) 여기서 볼 수 있다.



### Parameter example
```
int ok;
int i, j;
long k, l;
const char *s;
int size;
 
ok = PyArg_ParseTuple(args, ""); /* No arguments */
    /* Python call: f() */
ok = PyArg_ParseTuple(args, "s", &s); /* A string */
    /* Possible Python call: f('whoops!') */
ok = PyArg_ParseTuple(args, "lls", &k, &l, &s); /* Two longs and a string */
    /* Possible Python call: f(1, 2, 'three') */
ok = PyArg_ParseTuple(args, "(ii)s#", &i, &j, &s, &size);
    /* A pair of ints and a string, whose size is also returned */
    /* Possible Python call: f((1, 2), 'three') */

{
    const char *file;
    const char *mode = "r";
    int bufsize = 0;
    ok = PyArg_ParseTuple(args, "s|si", &file, &mode, &bufsize);
    /* A string, and optionally another string and an integer */
    /* Possible Python calls:
       f('spam')
       f('spam', 'w')
       f('spam', 'wb', 100000) */
}
{
    int left, top, right, bottom, h, v;
    ok = PyArg_ParseTuple(args, "((ii)(ii))(ii)",
             &left, &top, &right, &bottom, &h, &v);
    /* A rectangle and a point */
    /* Possible Python call:
       f(((0, 0), (400, 300)), (10, 10)) */
}
{
    Py_complex c;
    ok = PyArg_ParseTuple(args, "D:myfunction", &c);
    /* a complex, also providing a function name for errors */
    /* Possible Python call: myfunction(1+2j) */
}
```
### Variable
```
int ok;
int i, j;
long k, l;
const char *s;
int size;
 
ok = PyArg_ParseTuple(args, ""); /* No arguments */
    /* Python call: f() */
ok = PyArg_ParseTuple(args, "s", &s); /* A string */
    /* Possible Python call: f('whoops!') */
ok = PyArg_ParseTuple(args, "lls", &k, &l, &s); /* Two longs and a string */
    /* Possible Python call: f(1, 2, 'three') */
ok = PyArg_ParseTuple(args, "(ii)s#", &i, &j, &s, &size);
    /* A pair of ints and a string, whose size is also returned */
    /* Possible Python call: f((1, 2), 'three') */

{
    const char *file;
    const char *mode = "r";
    int bufsize = 0;
    ok = PyArg_ParseTuple(args, "s|si", &file, &mode, &bufsize);
    /* A string, and optionally another string and an integer */
    /* Possible Python calls:
       f('spam')
       f('spam', 'w')
       f('spam', 'wb', 100000) */
}
{
    int left, top, right, bottom, h, v;
    ok = PyArg_ParseTuple(args, "((ii)(ii))(ii)",
             &left, &top, &right, &bottom, &h, &v);
    /* A rectangle and a point */
    /* Possible Python call:
       f(((0, 0), (400, 300)), (10, 10)) */
}
{
    Py_complex c;
    ok = PyArg_ParseTuple(args, "D:myfunction", &c);
    /* a complex, also providing a function name for errors */
    /* Possible Python call: myfunction(1+2j) */
}
```

### List
PyObject 배열을 만들기 위해서는 PyList_New를 사용하여 만든다.
`PyObject *dlist = PyList_New(length);`
리스트를 만든후 PyList_SetItem 으로 원하는 인덱스의 변수를 수정할 수 있다.	
`PyList_SetItem(dlist, i, PyString_FromString(msg));`
리스트의 변수를 추가를 하고 싶다면 PyList_Append를 이용하여 item을 추가 할 수 있다.
`PyList_Append(dlist, PyString_FromString(msg));`

```
PyObjecy *dlist = PyList_New(0);
int i = 0;
while(1){
	char msg[1000];
	fscanf(sample_file, "%s", msg);
	if(i++ == 1000 || feof(sample_file)) break;
	PyList_Append(dlist, PyString_FromString(msg));
}
return dlist;



PyList_Append(dlist, Py_BuildValue("(is)", num, msg));
```
이 예제처럼 하게되며, 튜플형식으로 만들어진다.

### Defining New Types
Python extendion에는 C에 없는 클래스를 구현가능하게 되어있다.

```
static PyTypeObject noddy_NoddyType = {
	PyVarobject_HEAD_INIT(NULL, 0),
	"noddy.Noddy",
	sizeof(noddy_NoddyObject),	/* tp_name */
	0,							/* tp_basicsize */
	0, 							/* tp_itemsize */
	0, 							/* tp_dealloc */
	0, 							/* tp_print */
	0, 							/* tp_getattr */
	0, 							/* tp_setattr */
	0, 							/* tp_compare */
	0, 							/* tp_repr */
	0, 							/* tp_as_number */
	0, 							/* tp_as_sequence */
	0, 							/* tp_as_mapping */
	0, 							/* tp_hash */
	0,							/* tp_call */
	0, 							/* tp_str */
	0, 							/* tp_getattro */
	0, 							/* tp_setattro */
	0,							/* tp_as_buffer */
	0,							/* tp_flags */
	Py_TPFLAGS_DEFAULT,			/* tp_flags */
	"Noddy objects",			/* tp_doc */
};
```

위 사진과 같은 PyTypeObject 변수를 선언한다. 이 변수는 pymodule레 object를 추가하는데 있어서 사용된다.
`PyModule_AddObject(m, "Noddy", (PyObject*) &noddy_NoddyType);`

위 처럼 모듈을 오브젝트를 추가하면 파이썬에서 클래스처럼 사용이 가능하다.

``` python
import mylib
c1 = mylib.Noddy()
```

#### Member variable
```
typedef struct{
	PyObeject_HEAD
	PyObject* input;
	PyObject* output;
	PyObject* option;
	int type;
}	Noddy;
```

위와 같은 구조체의 건언으로 변수처럼 사용가능하다.

``` python
import mylib
c1 = mylib.Noddy()
c1.type = 1
c1.input = "input"
```

#### Member method
```
satic PyMethodDef Noddy_methods[] = {
	{"setOption", (PyCFunction)setOption, METH_VARARGS},
	{"setFile", (PyCFunction)setFile, METH_VARARGS},
	{"setString", (PyCFunction)setString, METH_VARARGS},
	{NULL}
}
```

PyMethodDef 배열을 만든 후,
```
static PyTypeObject NoddyType = {
    PyVarObject_HEAD_INIT(NULL, 0)
    "noddy.Noddy",             /* tp_name */
    sizeof(Noddy),             /* tp_basicsize */
    0,                         /* tp_itemsize */
    (destructor)Noddy_dealloc, /* tp_dealloc */
    0,                         /* tp_print */
    0,                         /* tp_getattr */
    0,                         /* tp_setattr */
    0,                         /* tp_compare */
    0,                         /* tp_repr */
    0,                         /* tp_as_number */
    0,                         /* tp_as_sequence */
    0,                         /* tp_as_mapping */
    0,                         /* tp_hash */
    0,                         /* tp_call */
    0,                         /* tp_str */
    0,                         /* tp_getattro */
    0,                         /* tp_setattro */
    0,                         /* tp_as_buffer */
    Py_TPFLAGS_DEFAULT |
        Py_TPFLAGS_BASETYPE,   /* tp_flags */
    "Noddy objects",           /* tp_doc */
    0,                         /* tp_traverse */
    0,                         /* tp_clear */
    0,                         /* tp_richcompare */
    0,                         /* tp_weaklistoffset */
    0,                         /* tp_iter */
    0,                         /* tp_iternext */
    Noddy_methods,             /* tp_methods */
    Noddy_members,             /* tp_members */
    0,                         /* tp_getset */
    0,                         /* tp_base */
    0,                         /* tp_dict */
    0,                         /* tp_descr_get */
    0,                         /* tp_descr_set */
    0,                         /* tp_dictoffset */
    (initproc)Noddy_init,      /* tp_init */
    0,                         /* tp_alloc */
    Noddy_new,                 /* tp_new */
};
```

PyTypeObject 구조체를 선언할 때 tp_methods 부분에 구조체배열을 입력한다.

## 참조
http://egloos.zum.com/batsu05/v/839472
http://qwefgh90.github.io/sphinx/python/translating_extending.html
https://docs.python.org/2/extending/extending.html
https://docs.python.org/2/c-api/string.html
https://docs.python.org/2/extending/newtypes.html
