+++
title="konltk.tokenzier package"
+++
konltk tokenzier module

## konltk.tokenizer package
### Module contents
문자열 혹은 리스트 배열을 n-gram.....
### SubModules
### konltk.ngram.ngram module
```python
>>> from konltk.ngram import ngram
>>> n = ngram.Ngram()
>>> n.ngram("abcd", 2)
{('a', 'b'): 1, ('b', 'c'): 1, ('c', 'd'): 1}
```

{{% panel %}}
**class** konltk.ngram.ngram.**Ngram**<br>
*ngram(text, n)*<br>
Parameters:<br>
- text(string or list array): n-gram 할 문자열 혹은 list array<br>
- n(int): n-gram의 n<br>
*Return:* n-gram된 dictionary<br>
*Return Type:* dictionary<br>

*word_len()*<br>
*Return:* 나뉘어진 단어의 수<br>
*Return Type:* int<br>
    
*total_len()*<br>
*Return:* 모든 단어의 수<br>
*Return Type:* int<br>

*probabillity*<br>
*Return:* 각 단어의 빈도 확률<br>
*Return Type:* dictionary<br>
{{% /panel %}}

### konltk.wordcount.ngram module
```python
>>> from konltk.wordcount import wordcount
>>> wc = wordcount.WordCount()
>>> wc.wordcount("a b c d ef")
[('a', 1), ('b', 1), ('c', 1), ('d', 1), ('ef', 1)]
```
{{% panel %}}
**class** konltk.wordcount.wordcount.**WordCount**<br>
*wordcount(text)*<br>
Parameters:<br>
- text(string or list array): whitespace 기준으로 나뉘는 단어들의 수<br>
*Return:* 단어와 수가 튜플로 이루어진 list<br>
*Return Type:* list<br>
{{% /panel %}}