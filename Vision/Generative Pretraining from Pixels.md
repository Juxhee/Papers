## Introduction

- 자연어 처리에서 쓰이는 모델들이 image에 대해서도 좋은 representation을 생성하는지 실험해보고자 함
- image를 pixel로 이루어진 긴 시퀀스라고 보고 auto-regressive하게 pixel을 예측하게 하는 sequence transformer를 학습

## Approach

<img src="https://github.com/Juxhee/Papers/assets/60679596/abe572b7-af4f-42d0-9721-f4a9a84b4974" width="300" height="300"/>

<br>

self supervised learning 방식으로 모델을 학습




<br>


1. 이미지를 low-resolution으로 리사이징한 뒤 1d sequence 모양으로 바꿔줌
<img src="https://github.com/Juxhee/Papers/assets/60679596/bf9f571d-f35e-4731-a78a-c3436211e59f" width="700" height="450"/>

2. Pre-training 단계 (두 가지 방법 중 하나 선택)
<img src="https://github.com/Juxhee/Papers/assets/60679596/507e2d3c-544d-4611-8f8c-ce057a437f83" width="400" height="100"/>


 2.1 Autoregressive 
- 이전 픽셀 정보들을 활용하여 다음 픽셀을 예측
<img src="https://github.com/Juxhee/Papers/assets/60679596/f0795d5c-94f8-4d18-97ca-6ed8d104ed11" width="400" height="100"/>



<br>
<br>

 2.2 BERT

- input sequence의 픽셀들을 마스킹하고 앞 뒤 픽셀 정보들을 모두 활용하여 마스킹된 부분의 픽셀을 찾음

 <img src="https://github.com/Juxhee/Papers/assets/60679596/a4548a8f-a621-4e42-a275-c5f151662574" width="400" height="100"/>

<br>

 <img src="https://github.com/Juxhee/Papers/assets/60679596/8a58a097-cf97-4498-90de-bdf55b487b78" width="400" height="320"/>

<br>

<br>

<br>

3. **성능 평가**

3.1 finetuning
- 마지막 feature vector를 average pooling해서 projection한 다음 cross entropy loss로 계산

3.2 linear probe

- 마지막 레이어보다 중간 레이어가 좋다고 해서, 그거 빼고 뒤에 FC를 붙여서 뒷부분만 finetuning하여 classification
- 성능이 좋은 중간 layer의 feature를 사용하여 class logit 생성

<br>


## Methodology

**Dataset and Data Augmentation**

- ImageNet : training set의 4%를 validation set으로 사용 / 원래 validation set을 test set으로 사용
- CIFAR-10, CIFAR-100, STL-10은 10%를 validation set으로 사용
- Web images들에 대해 pre-train 시킬 때는 Data Augmentation 적용하지 않음
- ImageNet에 대해 pre-train / fine-tuning할 때는 resize, crop등 간단한 Augmentation 적용

**Context Reduction**

- transformer decoder의 memory 문제 → 이미지 resize (원래 : 224x224x3 ⇒ 15만 sequence)
- 이미지를 low resolution(저해상도)으로 변경 (Input Resolution IR)
 <img src="https://github.com/Juxhee/Papers/assets/60679596/09d3179c-50e9-4b7d-a5df-8ac7c5963dc2" width="350" height="30"/>


- RGB 픽셀값들을 k=512의 k-means 를 통해 클러스터링 → RGB채널을 하나로 보게 됨, context length 를 3배 줄임 (Model Resolution MR)
 <img src="https://github.com/Juxhee/Papers/assets/60679596/0bb7dbe1-66cd-46d0-9804-e50b481e9d80" width="220" height="30"/>


<br>

<br>

**Model**

iGPT-XL : L= 60 layers, d = 3072,  6.8B parameters 

iGPT-L : L = 48 layers, d = 1536, 1.4M parameters

iGPT-M : L = 36 layers, d = 1024, 455M parameters

iGPT-S : L = 24 layers, d = 512, 76M parameters


<br>

## Experiments and results
 <img src="https://github.com/Juxhee/Papers/assets/60679596/d4085ad3-6442-456c-8401-b3ac1965a883" width="500" height="350"/>


- layer별로 representation 성능 확인한 결과 layer 깊어질수록 성능 올라가다가 중간 이후로 다시 떨어짐
- generative model을 linear probe로 평가하기 위해서는 best layer를 찾는 것이 중요하다고 주장함

<br>

<br>

 <img src="https://github.com/Juxhee/Papers/assets/60679596/3cd9599e-968d-4089-938b-d3666a745136" width="500" height="420"/>

- validation 성능 높을 수록 linear probe 성능 좋다 → 생성 잘하는 모델이 분류 성능도 좋다
- 모델이 클 수록 좋다

<br>

<br>

 <img src="https://github.com/Juxhee/Papers/assets/60679596/8b9717d6-9b7c-4dca-aaf5-2b1945afe9a7" width="500" height="550"/>

- ImageNet으로 pre-train시키고, 나머지 세 데이터셋으로 linear probe 실험했을 때 iGPT가 가장 좋은 성능을 보임

  <br>

 <img src="https://github.com/Juxhee/Papers/assets/60679596/bb469438-eaff-4dc4-bcc9-b059c1c27687" width="500" height="550"/>

 <br>
- Contrastive learning 방법의 SimCLR보다 parameters 많지만 견줄 수 있는 성능을 보여준다고 주장

- 2D input structure 없이 low-resolution에서 좋은 성능
