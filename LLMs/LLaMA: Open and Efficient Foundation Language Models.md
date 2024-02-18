---

### Summary

#### LLaMA 
- 메타(페이스북)에서 2023년 공개한 언어 모델
- 공개적으로 사용 가능한 오픈 데이터셋으로만 학습되어 개방성과 효율성을 우선시하는 모델

<br>

#### Pre-training Data

1. English CommonCrawl [67%]
- 2017-2020 의 English Common Crawl 데이터셋
- 또한, 노이즈 제거를 위해 Wikipedia에서 참조로 사용되는 페이지와 무작위로 샘플링된 페이지를 분류하기 위해 linear model을 학습시켜 참조로 분류되지 않은 페이지들은 제외
2. C4 [15%]
3. Github [4.5%]
- Google BigQuery에서 제공되는 공개 GitHub 데이터셋
4. Wikipedia [4.5%]
- 20개 이상 언어의 2022 6-8월 동안의 Wikipedia 데이터셋
5. Gutenberg and Books3 [4.5%]
- Gutenberg project의 책들과 ThePile Books3 섹션에 포함된 책들의 말뭉치
6. ArXiv [2.5%]
- arXiv Latex files
7. Stack Exchange [2%]
- 다양한 도메인에 걸쳐 컴퓨터 과학에서 화학까지 다양한 주제를 다루는 높은 품질의 질문과 답변을 제공하는 Stack Exchange Dump

  <br>
  
![image](https://github.com/Juxhee/Papers/assets/60679596/fdbae40a-559f-459a-b45c-5f1c3a66d769)

<br>

#### Architecture
- Transformer  Architecture를 기반으로 하며, 성능 향상 등을 위해 아래와 같이 하이퍼파라미터 등을 조정
  
##### Pre-normalization [GPT3]
- 안정적인 학습을 위해 각 Transformer sub layer input 을 normalization
- RMSNorm normalizing function 활용

##### SwiGLU activation function [PaLM]
- 성능 향상을 위해 SwiGLU activation function 사용

##### Rotary Embeddings [GPTNeo]
- 각 레이어에서 rotary positional embeddings(RoPE)을 추가


<br>

##### 추후 공부할 내용
- Instruction Tuning
-

