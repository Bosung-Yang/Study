# DTrOCR: Decoder-only Transformer for Optical Character Recognition
Author: Masato Fujitake (FA Research, Fast Accounting Co., Ltd. Japan)

## summary 
- Decoder-Only Transformer 구조
- Vision Encoder 없이 SOTA 성능 달성

## 인트로
현재 OCR 모델들은 encoder-decoder 구조임.  
최근 언어모델은 decoder-only구조로 성공하고 있는데, OCR에는 아직 적용이 안되서 해보는게 논문의 목적임.  
```
The proposed method transforms a pretrained generative LM with a high language representation capability into a text recognition model.
Generative models that are used in NLP use a text sequence as input and generate the following text autoregressively.
In this work, the model is trained to use an image as the input. The input image is converted into a patch sequence, and the recognition results are output autoregressively.
```
즉, autoregression을 하는 decoder를 OCR에 도입하고자 함.
+ 그런데 이제 파인튜닝을 곁들인.

## 모델 구조
### 1. Patch embedding
이미지를 패치로 자르고, positional embedding 수행

### 2. Transfomer Decoder
기본적으로 구조는 GPT-2임. (pretrained version)  
디코더는 [SEP] 토큰으로 시작하는 이미지 시퀀스를 읽고, [EOS]를 받을때까지 텍스트 생성을 수행하고, softmax로 최종 만들어진 단어를 선정함.  
> 근데 이미지는 어떻게 토큰화함?

디코더는 여러층의 스택으로 구성, 총 1개의 트랜스포머 구성. 
- MHA, FFN이 있음

![image](https://github.com/user-attachments/assets/56542507-bc19-4474-915d-4732ca0e3591)


**구조적인 이점 : encoder가 없어서 cross-attention이 없어도 됨**

### 합성데이터셋 + 현실데이터셋 
> 걍 다른논문에도 있는 쓰잘데없는 이야기임.

## 실험
기존 실험에서 썼던 데이터들은 전처리가 필요해서 직접 만들었음. (이게 뭔...)  
데이터셋 목록:  
- PILE (영어)
- CC100 (중국어)
- Chinese NLP Corpus (중국어)

이파트에서 새로운 건 없음.  

### Scale 법칙 존재 : GPT 파라미터 커질수록, 데이터 많이 쓸수록 좋아짐


