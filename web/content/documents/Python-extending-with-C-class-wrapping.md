+++
title="Python extending with C class wrapping"
categories="extend-python"
tags=["python", "c"]
+++
## intro
가끔 c로 작성한 코드가 python으로 작성한 코드보다 효율이 좋을때가 있습니다. 
이럴때는 파이썬에서도 c로 작성한 코드를 사용할 수 있게 확장을 할 수 있습니다. 
이 문서에서는 c로 작성된 코드를 사용하여서 파이썬에서 ‘class’로 사용하기 위해서 wrapping을 하는 방법을 정리한 것입니다.

## c에서는 함수부분만 정의 나머지 작업은 python 내에서 구현
### python extending with c 방식으로 c로 주요 함수만 구현
python에서 제공하는 python c extending을 이용하여서 주요 함수를 c로 구현을 한다. 

```
static PyObject* wordcount(PyObject* self, PyObject*  args){
    int is_add = 0, n_uniqwords, n_words;
    char* input, *output_file = OUTPUT;
    PyObject *dlist = PyDict_New();
    PyObject *result_list = PyList_New(0);
    int i;
    FILE* temp_output_file;
    WORDCNT_STR Text={0}, *wp=&Text;
    if(!PyArg_ParseTuple(args, "si", &input, &is_add))
        return NULL;
    if(checkInputType(input) == TY_TEXT){
        FILE *temp_intput_file = fopen(TEMP, "w");
        fprintf(temp_intput_file, "%s", input);
        fclose(temp_intput_file);
        input = TEMP;
    }
    
    if(is_add)
        delete_accumulated_data();
    
    n_uniqwords = word_count(input, wp, 0/*word unit*/, 0/*dont show dot*/);
    for(i = 0; i < wp->n_uniqwords; i++){
        int temp = wp->sidx[i];
        PyObject *obj = PyTuple_New(2);
        PyTuple_SET_ITEM(obj, 0, PY_STRING(wp->words[temp]));
        PyTuple_SET_ITEM(obj, 1, PY_INT(wp->count[temp]));
        PyList_Append(result_list, obj);
    }    
    return result_list;
}
//methods list
static struct PyMethodDef methods[] ={
    {"wordcount", wordcount, METH_VARARGS},
    {NULL, NULL}
};
//main
MOD_INIT(wordcount){
    PyObject* m;
    MOD_DEF(m, "konlp.c.wordcount", "wordcount c code", methods);
    
    if(m == NULL)
        return MOD_ERROR_VAL;
    
    return MOD_SUCCESS_VAL(m);
}
```

위의 코드와 같이 구현을 할 수 있다. 자세히 구현하는 부분에 있어서는 “다른 문서”에 기술 되어있다.
위의 코드처럼 구현을 하면 python에서는 konlp.c.wordcount 의 패키지를 import를 하고 konlp.c.wordcount.wordcount()를 호출함으로써 주요 함수를 사용 할 수 있다.
이제 주요 함수를 호출 할 수 있으니 python에서 class를 만들어서 다른 함수를 구현하면 된다.

```python
import konlp.c.wordcount
import os
class WordCount:
    def __init__(self):
        self.result_list = []
    def wordcount(self, input = ""):
        self.input = input
        self.result_list = konlp.c.wordcount.wordcount(self.input, 1)
        return self.result_list
```

위의 코드처럼 다른 작업은 python으로 구현을 하고 중간에 보면 `konlp.c.wordcount.wordcount` 를 호출하였는데 이것이 c에서 작업한 함수를 호출한 부분이다.

## c의 라이브러리파일을 python에서 호출
위의 ‘python extending c방식으로 c로 주요 함수만 구현’ 에서는 c의 주요 함수를 python c extending의 문법에 맞춰서 c코드를 작성한 것이다. 하지만 python c extending 문법에 맞춰 c를 구현할 필요 없이 so파일이나 dll파일을 사용하여서 함수를 호출하여서 사용이 가능하다. 

아래의 python의 사용 예제를 들어가기전에 python 사용 예제에서 쓰인 so 파일의 c code이다.

```
//my_lib.c
#include "my_lib.h"
void foo(char * str){
    printf("%d %s\n", 11, str);
}


//my_lib.c
#include "my_lib.h"
void foo(char * str){
    printf("%d %s\n", 11, str);
}
```

python3에서는 ctypes 모듈을 사용한다. ctypes의 CDLL 함수를 이용하여 라이브러리 파일을 load를 한다. 
간단한 예제는 아래와 같다.

```python
import ctypes
lib = ctypes.CDLL("libmy_lib.so")
rv = lib.foo()
```

foo 함수에서는 매개변수로 문자열을 받는다. 파이썬에서 c의 문자열을 사용하기 위해서는 ctypes에 있는 c data types를 사용한다.

| ctypes type   | C Type        | python type   |
| :----------   | :---------    | :-----------  |
| c_bool        | _Bool         | bool(1)       |
| c_char        | char          | 1-character bytes object |
| c_wchar       | wchar_t       | 1-character string |
| c_byte        | char          | int           |
| c_ubyte       | unsigned char | int           |
| c_short       | c_short       | int           |
| c_ushort      | unsigned short| int           |
| c_int         | int           | int           |
| c_uint        | unsigned int  | int           |
| c_long        | long          | int           |
| c_ulong       | unsigned long | int           |
| c_longlong    | __int64 or long long | int           |
| c_size_t      | size_t        | int           |
| c_ssize_t     | ssize_t ot Py_ssize_t | int           |
| c_float       | float         | float         |
| c_double      | double        | float         |
| c_longdouble  | long double   | float         |
| c_char_p      | char * (NUL terminated) | bytes object or None |
| c_wchar_p     | wchar_t * (NUL terminated) | string or None |
| c_void_p      | void *        | int or None |

위의 표는 python에서 c 데이터 형식을 사용하기 위해서 사용하는 함수들입니다. 위의 표의 형식을 사용하여 위의 예제를 계속 진행하면,

```python
import ctypes
lib = ctypes.CDLL("libmy_lib.so")
str = "input text"
p = ctypes.create_string_buffer(str.encode())
rv = p.foo(p)
```

위와 같은 코드가 된다.
그러나 변경 가능한 메모리에 대한 포인터를 예상하는 함수에 전달하지 않도록주의해야합니다. 변경 가능한 메모리 블록이 필요하면 ctypes에는 create_string_buffer () 함수가있어 다양한 방법으로이를 생성합니다.

create_string_buffer () 함수는 이전 ctypes 릴리즈의 c_string () 함수뿐만 아니라 c_buffer () 함수 (여전히 별칭으로 사용 가능)를 대체합니다. C 유형 wchar_t의 유니 코드 문자를 포함하는 변경 가능한 메모리 블록을 작성하려면 create_unicode_buffer () 함수를 사용하십시오.

위의 대한 설명은 [https://docs.python.org/3/library/ctypes.html](https://docs.python.org/3/library/ctypes.html) 에 더 나와 있습니다.

##c에서 ptyhon class까지 구현
python c extending 문법에서 python에서 class 처럼 사용가능하게 Define new types 기능(?)을 제공한다.
아래의 코드가 Define new types을 하기위한 작업입니다.

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

멤버 함수는 위 코드의 tp_methos 부분에 PyMethodDef 구조체 배열을 인자로서 넘긴다.

```
static PyMethodDef Noddy_methods[] = {
    {"setOption", (PyCFunction)setOption, METH_VARARGS},
    {"setFile", (PyCFunction)setFile, METH_VARARGS},
    {"setString", (PyCFunction)setString, METH_VARARGS},
    {"setOutputFile", (PyCFunction)setOutputFile, METH_VARARGS},
    {"start", (PyCFunction)start, METH_VARARGS},
    {NULL}  /* Sentinel */
};
```

멤버 변수는 tp_members 부분에 구조체를 배열을 인자로서 넘긴다.

```
typedef struct {
    PyObject_HEAD
    PyObject* input;
    PyObject* output;
    PyObject* option;
    int type;
} Noddy;
```

위 처럼 코드를 작성한 후 파이썬에서 일반 클래스 처럼 사용이 가능하다.