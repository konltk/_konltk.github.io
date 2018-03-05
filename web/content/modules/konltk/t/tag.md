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
**class** konltk.tag.index.**KltIndex**  
주어진 텍스트에서 명사만 추출하는 명사 추출기입니다.  
&emsp;**dic_init(path)**  
&emsp;&emsp;사전을 설정하는 함수입니다.  
&emsp;&emsp;Parameters:  
&emsp;&emsp;&emsp;- path(string): 설정할 사전의 위치  
&emsp;&emsp;*Return:* 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.  
&emsp;&emsp;*Return Type:* int    
  
&emsp;**index(data)**  
&emsp;&emsp;이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.  
&emsp;&emsp;Parameters:  
&emsp;&emsp;&emsp;- data(string): 명사를 추출하고 싶은 문자열이나 혹은 파일의 path  
&emsp;&emsp;*Return:* (원본, 명사 리스트) 튜플로 이루어진 리스트  
&emsp;&emsp;*Return Type:* list  

&emsp;**noun_comp(data)**  
&emsp;&emsp;이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.  
&emsp;&emsp;Parameters:  
&emsp;&emsp;&emsp;- data(string): 분해하고 싶은 복합명사
&emsp;&emsp;*Return:* 분해된 명사들의 list
&emsp;&emsp;*Return Type:* list  
{{% /panel %}}

### konltk.tag.kma module
```python
....
```

{{% panel %}}
**class** konltk.tag.kma.**KltKam**  
형태소 분석기  
&emsp;**dic_init(path)**  
&emsp;&emsp;사전을 설정하는 함수입니다.  
&emsp;&emsp;Parameters:  
&emsp;&emsp;&emsp;- path(string): 설정할 사전의 위치  
&emsp;&emsp;*Return:* 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.  
&emsp;&emsp;*Return Type:* int  
&emsp;**morpha(data)**  
&emsp;&emsp;이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.  
&emsp;&emsp;Parameters:  
&emsp;&emsp;&emsp;- data(string): 형태소 분석을 할 문자열  
&emsp;&emsp;*Return:* 튜플들로 이루어진 리스트  
&emsp;&emsp;*Return Type:* list  
{{% /panel %}}