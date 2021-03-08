# CBPN : https://www.semanticscholar.org/paper/Efficient-Single-Image-Super-Resolution-via-Hybrid-Zhu-Zhao/9fabdda717babe074670d754ec77281d3a70c677

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Efficient Single Image Super-Resolution via Hybrid Residual Feature Learning with Compact Back-Projection Network
-----------------------------------------------------------------------------------------------------------------

Abstract
--------
  * 딥러닝 기반 SISR 모델들의 높은 정확도 달성
  * 하지만 SISR에서 효율성과 정확도 사이의 좋은 균형을 달성하는데 문제를 가지고 있음 
  * LR과 HR을 같이 사용하여 잔여 특징을 학습하면 더 효율적임
  * Hybrid Residual Feature Learning을 달성하기 위해 작은 크기의 필터를 이용하는 up-down sampling layer를 가지며, LR과 HR공간에서 특징들을 동시에 사용하는 compact back-projection network 제안

Introduction
------------
  * Residual 방식, deep back-projection 방식, Memory 방식들은 높은 정확도를 가지고는 있지만 많은 Parameter와 operation를 가지고 있어 실시간 및 저 전력 컴퓨터에 적용하기 어려운 점을 가지고 있음
  * DBPN의 저자는 위와 같은 문제를 해결하기 위해 기존의 DBPN구조에서 깊이뿐만 아니라 필터의 수도 줄여 경량화를 시켰지만 더불어 정확도 또한 줄어듦
  * CARN의 저자는 LR 특징 공간에서 Residual을 학습하기 위해 다수의 shortcut connection을 사용하는 종속 잔여 네트워크를 제안, 재귀적인 방법을 이용해 Mobile Applications에서도 사용가능하게 만듦(LR 공간만 이용하여 특징 추출)
  * information distillation Net또한 LR공간에서 HR Residual 특징을 학습함으로써 적은 parameters 와 계산량을 가지고 있음(LR 공간만 이용하여 특징 추출)
  * DBPN에서 영감을 받아 LR과 HR공간 모두에서 Residual learning을 통해 정확한 SR결과를 얻는 네트워크 제안
  * HR 이미지 공간에서 Residuald,f 학습하여 LR 이미지에 대한 HR 이미지를 재구성하는 것이 효율적인 방법
  * DBPN은 큰 필터를 이용하여 복잡한 계산을 가지고 있음, back-projection Net을 통해 얻어진 다수의 HR 특징들로부터 HR 영상을 복원하는 구조
  * SISR의 효율성과 정확도 사이에서 좋은 균형을 달성하기위해 LR와 HR 공간에서 Hybrid Residual Feature를 학습할 수 있는 Compact back projection Net을 제안 

Realted Work
------------
  * VDSR, DRRN, BAN, EDSR, MemNet 등과 같은 SOTA Net들은 높은 SR 정확도를 얻는데 주 목표를 하고 있지만 많은 operations 와 parameters를 가져 저전력 컴퓨터, 실시간에 적용하기 어려움 
  * CARN 저자는 cascading residual block을 이용하여 계산 복잡성을 줄였고 group conv, point wise conv를 사용하여 더 적은 모델을 만듦
  * 제안된 네트워크는 deconv 대신 sub pixel conv를 이용함으로써 operation을 줄였으며 채널 크기를 줄여 SR Net을 압축 하였음
    
Proposed Method
---------------
![image](https://user-images.githubusercontent.com/61686244/110328999-6504e480-805f-11eb-9961-178429b3ed4d.png)

  * 3단계로 구성 : ⓵ Low-level feature Extraction Module, ⓶ Compact Projection Module, ⓷ Reconstruction Module

![image](https://user-images.githubusercontent.com/61686244/110329091-836ae000-805f-11eb-9a0f-a0fec6c4bdee.png)

  * ![image](https://user-images.githubusercontent.com/61686244/110329135-91206580-805f-11eb-9028-2f694fa00361.png)는 각각 t 번째 UD Block의 LR 특징과 HR 특징을 의미

Channel Attention
-----------------
![image](https://user-images.githubusercontent.com/61686244/94658338-a0c3b180-033d-11eb-91f6-ebfbb9b0284d.png)
![image](https://user-images.githubusercontent.com/61686244/94658381-ae793700-033d-11eb-9448-b9d176b0817c.png)

* Attention : 사용 가능한 자원들 중 가장 유익한 자원 요소로 편향 시키는 방법
* LR 공간에는 풍부한 저 주파수와 가치있는 고 주파수의 성분들을 가지고 있음
* 기존의 Conv layer의 각 채널들은 local receptive field 에서 수행을 하게 돼서 결과적으로 Conv후에 output은 local 영역 밖의 정보에 접근을 할 없음
* 채널 별 글로벌 공간 정보를 얻기 위해 입력으로 들어온 채널을 global average pooling을 적용하여 single value로 만듦
* 비선형적으로 상호 작용을 할 수 있고 여러 채널의 기능을 강조 할 수 있는 gating mechanism을 sigmoid로 적용
* global average pooling과  gating mechanism sigmoid를 적용한 Channel attention을 residual block에 적용한 RCAB를 이용

Investigation of RIR and CA
---------------------------
* Train Set : DIV2K / Test Set : Set14, B100, Urban100, Manga109 
![image](https://user-images.githubusercontent.com/61686244/94658476-cf418c80-033d-11eb-8f36-3b35dfc5ec11.png)


BI degradation model
--------------------
![image](https://user-images.githubusercontent.com/61686244/94657626-aa98e500-033c-11eb-90b4-582f04839829.png)

![image](https://user-images.githubusercontent.com/61686244/94657657-b7b5d400-033c-11eb-865b-27d91d4669f2.png)

BD degradation model
--------------------
![image](https://user-images.githubusercontent.com/61686244/94657705-ce5c2b00-033c-11eb-9e5b-1625ef2c2da6.png)
![image](https://user-images.githubusercontent.com/61686244/94657722-d5833900-033c-11eb-8205-399c1a6645a3.png)

Object Recognition Performance
------------------------------
![image](https://user-images.githubusercontent.com/61686244/94657847-05cad780-033d-11eb-8ef1-f6e6e5af39cc.png)

Conclusions
-----------
* RIR, LSC, SSC을 이용하여 깊은 네트워크를 만들어 큰 Receptive field를 얻음
* RIR은 많은 skip connection을 통해 저 주파수 성분을 우회시켜 main network가 고 주파수 정보를 학습하는데 초점을 맞춤
* 각 채널들 사이의 상호 의존성을 고려하여 각 채널의 유익한 정도에 따른 채널을 재정의하는 Channel Attention을 제안
* RCAN은 SR, BI, BD degradation model에서도 좋은 성능을 발휘함
