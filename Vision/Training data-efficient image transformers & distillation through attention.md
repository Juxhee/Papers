---

### Overview

#### Knowledge Distillation?
- 미리 잘 학습된 **큰 네트워크(Teacher network)**의 지식을 실제로 사용하고자 하는 **작은 네트워크(Student network)**에게 전달하는 것
- 목적 : 작은 네트워크도 큰 네트워크와 비슷한 성능을 낼 수 있도록, 학습 과정에서 큰 네트워크의 지식을 작은 네트워크에게 전달하여 작은 네트워크의 성능을 높이겠다

---

### Summary

#### Distillation through attention
##### Soft Distillation (Hinton et al.(2015))
- teacher model의 softmax와 student model의 softmax사이의 KL-divergence 를 최소화 
![image](https://github.com/Juxhee/Papers/assets/60679596/7480c28c-fef4-40f1-99c1-f9ba780524e5)
- 왼쪽 항 : student model의 output과 true label과의 cross entropy를 계산
- 오른쪽 항 : student와 teacher의 softmax값을 smoothing 해서 KL-divergence를 구함
  
<br>

##### Hard Distillation (DeiT)
![image](https://github.com/Juxhee/Papers/assets/60679596/f979501d-26db-4414-bf83-07c614eb9a96)

- 왼쪽 항 : student model output과 true label간의 cross entropy를 계산
- 오른쪽 항 : student model output과 teacher model output의 argmax하여 얻은 encoding된 hard한 값(yt)과의 cross entropy를 계산
- soft distillation의 λ와 τ가 없어졌기 때문에, 하이퍼파라미터로부터 자유로움

<br>

##### Distilation Token

![image](https://github.com/Juxhee/Papers/assets/60679596/50485195-c74a-458d-9f55-cbcdb7d09a5d)

- distillation token이라는 새로운 토큰을 임의로 추가 (초기값 랜덤)
- distillation token은 class token과 동일한 방식으로 동작하며 다른 임베딩들과 Self Attention을 통해 상호작용하며 값이 변하게 되고, 최종 output을 출력
- 각각의 output은 distillation loss와 ground truth loss를 통해 back propagation으로 학습
- class token과 distillation token의 최초 코사인 유사도는 0.06으로 다른 벡터값이지만, 학습을 진행해나감에 따라 cosine 유사도가 점차 올라감 (0.93)

→ distillation token은 class token과는 조금 다른 방향으로 학습되어 감
    ex) 원래 레이블은 고양이지만, teacher model에서 오는 정보는 고양이가 아닐 수 있기 때문
→ distillation을 통해 teacher model의 정보가 전달됨
- 반면 똑같은 target label을 가지는 class token을 단순히 하나 더 부착했을 때는
→ 처음에 분명히 초기값을 임의로 다르게 배정하였음에도 불구하고, 학습을 진행하면서 두 개의 토큰은 거의 동일한 (cosine 유사도 0.999) 벡터로 Converge 되어 버리고, output 또한 사실상 같은 값이 나오게 되어 분류 성능에 전혀 영향을 미치지 않음
- test시에는 output이 2개가 나오게 됨 (class token, distillation token)
      1)  두 개를 concat해서, linear classifier 붙여서 infer
      2)  두 개를 더함

<br>

---

### Experiments

#### accuracy & speed trade-off graph

![image](https://github.com/Juxhee/Papers/assets/60679596/3f2faf29-1d4b-414c-8f25-91577c64832e)

- ViT-B : ImageNet
- ViT-L : ImageNet-21k
- ViT-H : JFT-300M
- 증류기 기호 : Distillation token을 추가한 모델

<br>

#### 모델 크기 비교 
![image](https://github.com/Juxhee/Papers/assets/60679596/096a8916-d97f-493f-ae9b-a248d52c5fbc)

- DeiT-B : ViT-B와 동일한 구조
- DeiT-S : small model
- DeiT-Ti : Tiny model

<br>

#### Teacher model에 따른 성능 변화

![image](https://github.com/Juxhee/Papers/assets/60679596/9dbfabcb-b294-437d-a780-aaa63137dfa8)
- CNN 계열의 모델을 teacher model로 사용했을 때 성능이 더 높음
      → teacher model로부터 inductive bias를 가져올 수 있다는 기존 연구 결과를 언급

<br>

#### Distillation에 따른 성능 변화

![image](https://github.com/Juxhee/Papers/assets/60679596/f8fd691e-12fe-4667-83fe-c010dc9f6e26)

- soft distillation보다 hard distillation의 성능이 좋음
- class token, distillation token을 모두 활용했을 때 성능이 좋음

<br>

### Results 
- DeiT는 비교적 적은 양의 레이블된 데이터를 사용하여도 뛰어난 성능을 달성할 수 있어, 데이터가 부족한 환경에서 유용하게 사용될 수 있음
