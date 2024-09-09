# CLIP4STR: A Simple Baseline for Scene Text Recognition with Pre-trained Vision-Language Model

## 요약
Vision Language Model (VLM)은 다양한 downstrem에서 강세를 보이고 있음.   
특히 CLIP이 대표적인 VLM으로 여러 distortion에서 강력한 언어추출 기능을 보이고 있음.  
따라서 본 논문에서 CLIP을 STR(OCR)의 영역으로 데리고 오는 연구를 수행함.  
모델은 2쌍의 Encoder-Decoder로 나누어져 있음.  
- visual branch: 시각정보에 대한 초기 예측
- cross-modal branch: 시각정보와 언어의미간의 불일치도 기반 교정

## 인트로  
고질적인 STR의 문제점 : 멀티모달을 구성하더라도, single-modality로 학습된 백본 모델에 성능이 의존적임.  
VLM이 irregular text에 대해 robust함 > 그래서 CLIP 써봄

![image](https://github.com/user-attachments/assets/ad35b900-bbe6-44f3-99eb-c2125918cef7)

> It is evident that CLIP pays high attention to the text sticker and accurately understands the meaning of the word, regardless of text variations
 
이를 통해 봤을때, CLIP을 백본으로 사용해도 괜찮을 것 같음.

CLIP4STR은 encoder-decoder 구조로 구성되어 있음. 
1) visual branch : clip에서 따옴
2) cross-model branch : transformer 디코더임 + PARSeq를 도입 >> 알파벳 순서 예측

predict and refine 구조 
> we design a dual predict-and-refine decoding scheme to fully utilize the capabilities of both encoderdecoder branches for improved character recognition.

## Method
![image](https://github.com/user-attachments/assets/98ca1ce6-417b-4785-8eb5-7096f39f6238)

CLIP + PSM(PARSeq) 
- CLIP은 토큰별 예측값 반환 / Encoder는 transformer 구조 + tokenizer 는 BPE 사용 >> 추후에 normalize and linear project 됨
- PSM은 캐릭터별 structral relationship을 찾는 것을 목표로 함.
  > This technique uses a random attention mask M for attention operations [25] to generate random dependency relationships between the input context and the output.

Text-encoder는 freeze된 상태, visual encoder만 학습

> The visual branch first performs autoregressive decoding, where the future output depends on previous predictions. Subsequently, the cross-modal branch addresses possible discrepancies between the visual feature and the text semantics of the visual prediction, aiming to improve recognition accuracy. This process is also autoregressive. Finally, the previous predictions are utilized as the input context for refining the output in a cloze-filling manner. 

![image](https://github.com/user-attachments/assets/98f342d2-118f-4d44-8b7d-463f29b9b8f8)

## Ablation  

1) Encoder는 생각보다 성능에 큰 영향 없음
   > The encoder is a ViT-S without pre-training. Then we apply the permuted sequence modeling (PSM) technique [26] to the visual decoder and follow the training recipe of PARSeq: 4×8 patch size, the same learning rate for the encoder and decoder, and 20 training epochs. This brings a 0.7% improvement in accuracy. Next, we replace the encoder with the image encoder of CLIPViT-B/16. However, no significant gain is observed without adaptations. 
2) PSM은 유용함
   > To better grasp the contribution of PSM, we conducted an experiment where we removed PSM and achieved an accuracy of 90.0%. This emphasizes the significance of PSM.

![image](https://github.com/user-attachments/assets/82f31388-961b-4063-a900-1f02b49bcfab)



