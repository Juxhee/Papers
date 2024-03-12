---

### Summary

- Sentence embedding은 NLP에서 중요한 문제. Contrastive objective가 효과적임
- Sentence Bert와의 차이 ? dropout만으로 positive pair 구성할 수 있기 때문에 unsupervised 가능

##### Contrastive Learning

![image](https://github.com/Juxhee/Papers/assets/60679596/5ad80deb-d217-4fea-aef2-31b09f29bb17)

- 이웃은 가깝게, 이웃이 아닌 데이터는 멀게 밀어내 효과적으로 representation을 학습하는 것이 목표
- pair의 유사도 값으로 cross entropy
- vision에서는 crop, rotation 등 augmentation을 수행하지만 nlp는 의미가 바뀔 수 있기 때문에 위험 (contrastive learning을 위해 data augmentation 하는게 label의 정보를 해칠 수도 있음)
      
<br>


<br>

#### Architecture

![image](https://github.com/Juxhee/Papers/assets/60679596/2de4d143-062d-44e6-83ef-954e785c3285)

**points**
- unsupervised learning을 위한 positive pair은 같은 문장에 dropout 적용한 문장으로 구성
- negative pair은 같은 배치 내에 있는 다른 문장으로 구성

