+++
title="C언어를 파이썬 모듈화 할 때 파이썬 2, 3버전 호환 가능한 매크로"
categories="extend-python"
tags=["python", "c"]
+++

## 모듈 초기화
파이썬 2버전을 확장할때에는 init+모듈이름 함수 이내에 'Py_InitModule' 과 같은 함수를 사용하여서 모듈을 초기화 했다.

파이썬 3버전에서는 'PyModule_Create' 함수를 사용해서 모듈을 초기화 한다.
PyModule_Create 함수를 호출할 때에는 PyModuleDef 구조체를 인자로 넘거야 한다.
```
static struct PyModuleDef moduledef = {
    PyModuleDef_HEAD_INIT,
    "lib",   /* name of module */
    NULL, /* module documentation, may be NULL */
    -1,       /* size of per-interpreter state of the module,
                 or -1 if the module keeps state in global variables. */
    methods	/* method_list */
};
//main
PyMODINIT_FUNC PyInit_lib(void){
    return PyModule_Create(&moduledef);
}
```


파이썬 2버전과 3버전을 같이 사용하고 싶으면 #if 를 사용한다.
```
#if PY_MAYJOR_VERSION >= 3
	static struct PyModuleDef moduledef = {
    	PyModuleDef_HEAD_INIT,
	    "lib",   /* name of module */
    	NULL, /* module documentation, may be NULL */
	    -1,       /* size of per-interpreter state of the module,
    	             or -1 if the module keeps state in global variables. */
	    methods	/* method_list */
	};
#endif

//main
PyMODINIT_FUNC PyInit_lib(void){
{
    #if PY_MAJOR_VERSION >= 3
    	m = PyModule_Create(&moduledef);
	#else
    	m = Py_InitModule3("lib", methods, "모듈 설명");
	#endif
    return m;
}
```

## 메인

파이썬 2버전에서는 아래와 같은 형식으로 메인 함수를 만들었다.
> void init<모듈이름>

하지만 파이썬 3버전에서는 아래와 같이 한다.
> PyMODINIT_FUNC PyInit_<모듈이름>

파이썬 2버전과 3버전을 한꺼번에 되는 코드는 아래와 같이 할 수 있다.
```
PyMODINIT_FUNC
#if PY_MAJOR_VERSION >= 3
PyInit_<모듈이름>()
#else
init<모듈이름>()
#endif
{
...
}
```

이것을 더 편히 간단히 한다면
```
#if PY_MAJOR_VERSION >= 3
	#define MOD_INIT(name) PyMODINIT_FUNC PyInit_##name()
#else
	#define MOD_INIT(name) PyMODINIT_FUNC init##name()

MODINIT(모듈 이름){
...
}
```

위의 각각 정의한것을 합치게 되면
```
#if PY_MAJOR_VERSION >= 3
static struct PyModuleDef moduledef = {
    	PyModuleDef_HEAD_INIT,
	    "lib",   /* name of module */
    	NULL, /* module documentation, may be NULL */
	    -1,       /* size of per-interpreter state of the module,
    	             or -1 if the module keeps state in global variables. */
	    methods
        }
#endif

static PyObject* moduleinit(){
	PyObject* m;

#if PY_MAJOR_VERSION >= 3
	m = PyModule_Create(&moduledef);
#else
	m = Py_InitModule("lib", methods);
#endif
	...
    return m;
}

#if PY_MAJOR_VERSION >= 3
PyMODINIT_FUNC initlib(){
	moduleinit();
}
#else
PyMODINIT_FUNC PyInit_lib(){
	return moduleinit();
}
#endif
```

좀 더 깔끔하게 한다면
```
#if PY_MAJOR_VERSION >= 3
	#define MOD_INIT(name) PyMODINIT_FUNC PyInt_##name()
    #define MOD_DEF(ob, name, doc, methids) \
    		static PyModuleDef moduledef = { \
            PyModuleDef_HEAD_INIT, name, doc, -1, methids};\
            ob = PyModule_Create(&moduledef);
#else
	#define MOD_INIT(name) void init##name()
    #define MOD_DEF(ob, name, doc, methids) \
    		ob = Py_InitModule3(name, methods, doc);
#endif            

MOD_INIT(lib){
PyObject* m;

MOD_DEF(m, "lib", module__doc__, methids)

...

return m;
}
```

## 참조
http://python3porting.com/cextensions.html


