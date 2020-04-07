# MCMC3_gibbsSampling



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