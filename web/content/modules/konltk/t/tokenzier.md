+++
title="konltk.tokenzier package"
+++
konltk tokenzier module

## konltk.tokenizer package
### Module contents

### SubModules
### konltk.tokenizer.ngram module
```python
>>> from konltk.tokenizer.ngram import Ngram
>>> n = ngram.Ngram()
>>> n.ngram("abcd", 2)
{('a', 'b'): 1, ('b', 'c'): 1, ('c', 'd'): 1}
```

{{% panel %}}
**class** konltk.tokenizer.ngram.**Ngram**  
&emsp;**ngram(text, n)**<a id="konltk.tokenizer.ngram.Ngram.ngram"></a>  
&emsp;&emsp;Parameters:  
&emsp;&emsp;&emsp;- text(string or list array): n-gram 할 문자열 혹은 list array  
&emsp;&emsp;&emsp;- n(int): n-gram의 n  
&emsp;&emsp;*Return:* n-gram된 dictionary  
&emsp;&emsp;*Return Type:* dictionary  
  
&emsp;&emsp;**word_len()**<a id="konltk.tokenizer.ngram.Ngram.word_len"></a>  
&emsp;&emsp;*Return:* 나뉘어진 단어의 수  
&emsp;&emsp;*Return Type:* int  
    
&emsp;&emsp;**total_len()**<a id="konltk.tokenizer.ngram.Ngram.total_len"></a>  
&emsp;&emsp;*Return:* 모든 단어의 수  
&emsp;&emsp;*Return Type:* int  

&emsp;&emsp;**probabillity**<a id="konltk.tokenizer.ngram.Ngram.probabillity"></a>  
&emsp;&emsp;*Return:* 각 단어의 빈도 확률  
&emsp;&emsp;*Return Type:* dictionary  
{{% /panel %}}

### konltk.tokenizer.wordcount module
```python
>>> from konltk.tokenizer.wordcount import WordCount
>>> wc = wordcount.WordCount()
>>> wc.wordcount("a b c d ef")
[('a', 1), ('b', 1), ('c', 1), ('d', 1), ('ef', 1)]
```
{{% panel %}}
**class** konltk.tokenizer.wordcount.**WordCount**  
&emsp;**wordcount(text)**<a id="konltk.tokenizer.wordcount.WordCount.wordcount"></a>  
&emsp;&emsp;Parameters:  
&emsp;&emsp;&emsp;- text(string or list array): whitespace 기준으로 나뉘는 단어들의 수  
&emsp;&emsp;*Return:* 단어와 수가 튜플로 이루어진 list  
&emsp;&emsp;*Return Type:* list  
{{% /panel %}}