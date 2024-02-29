---

### Summary

#### Sentence Bert
- BERT는 STS(semantic text similarity task)에서 연산량 많이 필요
- 샴 네트워크 구조를 활용한 BERT 파인튜닝으로 문장 임베딩 성능 개선 및 computational cost 감소
- 기존 BERT의 문장 임베딩 방법
   1) BERT의 [CLS] 토큰의 표현 벡터를 문장 표현으로 사용
   2) BERT의 모든 단어의 feature vector를 평균 pooling
   3) BERT의 모든 단어의 feature vector를 최대 pooling
      
<br>


<br>

#### Architecture

![image](https://github.com/Juxhee/Papers/assets/60679596/d74bd12a-62f6-49d7-885d-0674e0c84fe4)


<br>
- 동일한 layer, weight을 공유하는 network에 문장을 각각 넣어 나온 임베딩 벡터들의 거리를 비교하여 유사도를 계산 
<br>
- 비슷한 문장은 가깝고, 비슷하지 않은 문장은 멀게 학습하도록 함
<br>
- SNLI Dataset(Entailment, Neutral, Contradiction)으로 파인튜닝
<br>
- Classification(왼) : 두 문장 임베딩 값의 similarity 를 softmax 거쳐 확률값으로 바꾸고, cross entropy로 업데이트
<br>
- Regression(오) : 두 문장 임베딩 값의 similarity를 MSE로 업데이트


