+++
title="konltk.tag package"
+++
konltk pos module

## konltk.pos package
### Module contents
....
### SubModules
### konltk.tag.index module
```python
from konltk.pos.index import KltIndex

idx = KltIndex()
rc = idx.dic_init()
index_list = idx.index("나는 학교에 간다")
```
{{% panel %}}
**class** konltk.tag.index.**KltIndex**<br>
주어진 텍스트에서 명사만 추출하는 명사 추출기입니다.<br>
**def** dic_init(path)<br>
사전을 설정하는 함수입니다.<br>
Parameters:<br>
- path(string): 설정할 사전의 위치<br>
*Return:* 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.<br>
*Return Type:* int<br><br>
**def** index(data)<br>
이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.<br>
Parameters:<br>
- data(string): 명사를 추출하고 싶은 문자열이나 혹은 파일의 path<br>
*Return:* (원본, 명사 리스트) 튜플로 이루어진 리스트<br>
*Return Type:* list<br>

**def** noun_comp(data)<br>
이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.<br>
Parameters:<br>
- data(string): 분해하고 싶은 복합명사
*Return:* 분해된 명사들의 list
*Return Type:* list<br>
{{% /panel %}}

### konltk.tag.ham module
```python
....
```

{{% panel %}}
**class** konltk.tag.ham.**KltHam**<br>
형태소 분석기<br>
*dic_init(path)*<br>
사전을 설정하는 함수입니다.<br>
*Parameters:<br>
- path(string): 설정할 사전의 위치<br>
*Return:* 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.<br>
*Return Type:* int<br>
*morpha(data)*<br>
이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.<br>
*Parameters:<br>
- data(string): 형태소 분석을 할 문자열<br>
*Return:* 튜플들로 이루어진 리스트<br>
*Return Type:* list<br>
{{% /panel %}}