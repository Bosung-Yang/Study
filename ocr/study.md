# 관련 라이브러리

## EasyOCR
https://github.com/JaidedAI/EasyOCR  
기본적인 cnn+lstm기반의 ocr 라이브러리 
#### HG 데모
![image](https://github.com/user-attachments/assets/65b01c48-0b83-4542-80ed-2693c2614bcd)

#### Usage
```
import easyocr
reader = easyocr.Reader(['ko','en'])
result = reader.readtext('bet.png')
result
```
##### 실행시간 : 5.4s, 결과 : 
![image](https://github.com/user-attachments/assets/ed392030-c9e8-4f12-84de-6f0d8502333d)





## PaddlePaddle  
한국어 번역 프로젝트를 하면 좋을 것 같음 
https://github.com/PaddlePaddle/PaddleOCR

#### Usage
```
from paddleocr import PaddleOCR,draw_ocr

ocr = PaddleOCR(use_angle_cls=True, lang='korean') # need to run only once to download and load model into memory
img_path = './biz.png'
result = ocr.ocr(img_path, cls=True)
for idx in range(len(result)):
    res = result[idx]
    for line in res:
        print(line)
```
##### 실행시간 : 8.52s, 결과 : 
![image](https://github.com/user-attachments/assets/21e86e80-0231-429b-bb66-61054d899468)



## Tesseract (wrapped by pyocr)
#### Usage
```
from PIL import Image
import sys

import pyocr
import pyocr.builders

tools = pyocr.get_available_tools()
if len(tools) == 0:
    print("No OCR tool found")
    sys.exit(1)

tool = tools[0]
print(tools)
print("Will use tool '%s'" % (tool.get_name()))


langs = tool.get_available_languages()
print("Available languages: %s" % ", ".join(langs))
lang = langs[0]
print("Will use lang '%s'" % (lang))

txt = tool.image_to_string(
    Image.open('bet.png'),
    lang='kor',
    builder=pyocr.builders.TextBuilder()
)
```
##### 실행시간 : 0.5s, 결과 : 
![image](https://github.com/user-attachments/assets/48aed301-51df-45fd-a26b-d9e65ba3d767)


## 한국어 오타 교정 관련 내용
https://github.com/ssut/py-hanspell/tree/master
