+++
title="konltk.detector package"
+++
konltk detector module

## konltk.detector package

### Module contents
이 모듈에는 파일 혹은 문자열의 문자 인코딩을 확인을 할 수 있는 class가 포함되어 있습니다.
### Submodules
### konltk.detector.detector module
{{% panel %}}
*class* konltk.detector.detector.**Detect**<br>
detect(text, max)<br>
Parameters:<br>
text(string): 문자 인코딩을 확일한 텍스트 혹은 파일의 path<br>
max(int): 문자열 혹은 파일에서 앞에서 max만큼의 문자를 확인을 합니다. max의 수가 커질수록 정확도는 향상이 되지만 속도는 더 오래 걸릴 수 있습니다.<br>
*Return:* "NONE", "EUCKR", "UTF8", "UTF16BE", "UTF16LE" 이 중 하나를 얻습니다.<br>
*Return Type:* string<br>
{{% /panel %}}
