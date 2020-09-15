# 강화학습을 접목한 협업 필터링 모델
<button onclick="location.href='https://github.com/leee5495/RL_RBM'" type="button">&#128193; GitHub Repository</button>
<br><br>

### 연구 동기
---
RBM 추천 시스템의 단점
1. 사용자 평점의 **시간 관계**를 고려하지 않는 입력<br>
   → 변화할 수 있는 사용자의 취향을 반영하지 못한다
   
2. 새로운 데이터가 생기면 위해 **전체 데이터를 이용해 다시 학습**<br>
   → 새로운 데이터에 대해 시스템을 빠르게 업데이트하지 못한다
   
3. **클릭, 장바구니 같은 사용자의 액션**들은 고려하지 않는다<br>
   → 평점보다 더 유의미할 수 있는 데이터를 사용하지 못한다

<br><br>

### 제안방법
---
Enhanced Collaborative Filtering with Reinforcement Learning
<br>

<img src="images/rlrbm.png?raw=true"/>
<br>

<img src="images/rlrbm_algorithm.png?raw=true"/>
<br>

1. RBM의 출력으로 각 아이템에 대한 점수 r̂을 얻는다
2. Half normal distribution을 이용해 K개의 랭킹 값을 추출한다
3. 추출한 값에 해당하는 랭킹을 가지는 아이템을 선택하고 left out item과 비교해 적절한 보상을 부여한다
4. r̂ 에 대한 랭킹을 gradient를 찾을 수 있는 근사치로 계산하고 Half normal distribution 식에 대입해 보상에 대한 policy gradient를 얻어 RBM의 파라미터를 학습한다

<br><br>

### 실험 데이터 설명
---
MovieLens 평가 데이터세트를 이용해 추천 시스템에 대한 성능 평가 진행
- MovieLens 웹사이트에서 수집된 영화 평가 데이터 세트
- 추천 시스템의 성능을 검정하기 위한 벤치마크 데이터 세트로 자주 사용
- 100,000개의 평가 데이터로 구성된 MovieLens100K와 1,000,000개의 평가 데이터로 구성된 MovieLens1M을 사용함

&nbsp; | 사용자 수 | 아이템 수 | Rating 수 | 평가   밀도(density)
-- | -- | -- | -- | --
MovieLens100K | 1,000 | 1,700 | 100,000 | 5.88%
MovieLens1M | 6,000 | 4,000 | 1,000,000 | 4.17%
  
<br><br>

### 기본 RBM과의 성능 비교
---
MovieLens100K 결과

![image](https://user-images.githubusercontent.com/39192405/93019674-7bcc0080-f613-11ea-8844-c96b4651236a.png)

    | RLRBM | RBM
  -- | -- | --
  HR@10 | 0.1481 | 0.1217
  HR@25 | 0.2169 | 0.2169
  ARHR | 0.05986 | 0.05152


MovieLens1M 결과

![image](https://user-images.githubusercontent.com/39192405/93019679-7ff81e00-f613-11ea-8fe0-c23c9138c6dc.png)

    | RLRBM | RBM
  -- | -- | --
  HR@10 | 0.09272 | 0.08940
  HR@25 | 0.1813 | 0.1763
  ARHR | 0.04423 | 0.04353


- 아이템과 사용자 수가 더 적고 **평가의 density가 높은** MovieLens100K에 대해선 **눈에 띄는 성능 향상**을 볼 수 있다.
- 하지만 아이템과 사용자 수가 많고 **density가 낮은** MovieLens1M에서는 **큰 변화를 볼 수 없다**.
  
<br><br>

### 지도학습을 통해 Left out item에 대한 추가적인 학습이 진행된 RBM과 성능 비교
---
MovieLens100K 결과

![image](https://user-images.githubusercontent.com/39192405/93019768-0ad91880-f614-11ea-9cf2-6cfcbfb58b5f.png)

    | RLRBM | SUPERVISED
  -- | -- | --
  HR@10 | 0.1481 | 0.1217
  HR@25 | 0.2169 | 0.2275
  ARHR | 0.05986 | 0.05036


MovieLens1M 결과

![image](https://user-images.githubusercontent.com/39192405/93019797-32c87c00-f614-11ea-8296-b15c7ec2c950.png)

    | RLRBM | SUPERVISED
  -- | -- | --
  HR@10 | 0.09271 | 0.08858
  HR@25 | 0.1813 | 0.1738
  ARHR | 0.04423 | 0.04557

- 같은 Left out item에 대한 학습을 강화학습으로 했을 때와 지도학습으로 했을 때의 성능 비교 결과 **강화학습의 성능이 대부분 더 높은 것을 확인했다**.
- 지도학습은 하나의 사용자 상태에 대해 하나의 분명한 정답을 학습시키는 것에 반면, 강화학습은 사용자의 상태에 대해 **확률적으로 보상을 분배**하기 때문에 더 효과적인 것으로 보인다.
  
<br><br>

### 강화학습 훈련을 진행할 때 선택하는 아이템의 개수 K에 따른 효과 비교
---
![image](https://user-images.githubusercontent.com/39192405/93019820-699e9200-f614-11ea-8670-5834469d5c45.png)

- **작은 크기의 데이터세트에서는 작은 K 값**의 성능이 대부분 더 높은 경향을 띄는 것을 볼 수 있다.
- **큰 크기의 데이터세트에서는 상대적으로 더 큰 K 값**을 사용할 때 성능이 높아지는 것을 확인할 수 있었다.
- 아이템과 사용자의 수가 많은 데이터일수록 더 큰 K 값을 사용해 사용자의 피드백을 더 많이 얻는 것이 강화학습 훈련에 더 효과적인 것으로 보인다.
  
<br><br>

### 결론 및 개선점
---

&nbsp;`결론`
- 사용자와 교류하면서 추천에 대한 반응을 반영하는 **강화학습 기반의 추천 모델 설계**
- 다양한 실험을 통해 강화학습 훈련으로 RBM의 가중치를 추가적으로 학습시킬 때 추천의 성능이 높아질 수 있다는 것을 증명
- **미리 학습된 협업 필터링 모델을 기반**으로 사용자와 아이템의 상관관계를 더 깊게 학습할 수 있다
- 제안한 학습 방법을 통해 **사용자의 취향을 실시간**으로 추천에 대한 반응을 이용해 학습할 수 있다

&nbsp;`개선점`
- 사용자의 리뷰 데이터뿐만 아닌, 사용자의 Implicit한 취향 정보가 될 수 있는 **클릭, 장바구니 등의 정보를 포함하는 데이터 세트**를 사용해 적절한 reward를 설정한다면 더 좋은 결과를 얻을 수 있을 것이라 생각
- 실시간(on-line)으로 추천에 대한 고객의 반응을 얻는 것을 전제로 설계된 모델이기 때문에 **실제 사용되는 시스템에 적용**하는 것이 가장 적절한 훈련 방법이라 생각








