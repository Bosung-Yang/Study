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
