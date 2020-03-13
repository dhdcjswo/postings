# PRML2-1. beta distribution



bernoulli 와 beta 분포간의 likelihood, prior 관계를 알아보자.



## Bernoulli likelihood

bernoulli 분포의 likelihood 형태인 binomial 분포는 다음과같다.
$$
Bin(m|N,\mu) = \begin{pmatrix} x \\ z \end{pmatrix} \mu^m(1-\mu)^{N-m}
$$
두가지 경우의 수에서 x=1인 사건이 일어날 확률 $\mu$와 x=0인 사건이 일어날 확률을 1-$\mu$ 라고하자.N번의 시행에서 x=1 인 사건이 'm' 번 일어난 경우에 대한 식이다. 이형태는 Binomial 분포라고도 한다.





## beta distribution

베타 분포는 연속확률분포로써 확률변수의 값이 [0,1] 사이에서 정의되는 분포이며 분포의 모양은 shape-parameter $\alpha$ , $\beta$   에 의해 결정된다.

베이지안의 관점에서 베타분포는 상당히 효과적인 분포이다. likelihood 함수가 Binomial 의형태일때, 사전분포를 베타 분포로 가정하면 사후분포 역시 베타분포이다. 이런 likelihood ~ prior 의 관계에 의해 사후 분포의 형태가 prior가 같아질때, 두 분포는 conjugate한 관계를 가진다고 이야기 한다. conjugate 관계는 사후분포를 계산하는데 효과적이다. 또 베타분포는 [0,1]사이의 값을 가지기 때문에 확률값에 대한 사전분포로 uniform 분포와 더불어 일반적으로 많이 사용된다.


$$
Beta(\mu|a,b) = \frac {\Gamma(\alpha+\beta)} {\Gamma(\alpha)\Gamma(\beta)}\mu^{\alpha-1}(1-\mu)^{\beta-1} \\ 
 \\
E[\mu] = \frac {\alpha} {\alpha+\beta} \\
Var[\mu] = \frac {\alpha\beta} {(\alpha+\beta)^2(\alpha+\beta+1))}
$$


hyper-parameter 인 $\alpha,\beta$ 가 평균과 분산을 결정한다. 



likelihood x prior의 곱은 지금의 예제에서 Binomial x Beta의 형태이고 두 분포 모두 $\mu$ 에 의존하는 형태이다 따라서 다음과같은 사후분포를 구할 수 있다.


$$
p(\mu|m,l,a,b) =\frac {\Gamma(m+a+l+b)} {\Gamma(m+a)\Gamma(l+b)} \mu^{m+a-1}(1-\mu)^{l+b-1}
$$


사후분포의 형태 역시 conjugate한 성질대로 베타분포의 형태를 띈다.



X=0 또는 X=1 인 두 사건중 한가지가 순차적으로 발생한다고 하자. 이런경우 posterior 분포의 업데이트는 직관적이면서 재미있다. 마찬가지로 beta 분포를 prior로 가정을 하면 (3)번 형태의 posterior을 얻을 수 있다. 이때 새로운 x=1 인 사건이 추가로 발생한경우 사후 분포는 m 대신 (m+1)을 넣으면 된다. 반대로 x=0인 사건이 추가로 발생하면 l 대신 (l+1)을 넣으면 된다.

Beta(a,b)인 분포의 형태에서 베르누이 사건이 하나씩 추가되면 Beta(a+1,b) 또는 Beta(a,b+1)과 같은 직관적인 형태로 사후분포가 업데이트 된다. 또 사건이 쌓이면서 (a+b)의  값이 커지면 서두의 분산의 식에서 분포가 계속 커지며 분포의 형태가 중앙 집중적으로 샤프한 형태를 띄게된다.이는 베이지안의 일반적인 특징인데 데이터가 늘어날수록 불확실성이 줄어듦을 의미한다.