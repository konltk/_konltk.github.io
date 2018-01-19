+++
title="konltk.pos package"
+++
konltk pos module

## konltk.pos package
### Module contents
....
### SubModules
### konltk.pos.index module
```python
....
```
*class* konltk.pos.index.**Index**
주어진 텍스트에서 명사만 추출하는 명사 추출기입니다.
*dic_init(path)*
사전을 설정하는 함수입니다.
Parameters:
- path(string): 설정할 사전의 위치
*Return:* 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.
*Return Type:* int
*index(data)*
이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.
Parameters:
- data(string): 명사를 추출하고 싶은 문자열이나 혹은 파일의 path
*Return:* (원본, 명사 리스트) 튜플로 이루어진 리스트
*Return Type:* list


### konltk.pos.ham module
```python
....
```
*class* konltk.pos.ham.**Ham**
형태소 분석기
*dic_init(path)*
사전을 설정하는 함수입니다.
*Parameters:
- path(string): 설정할 사전의 위치
*Return:* 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.
*Return Type:* int
*morpha(data)*
이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.
*Parameters:
- data(string): 형태소 분석을 할 문자열
*Return:* 튜플들로 이루어진 리스트
*Return Type:* list
