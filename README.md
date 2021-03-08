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
  * HR 이미지 공간에서 Residual를 학습하여 LR 이미지에 대한 HR 이미지를 재구성하는 것이 효율적인 방법
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
  * ![image](https://user-images.githubusercontent.com/61686244/110329235-ad240700-805f-11eb-8bee-faac4674b4ab.png)은 모든 UD Block에 의해 만들어진 LR 특징들을 압축된 값
  * ![image](https://user-images.githubusercontent.com/61686244/110329291-ba40f600-805f-11eb-8361-50a4a7bdbdb6.png)는 첫 번째 up sampling layer에 의해 통과된 값
  * ![image](https://user-images.githubusercontent.com/61686244/110329337-c88f1200-805f-11eb-9e37-58bb50566f1b.png)는 두 번째 up sampling layer의 출력 값
  * ![image](https://user-images.githubusercontent.com/61686244/110329373-d775c480-805f-11eb-95a3-0eff77fb5482.png)는 복원된 영상

  * ⓵ Low-level feature Extraction Module은 두 개의 Conv로 구성, 입력 영상을 특징 벡터 ![image](https://user-images.githubusercontent.com/61686244/110329442-ed838500-805f-11eb-9813-93a0f321a9e7.png)에 맞춰줌
  * ⓶ Compact Projection Module은 t 개의 UD 블록과 2개의 압축 층을 가지고 있음, UD Block은 dense 연결 되어 있고 LR, HR특징을 출력함 
  * 첫 번째 압축 층의 입력으로 ![image](https://user-images.githubusercontent.com/61686244/110329493-04c27280-8060-11eb-898d-150a9711712d.png) 들어가 1x1 Conv 통과
  * 첫 번째 up sampling 층의 입력으로 ![image](https://user-images.githubusercontent.com/61686244/110329554-1ad03300-8060-11eb-8577-34e977e41120.png)
  * F_H ![image](https://user-images.githubusercontent.com/61686244/110329627-39362e80-8060-11eb-8b27-1c1f633ef644.png)<= 두 번째 압축층과 up sampling  층 표현
  * ⓷ Reconstruction Module은 3x3 Conv를 사용하여 3채널로 만들어 복원하는데 사용
  * LR공간과 HR 공간의 특징을 결합하여 사용, UD Block에서 low level과 high level에서 만들어진 HR 특징은 복원을 하는데 사용이되며 이러한 사용은 높은 퀄리티로 HR영상을 만듦
  * LR 공간의 기능은 추가적인 저주파 정보를 제공하여 HR 기능과 함께 효과적인 재구성을 만듦
  
Projection Module
-----------------
![image](https://user-images.githubusercontent.com/61686244/110329751-5c60de00-8060-11eb-9100-94479e7e9751.png)

  * DBPN에서 영감을 받아 compact projection module 제안
  * 기존 DBPN을 3가지 측면에서 다르게 수정하여 정확도 감소 없이 만듦
  * 1) DBPN에서는 down projection으로부터 만들어진 LR 특징을 사용하지 않고 간과하였음, 반면에 제안된 네트워크는 HR 과 LR 특징을 모두 사용
  * 2) 복잡성을 낮추기 위해 DBPN에서 사용하던 deconv 대신 sub-pixel conv를 사용 
  * 3) UD Block의 학습 능력을 향상시키기 위해 local residual connection사용
  * 4x4 conv를 사용하여 HR space를 LR space에 매핑시킴
  
![image](https://user-images.githubusercontent.com/61686244/110329814-700c4480-8060-11eb-8d0d-b84d69ebe572.png)

![image](https://user-images.githubusercontent.com/61686244/110329829-74d0f880-8060-11eb-80c5-19d99e69833d.png)

![image](https://user-images.githubusercontent.com/61686244/110329851-7995ac80-8060-11eb-9a0c-29e7ac98261c.png)

![image](https://user-images.githubusercontent.com/61686244/110329861-7d293380-8060-11eb-8d7d-e2117c2b5869.png)

![image](https://user-images.githubusercontent.com/61686244/110329884-81ede780-8060-11eb-8ed4-4f3a286bfd6d.png)
  
  * duplication operation는 네트워크 내 LR특징의 가중치를 증가시키는데 목적을 둠
  * 종속된 up sampling, down sampling 층은 반복적으로 residual 특징을 학습하고, 반복적으로 기능을 개선하는 자체 수정 절차를 가짐

Experimental Results
--------------------
  * Train : DIV2K
  * Test : Set5, Set14, Bsd100, Urban100
  * SR Accuracy : PSNR, SSIM
  * Lightweight :　parameters, Multi-Adds로 평가

![image](https://user-images.githubusercontent.com/61686244/110330037-a5189700-8060-11eb-9afe-e60b603d7c61.png)

  * 위 그림에서 알 수 있듯이, UD Block을 8개 사용했을 때 각 Block의 HR을 사용하는 것보다 1, 4 8 번째 HR을 사용하는게 LR과 HR 특징을 활용하는데 있어 균형을 맞출 수 있음 
  * HR특징들이 좋은 성능에 영향을 미치긴 하지만 다수의 HR 특징은 균형을 깸
  
![image](https://user-images.githubusercontent.com/61686244/110330090-b366b300-8060-11eb-8192-5b0228dd2b2f.png)

  * CBPN-L : LR 특징들만 사용
  * CBPN-H : HR 특징들만 사용
  * CBPN : LR, HR 특징들 모두 사용
  * HR 과 LR 특징 공간에 이미지 SR에 유용한 보완 정보가 포함되어 있음을 의미 

![image](https://user-images.githubusercontent.com/61686244/110330144-c6798300-8060-11eb-952e-6f07f6bd2ee2.png)

![image](https://user-images.githubusercontent.com/61686244/110330152-ca0d0a00-8060-11eb-83a8-0309fe5e8106.png)

![image](https://user-images.githubusercontent.com/61686244/110330168-ced1be00-8060-11eb-9625-6a8f62570492.png)

![image](https://user-images.githubusercontent.com/61686244/110330187-d42f0880-8060-11eb-83bb-04c172c179e8.png)

![image](https://user-images.githubusercontent.com/61686244/110330194-d7c28f80-8060-11eb-8ca3-05c51418bf4e.png)



