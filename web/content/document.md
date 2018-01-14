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
<pre>	<b>detect(text, max)</b>
		<b>Parameters:
        	- text(string): 문자 인코딩을 확일한 텍스트 혹은 파일의 path
        	- max(int): 문자열 혹은 파일에서 앞에서 max만큼의 문자를 확인을 합니다. max의 수가 커질수록 정확도는 향상이 되지만 속도는 더 오래 걸릴 수 있습니다.
        <b>Return:</b> "NONE", "EUCKR", "UTF8", "UTF16BE", "UTF16LE" 이 중 하나를 얻습니다.
        <b>Return Type:</b> string
</pre>

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
<pre>	<b>ngram(text, n)</b>
		<b>Parameters:
        	- text(string or list array): n-gram 할 문자열 혹은 list array
        	- n(int): n-gram의 n
        <b>Return:</b> n-gram된 dictionary
        <b>Return Type:</b> dictionary
	
    <b>word_len()</b>
        <b>Return:</b> 나뉘어진 단어의 수
        <b>Return Type:</b> int
        
	<b>total_len()</b>
        <b>Return:</b> 모든 단어의 수
        <b>Return Type:</b> int
    
    <b>probabillity</b>
        <b>Return:</b> 각 단어의 빈도 확률
        <b>Return Type:</b> dictionary
</pre>

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
<pre>	<b>wordcount(text)</b>
		<b>Parameters:
        	- text(string or list array): whitespace 기준으로 나뉘는 단어들의 수
        <b>Return:</b> 단어와 수가 튜플로 이루어진 list
        <b>Return Type:</b> list
</pre>

# konlp.pos package
## Module contents
....
## SubModules
## konlp.pos.index module
```python
....
```
*class* konlp.pos.index.**Index**
<pre>	주어진 텍스트에서 명사만 추출하는 명사 추출기입니다.
	<b>dic_init(path)</b>
    	사전을 설정하는 함수입니다.
		<b>Parameters:
        	- path(string): 설정할 사전의 위치
        <b>Return:</b> 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.
        <b>Return Type:</b> int
	<b>index(data)</b>
    	이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.
		<b>Parameters:
        	- data(string): 명사를 추출하고 싶은 문자열이나 혹은 파일의 path
        <b>Return:</b> (원본, 명사 리스트) 튜플로 이루어진 리스트
        <b>Return Type:</b> list
</pre>

## konlp.pos.ham module
```python
....
```
*class* konlp.pos.ham.**Ham**
<pre>	형태소 분석기
	<b>dic_init(path)</b>
    	사전을 설정하는 함수입니다.
		<b>Parameters:
        	- path(string): 설정할 사전의 위치
        <b>Return:</b> 잘 설정이 되었으면 0, 아니면 1이 리턴이 됩니다.
        <b>Return Type:</b> int
	<b>morpha(data)</b>
    	이 함수를 사용하기전에 `dic_init` 함수를 사용하여서 사전을 설정을 해야합니다.
		<b>Parameters:
        	- data(string): 형태소 분석을 할 문자열
        <b>Return:</b> 튜플들로 이루어진 리스트
        <b>Return Type:</b> list
</pre>