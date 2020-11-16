# Enhanced Collaborative Filtering with Reinforcement Learning
<button onclick="location.href='https://github.com/leee5495/RL_RBM'" type="button">&#128193; GitHub Repository</button>
<br><br>

### Research Motivation
---
Disadvantages of RBM based recommendation systems
1. Inputs do not take into account the **time relationship** of user ratings<br>
   → Doesn't reflect the user's change in taste
   
2. Must train on the **whole data** to account for new data<br>
   → Cannot update the system fast enough to the incoming data
   
3. **Implicit feedback data** is not considered<br>
   → Cannot utilize data that can be more meaningful than explicit rating data

<br><br>

### Suggested Model
---
Enhanced Collaborative Filtering with Reinforcement Learning
<br>

<img src="images/rlrbm.png?raw=true"/>
<br>

<img src="images/rlrbm_algorithm.png?raw=true"/>
<br>

1. Obtain score r̂ for each item from the output of RBM
2. Extract K rank values from Half normal distribution
3. Select items ranked equivalently to the extracted rank values and produce approporiate rewards by comparing to the left-out items
4. Calculate ranks based on r̂ approximately and apply it to the Half normal distribution to obtain the policy gradients to train the RBM's parameters

<br><br>

### Experimentation Data
---
Used MovieLens dataset to perform recommendation system evaluation
- Movie rating data from the MovieLens website
- Used frequently to evaluate recommendation systems
- Selected MovieLens100K, which has 100,000 rating data and MovieLens1M, which has 1,000,000 rating data

&nbsp; | Users | Items | Ratings | Rating density
-- | -- | -- | -- | --
MovieLens100K | 1,000 | 1,700 | 100,000 | 5.88%
MovieLens1M | 6,000 | 4,000 | 1,000,000 | 4.17%
  
<br><br>

### Comparison to Original RBM
---
MovieLens100K

![image](https://user-images.githubusercontent.com/39192405/93019674-7bcc0080-f613-11ea-8844-c96b4651236a.png)

    | RLRBM | RBM
  -- | -- | --
  HR@10 | 0.1481 | 0.1217
  HR@25 | 0.2169 | 0.2169
  ARHR | 0.05986 | 0.05152


MovieLens1M

![image](https://user-images.githubusercontent.com/39192405/93019679-7ff81e00-f613-11ea-8fe0-c23c9138c6dc.png)

    | RLRBM | RBM
  -- | -- | --
  HR@10 | 0.09272 | 0.08940
  HR@25 | 0.1813 | 0.1763
  ARHR | 0.04423 | 0.04353


- For MovieLens100K with fewer items and users and **higher rating density**, we could see an **outstanding improvement in recommendation performance**
- But for MovieLens1M with **lower rating density**, we **couldn't see much change in the recommendation performance**
  
<br><br>

### Comparison to Supervised Learning Approach for Additional Training to the Left-out Data
---
MovieLens100K

![image](https://user-images.githubusercontent.com/39192405/93019768-0ad91880-f614-11ea-9cf2-6cfcbfb58b5f.png)

    | RLRBM | SUPERVISED
  -- | -- | --
  HR@10 | 0.1481 | 0.1217
  HR@25 | 0.2169 | 0.2275
  ARHR | 0.05986 | 0.05036


MovieLens1M

![image](https://user-images.githubusercontent.com/39192405/93019797-32c87c00-f614-11ea-8296-b15c7ec2c950.png)

    | RLRBM | SUPERVISED
  -- | -- | --
  HR@10 | 0.09271 | 0.08858
  HR@25 | 0.1813 | 0.1738
  ARHR | 0.04423 | 0.04557

- Comparing two different training methods for the same left-out items, we found that the **performance of Reinforcement Learning is greater than Supervised Learning**
- It seems that whereas Supervised Learning can only train to prefer one specific item to a user's state, Reinforcement Learning can **probabilistically distribute rewards** across a user's state
  
<br><br>

### Comparison of Different Values of K
---
![image](https://user-images.githubusercontent.com/39192405/93019820-699e9200-f614-11ea-8670-5834469d5c45.png)

- **For a smaller dataset, a smaller value of K** has better recommendation performace
- **For a bigger dataset, a bigger values of K** has better recommendation performance
- For data with larger number of items and users, it will be more effective to user bigger value of K to obtain more user feedback during Reinforcement Learning training
  
<br><br>

### Conclusion
---

&nbsp;`Conclusion`
- Designed a recommendation system that can **reflect user feedback** using Reinforcement Learning
- Through various experiments proved that additional recommendation model training with Reinforcement Learning can improve recommendation performance
- Using a **pretrained Collaborative Filtering model**, can learn more deeply about the correlation between users and items
- Through the proposed learning method, **User's change in taste can be learned in real-time**

&nbsp;`Future Work`
- **Use of implicit feedback data** such as clicks and setting the approporiate rewards for those actions will result in better recommendation performance
- **Applying the method to a real-world system** so that the model can gather feedback on-line is the most advisable method of evaluating/training the proposed system
  
<br><br>
