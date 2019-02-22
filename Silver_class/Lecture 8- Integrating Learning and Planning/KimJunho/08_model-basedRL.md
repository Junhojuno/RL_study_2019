## Model-Based RL

### Preview
- 직전 강의에서 experience로 policy를 직접 학습시켰다.
- 이번엔 experience로 model을 직접 학습시켜보자.
- 이렇게 학습시킨 model을 사용해서 value function or policy를 만들어보자.
- **model을 learning하고 planning하는 integrating방식.**

### 이 방식을 사용하면 뭐가 좋은건가? 안좋은점은?
- **장점**
  - 효율적인 학습이 가능하다.(can efficiently learn model by supervised learning methods)
  - can reason about model uncertainty ---> ?
- **단점**
  - 2가지 error가 발생하게 된다.(model learning error, value estimation error)
  
### Model?
- model은 reward function과 transition probability로 나뉘는데, 이것을 같은 parameter로 parameterized시킨다.
- 그리고 **transition probability와 reward function가 서로 독립임을 가정한다.**
  ---> reward는 transition에 dependent하지 않나???
  
### Model은 어떻게 학습시키지?
- 먼저, experiences를 많이 뽑는다.
- 이걸 supervised learning problem으로 만들어 학습시킨다.
  - s1,a1 --> neural network --> r1 (regression)
  - s1,a1 --> neural network --> s2 (density estimation)
- loss function은 MSE, Cross Entropy 등등을 사용한다.
- 동일한 parameter로 학습시키는걸로 봐선 multi-output을 뱉어주는 network를 구성해야할 것 같다.

### An Example of Model : Table Lookup Model 
- 단순하게 평균내서 model을 estimation
- (s,a,s')단위로 방문횟수 평균을 해준다 ; (s,a)를 해서 (s,a,s')인 경우
- reward는 s'대신 방문해서 얻은 reward를 평균낸다.
- 가본곳만을 바탕으로 평균내기때문에 안가본곳이 나오면...? 이땐 neural network 등을 사용해야할듯.

### Planning with Model
- 위에서 구한 model을 통해 가상의 MDP <S,A,P_hat,R_hat>을 푸는 문제로 만든다.
- 이 문제를 푸는 방식은 DP를 이용해도 되고 등등 (policy/value iteration, tree search)

### Sample-Based Planning
- sample을 뽑는것에만 model을 사용하고, 학습은 model-free 적용.(간단하지만 강력하다고 함)
- DP의 full-width backup에 비해, 자주 나오는 sample에 집중하여 학습이 가능하다.(차원의 저주 해결?!)

### 부정확한 Model로 planning을 하게되면?
- 조지는거지 뭐...
- Model-Based RL은 model에 성능이 달려있다.
- 부정확한 model을 사용하게 되면, suboptimal을 죽어라 계산하게 될 것이다.
- 이에 대한 해결책은
  - model이 잘못되었을때, model-free RL을 사용한다.
  - reason explicitly about model uncertainty ---> bayesian model-based RL을 의미하는건가?
  
### Integrating Architectures
- Real Experiences(sampled by environment)와 Simulated Experiences(sampled by model)를 합친다.

### Dyna
- real experiences로부터 model을 학습하고
- learning과 planning value functino / policy ---> psedo code를 보면 둘이 합쳐진 형태를 볼 수 있음.
