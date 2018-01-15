---
title: "Konlp Document"
date: 2017-12-28T08:58:25+09:00
draft: false
keywords: ["konltk", "document"]
---

konlp Document
============


# konlp.detector package

## Module contents
이 모듈에는 파일 혹은 문자열의 문자 인코딩을 확인을 할 수 있는 class가 포함되어 있습니다.
## Submodules
## konlp.detector.detector module
*class* konlp.detector.detector.**Detect**
detect(text, max)
Parameters:
- text(string): 문자 인코딩을 확일한 텍스트 혹은 파일의 path
- max(int): 문자열 혹은 파일에서 앞에서 max만큼의 문자를 확인을 합니다. max의 수가 커질수록 정확도는 향상이 되지만 속도는 더 오래 걸릴 수 있습니다.
*Return:* "NONE", "EUCKR", "UTF8", "UTF16BE", "UTF16LE" 이 중 하나를 얻습니다.
*Return Type:* string


# konlp.ngram package
## Module contents
문자열 혹은 리스트 배열을 n-gram.....
## SubModules
## konlp.ngram.ngram module
```python
>>> from konlp.ngram import ngram
>>> n = ngram.Ngram()
>>> n.ngram("abcd", 2)
{('a', 'b'): 1, ('b', 'c'): 1, ('c', 'd'): 1}
```
*class* konlp.ngram.ngram.**Ngram**
*ngram(text, n)*
Parameters:
- text(string or list array): n-gram 할 문자열 혹은 list array
- n(int): n-gram의 n
*Return:* n-gram된 dictionary
*Return Type:* dictionary

*word_len()*
*Return:* 나뉘어진 단어의 수
*Return Type:* int
    
*total_len()*
*Return:* 모든 단어의 수
*Return Type:* int

*probabillity*
*Return:* 각 단어의 빈도 확률
*Return Type:* dictionary


# konlp.wordcount package
## Module contents
....
## SubModules
## konlp.wordcount.ngram module
```python
>>> from konlp.wordcount import wordcount
>>> wc = wordcount.WordCount()
>>> wc.wordcount("a b c d ef")
[('a', 1), ('b', 1), ('c', 1), ('d', 1), ('ef', 1)]
```
*class* konlp.wordcount.wordcount.**WordCount**
*wordcount(text)*
Parameters:
- text(string or list array): whitespace 기준으로 나뉘는 단어들의 수
*Return:* 단어와 수가 튜플로 이루어진 list
*Return Type:* list


# konlp.pos package
## Module contents
....
## SubModules
## konlp.pos.index module
```python
....
```
*class* konlp.pos.index.**Index**
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


## konlp.pos.ham module
```python
....
```
*class* konlp.pos.ham.**Ham**
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
