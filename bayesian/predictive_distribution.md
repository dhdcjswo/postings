# predictive distribution



베이지안 분석을 진행하다 보면 *prior predictive check*  나 혹은 *posterior predictive check* 를 만나게 된다. 나 역시도 공부를 하다가 갑자기 튀어나온 모르는 용어에 구글링을 하며 정리를 해보았다. 

 모델링을 하고 MCMC를 이용하여 sampling을 마친 후에, 우리가 근사한 posterior 분포에서 새로운 데이터를 샘플링 하는 단계가 바로 *posterior predictive check* (ppc) 이다. 내가 만든 모델이 얼마나 잘 만들어졌는지를 평가하는 목적으로 사용하는 것 같은데 조금더 자세하게 살펴보자.







## prior predictive distribution

 

모델을 만드는 과정을 한번 생각해보자. 

모델을 만들면 우선 먼저 likelihood 함수를 정한다. likelihood 함수를 정하고 나서야 이 함수에 필요한 모수들에 대해서 prior, 사전분포를 정의할 수 있다. *prior은 사실 likelihood 함수에 아주아주 많이 의존하고 있고, 선택과 그 해석은 likelihood함수를 통해서 가능하다* 

베이지안 추론을 하기 위해서는 likelihood와 prior의 곱으로 이루어지는 joint distribution이 필요하다.; &pi; (y,&theta;)


$$
g(\theta|y) = \frac {f(y|\theta)g(\theta)} {f(\theta)} = \frac {\pi(y,\theta)} { f(\theta)}
$$


단번에 joint distribution을 정의하거나 아니면 likelihood와 prior을 각각 정의 해서 곱하든, 만들어진 joint distribution 에서 데이터을 추출했을 때 신뢰할만한 결과가 나와야한다. 다시말하면,  joint distribution은 일종의 시뮬레이터와 같은 역할을 하는데 **이 시뮬레이터에서 추출된 데이터는 우리가 데이터에 대해서 가지고 있던 범위,제약 등과 일치해야한다. ** 이를 확인할 수 있는것이 **prior predictive check** 이다. joint distribution을 이용해서 파라미터와 데이터의 셋을 만들어 낸 후 이 결과가 말이 되는지 확인하는 것이다. 주의할 점은 이 단계는`관측치, observation과는 무관하다는 점이다.`

prior predictive distribution은 관측치를 보기전에 모델의 구조를 이해하는데 도움을 준다. prior predictive distribution 에서 얻은 데이터들이 실제 관측한 observation의 패턴과 비슷하다면 우리는 꽤 괜찮은 모델을 만들었다고 생각해도 된다. 

prior distribution은 파라미터 &theta;에 대한 분포이고, prior predictive distribution은 observation(관측치)에 대한 (기대)분포이다. 수식은 아래와 같다.



$$
\begin{aligned}
p(x) &=\int_{\theta} p(x,\theta)d\theta \\
	 &=\int_{\theta} p(x|\theta)p(\theta)d\theta \\
\end{aligned}
$$


모수&theta; 에 대해서 X는 p(X|&theta;) 확률을 가진다. 하지만 실제로 모수 &theta; 는 우리가 모르는 값이다. 불확실하다.  그래서 가능한 모든 &theta; 값을 평균해서 x의 분포에 대해 더 나은 아이디어를 얻는다. 데이터의 분포다. 이 분포에서 샘플링을 했을 때 기존에 우리가 관측치에대해서 가지고 있던 제약이나 생각들과 일치하는 샘플이 추출되어야 한다.  







## posterior predictive distribution



이번에는 관측치와 사전분포를 통해서 사후분포를 만들었다고 하자.  prior predictive distribution은 관측치가 없는 상태에서 우리가 가진 데이터에 대한 생각과 우리가 만든 모델을 비교하는 작업이었다면, **posterior predictive distribution** 의 경우는 관측치가 존재하는 상태에서 우리가 만든 모델을 새로운 관측치와 비교하는 작업이다. 이는 모델이 데이터에 대해 일관성을 띄는지를 보여주고 그래서 사후 모델을 검증하는 용도로 많이 쓰인다.

posterior predictive distribution은 기존의 관측치가 주어졌을 때 새로운 관측치에 대한 예측분포이다. 따라서 조건부 확률인 $p(x_{new}|X)$ 로 나타낼 수 있다.
$$
p(x_{new}|X) = \int p(x_{new}|\theta,X)p(\theta|X)d\theta
$$

$$
p(x_{new}|x) = \frac {p(x_{new},x)} {p(x)}  
= \frac {\int_\theta p(x_{new}, x,\theta)d\theta} {p(x)}  
= \frac {\int_{\theta} p(x_{new}|x,\theta)p(x,\theta)d\theta} {p(x)}
= \int_{\theta} p(x_{new}|x,\theta)p(\theta|x)d\theta
$$



대부분의 상황에서는$\theta$가 주어지면, $x_{new}|\theta$ 는 x와 독립이다. 또  $x_{new}$와 x는 $\theta$가 주어졌을 때 conditionally independent 하다. 그러면 위의 식은 다음과 같이 바뀔 수 있다.
$$
p(x_{new}|x) = \int_{\theta}  p(x_{new}|\theta)p(\theta|x)d\theta
$$



$x$ 는 관측치이고 $\theta$ 는 모수들이라고 한다면 $f(x_{new}|\theta)$ 는 새로운 관측치에 대한 에측이다. 보통 프리퀀티스트 들이라면 MLE를 통해서 추정된 $\theta$ 값을 사용해 새로운 관측치에대한 예측을 할 것이다. 하지만 $\theta$ 에 대해서 우리는 정확하게는 모르고 사후분포에 의해 불확실성이 포함된 정보만을 가지고 있다. 따라서 사후분포의 평균을 구해서 새로운 관측치를 예측하는 분포가 posterior predictive distribution이다. 위 수식의 결과로  $f(x_{new}|\theta)$  대신에 $f(x_{new}|X)$ 가 사용되는 것이다. 불확실성이 포함된 분포이기 때문에 점추정치로 예측한 분포보다는 좀더 넓은형태를 띄게 된다.







## summary



일반적으로 위와 같은 predictive distribution은 베이지안 통계학에서만 등장한다. 프리퀀티스트 의 프레임 내에서는 모수는 '점' 이기 때문에 평균을 낼수가 없다. prior predictive distribution과  posterior predictive distribution 의 수식내에서의 차이는 단지 사전확률이 쓰였는지, 사후확률이 쓰였는지의 차이다. 다만 prior predictive distribution이 관측 전 사전에 모델에 대한 검증의 개념이라면 posterior predictive distribution은 사후 검증이다.



## reference

http://bebi103.caltech.edu.s3-website-us-east-1.amazonaws.com/2018/tutorials/t6a_model_generation_and_prior_predictive_checks.html

https://stats.stackexchange.com/questions/394648/differences-between-prior-distribution-and-prior-predictive-distribution

https://donghwa-kim.github.io/Pred_-baye.html

http://www.medicine.mcgill.ca/epidemiology/Joseph/courses/EPIB-668/predictive.pdf