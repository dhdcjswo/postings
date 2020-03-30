# MCMC2_sampling

​	

​	우리는 어떤 target distribution에서 sampling을 하려고한다. 이 target distribution은 우리가 구하고자하는 posterior distribution(사후분포)이라고 생각하면 된다. 문제는 모수가 증가함에 따라, 또는 일반적이지 않은 likelihood로 인해서 직접적인 계산이 힘들어 베이즈룰을 풀기가 불가능해질 때 발생한다. 다행이도 우리에게는 'MCMC'가 있다. MCMC는 샘플링을 바탕으로 목적함수에 근사시키는 방식이다. MCMC는 irreducible, 그리고 aperiodic한 성질을 가지는 markov chain이다. 여기에 더해서 positive recurrent 하다면 unique 한 stationary distribution을 가지게 되는데 이는 MCMC에서 우리가 샘플링을 하려는 목적함수인 사후분포로 수렴할 수 있게 해주는 바탕이 된다.

자 이제  MCMC의 가장 대표적인 두 알고리즘인 'metropolis-hastings' 와 'gibbs sampling' 에대해서 공부해보자

## metropolis-hastings



​	먼저 x가 주어졌을때, y값에 대한 밀도함수/커널 Q를 만들어본다고 하자. 이 Q는 현재의 위치 x에서 다음의 위치 y로 랜덤하게 이동하는 방법을 정하기 때문에 transitional kernel라고 한다. 간단한 예로 살펴보면,  $y = x+N(0,1)$ 로 기존의 지점 x 에서 표준정규분포에서 얻은 샘플링 값을 더하게 되면 $y \sim N(x,1)$  , 즉 평균이 x이고 분산이 1인 함수에서 샘플링된 값이 된다. 따라서 Q는  다음과같이 정의될 수 있다.
$$
Q(y|x) = 1/\sqrt{2\pi} *exp[-0.5(y-x)^2]
$$
새로 이동할 위치를 얻기위해 현재 위치 x에서 random number를 더해주는 위와같은 형식의 커널을 'random walk'커널이라 부른다. 이런 역할때문에 'proposal distribution' 이라고도 한다. 다음 위치에 대한 제안을 하는 것이기 때문이다. 

MH는 transition kernel Q를 이용해서 target dist $\pi$ 로부터 샘플링을 할 수 있게된다.

* 랜덤하게 초기의 state를 정한다.

* for t=1,2,..

  * 제안함수 Q로부터 y를 샘플링한다. $Q(y|x)$ 는 현재 제안된 상태이다 선택된게 아니다.
  * 그리고 이 제안을 받아들일지를 결정하는 (0,1) 사이의 acceptance probability A를 계산한다.

  $$
  A = min(1,\frac {\pi(y)Q(x_t|y)} {\pi(x_t)Q(y|x_t)})
  $$

  

  * A에 해당하는 확률로, 제안된 $Q(y|x)$ 를 $X_{t+1}$ 선택하고, 그렇지 않으면 $X_t$를 $X_{t+1}$로 선택한다. 



이게 metropolis-hastings 알고리즘의 전부이다. 비교적 간단한 알고리즘으로 마법같이 target distribution의 샘플들을 얻을 수 있다.



장단점

Some properties of the MH algorithm are worth highlighting. Firstly, the normalising constant of the target distribution is not required. We only need to know the target distribution up to a constant of proportionality. Secondly, although the pseudo-code makes use of a single chain, it is easy to simulate several independent chains in parallel. Lastly, the success or failure of the algorithm often hinges on the choice of proposal distribution. This is illustrated in figure 7. Different choices of the proposal standard deviation σ	 lead to very different results. If the proposal is too narrow, only one mode of p(x) might be visited. On the other hand, if it is too wide, the rejection rate can be very high, resulting in high correlations. If all the modes are visited while the acceptance probability is high, the chain is said to “mix” well



**Q. 왜 acceptance probability는 저 모양이어야 하는걸까? **

'A' 의형태를 저렇게 만드는 이유에 대해서 생각해보면, 

**Q. 목적함수를 어떻게 stationary distribution으로 만들수 있는가?**



**Q. target distribution 인 $\pi$ 는 우리가 모르는 함수인데 어떻게 acceptance probability 를 구할때 사용할 수 있는걸까? 우리가 target 알고있는거라면 샘플링의 이유도 없지 않은가?**

맞다. 우리는 $\pi$ 를 모르는데 사용하고 있다. 정확하게 말하면 우리는 목적함수 $\pi$를 모르는 게 아니라 계산을 못하는것 뿐이다.
$$
P(\theta|D) \propto P(D|\theta)P(\theta) = k*P(\theta|D)
$$
목적함수 $\pi$ 는 베이즈 룰에 의해서 likelihood x prior 의 비율의 형태로 정의 된다.  우리에게는 이미 likelihood 함수와 prior은 알려져 있는 정보이다.  정확한 수치는 모르지만 사후분포에 상수k가 곱해진 형태임을 알 수 있다.
$$
\begin{matrix}
A &=& min(1,\frac {k*\pi(y)Q(x_t|y)} {k*\pi(x_t)Q(y|x_t)}) \\
&=& min(1,\frac {\pi(y)Q(x_t|y)} {\pi(x_t)Q(y|x_t)}) \\
\end{matrix}
$$
이는 우리가 acceptance probability를 계산할 때 상수 k는 상쇄되고 실제목적함수에 대한 정보를 그대로 사용할 수 있게된다. **결론은**, 우리는 $\pi$  를 모르지만 $\pi$의 정보 그대로를 사용할 수 있게되어 문제가 되지 않는다.



**Q. up to constant of probability**

In particular, it is constructed so that the samples x(i) mimic samples drawn from the target distribution p(x). (We reiterate that we use MCMC when we cannot draw samples from p(x) directly, but can evaluate p(x) up to a normalising constant.)



The Markov chain has a [stationary distribution](https://en.wikipedia.org/wiki/Markov_chain#Steady-state_analysis_and_limiting_distributions) which is the distribution that preserves itself if you run it through the chain. Under certain broad assumptions (e.g., the chain is irreducible, aperiodic), the stationary distribution will also be the limiting distribution of the Markov chain, so that regardless of how you choose the starting value, this will be the distribution that the outputs converge towards as you run the chain longer and longer. It turns out that *it is possible to design a Markov chain with a stationary distribution equal to the posterior distribution, even though we don't know exactly what that distribution is*. That is, it is possible to design a Markov chain that has π(θ|x)π(θ|x) as its stationary limiting distribution, even if all we know is that π(θ|x)∝Lx(θ)π(θ)π(θ|x)∝Lx(θ)π(θ).



총평



## Gibbs sampling

종종 단변량 분포가 아닌 다변량분포에서 샘플링을 해야할 때가 있다. *gibbs sampling* 알고리즘은 conditional distribution에서의 샘플링이 joint distribution에서의 샘플링보다 수월할 때 유용하게 사용할 수 있는 특징을 지니는데 특히 다변량인 상황에서 주로 사용된다.

The Gibbs sampling algorithm is one MCMC technique that leverages detailed balance in order to produce a Markov Chain with the desired invariant distribution.

사실 gibbs sampling은 metropolis-hasting 알고리즘의 특별한 형태이다. metropolis hastings 알고리즘의 acceptance probability가 1인 형태이다.  gibbs sampling의 key 아이디어는 한번에 하나의 변량만 다루는 것이고, 다른 변수들은 고정되어있다고 생각하는것이다. joint distribution의 형태라면 하나의 벡터에 n차원의 모수들의 set을 한번의 시뮬레이션으로 찾아가게 되는데 gibbs sampling은 다른변수들을 고정시키고 한번에 하나의 차원씩만 시뮬레이션하면서 target을 찾아가게 되는것이다. 그게 더 쉽다.

우리가 샘플링하려는 target distribution $p(x,y)$ 이 있다고 생각하자. 우리의 목표는 각각의 marginal $p(x), p(y)$ 를 구하고 싶은 것이다. gibbs sampling은 $p(x|y) , p(y|x)$ 의 조건부 확률을 구한다.시작점은 랜덤하게 정한 어느 지점 $x_0 ,y_0$ 이다.

샘플링의 과정은 다음과 가타
$$
x_i \sim p(x|y=y_{i-1}) \\ 
y_i \sim p(y|x=x_{i})
$$
이 과정을 계속해서 반복하게 되면 목표함수에 도달할 수 있다.





, updating a particular variable involves simulating an outcome from its conditional distribution, given the latest values of all the other variables.

The primary reason why Gibbs sampling was introduced was to break the curse of dimensionality (which impacts both rejection and importance sampling) by producing a sequence of low dimension simulations that still converge to the right target. Even though the dimension of the target impacts the speed of convergence. Metropolis-Hastings samplers are designed to create a Markov chain (like Gibbs sampling) based on a proposal (like importance and rejection sampling) by correcting for the wrong density through an acceptance-rejection step. But an important point is that they are not opposed: namely, Gibbs sampling may require Metropolis-Hastings steps when facing complex if low-dimension conditional targets, while Metropolis-Hastings proposals may be built on approximations to (Gibbs) full conditionals. In a formal definition, Gibbs sampling is a special case of Metropolis-Hasting algorithm with a probability of acceptance of one. (By the way, I object to the use of *inference* in that quote, as I would reserve it for *statistical* purposes, while those samplers are *numerical* devices.)

Usually, Gibbs sampling [understood as running a sequence of low-dimensional conditional simulations] is favoured in settings where the decomposition into such conditionals is easy to implement and fast to run. In settings where such decompositions induce multimodality and hence a difficulty to move between modes (latent variable models like mixture models come to mind), using a more global proposal in a Metropolis-Hasting algorithm may produce a higher efficiency. But the drawback stands with choosing the proposal distribution in the Metropolis-Hasting algorithm.



## reference

https://stats.stackexchange.com/questions/344189/posterior-distribution-and-mcmc (체크)

https://stephens999.github.io/fiveMinuteStats/MH_intro.html

https://en.wikipedia.org/wiki/Detailed_balance#Reversible_Markov_chains

https://stats.stackexchange.com/questions/244573/when-would-one-use-gibbs-sampling-instead-of-metropolis-hastings

http://wellredd.uk/gibbs/