# Poisson and Exponential



지수분포~포아송분포 간략정리.



## Exponential

지수분포는 연속확률분포로,  일반적으로 독립적인 사건 사이의 시간에 대한 분포로 사용된다. 모수로는 'rate parameter'로 불리는 $\lambda$를 가지고 있고 분포의 범위는 $[0, \infin]$이다.
$$
f(x) =\begin{cases} \lambda e^{\lambda x} & x \ge 0, \\
0  & x \le 0 
\end{cases}
$$
CDF의 형태는 다음과 같다.
$$
F(x) = \begin{cases} 1-e^{\lambda x} & x \ge 0 \\
0 & x <0
\end{cases}
$$


독립적인 사건 사이의 평균대기 시간이 $\lambda $ 인  확률분포이다.



**memorylessness**

지수분포의 특징은 비기역성이다. 다음 사건까지의 평균 대기시간은 대기하기 시작한 시간과는 무관하다. 따라서 버스를 20분을 기다렸다고 하면 20분전이나 지금이나 그 상황이 다르지 않다.
$$
\begin{matrix}
P(X>t+s|X>t) &=& \frac {P(X \ge t + s)} {P(X \ge s)} \\
&=& \frac {e^{-\lambda (t+s)}} {e^{-\lambda t}} \\
&=& e^{-\lambda s} \\
&=& P(X>s)
\end{matrix}
$$





## Poisson

포아송 분포는 이산확률분포로, 지수분포와 마찬가지로 범위 $[0,\infin]$ 에서 모수 $\lambda$를 가진다. 일반적으로 포아송 분포는 주어진 시간에 발생하는 사건의 횟수를 다루는경우의 분포로 많이 사용된다.
$$
f(x) = \frac {\lambda^k e^{-\lambda}} {x!}
$$
주목할점은 $\lambda$ 가 커지면 커질수록 분포의 형태가 대칭적으로 변한다.  **$ \lambda $ 가  10 이상이 되면 포아송분포는 평균과 분산이 모두 $\lambda$ 인 정규분포와 형태가 비슷해진다.**





### exponential~poisson

매 시간 마다 평균적으로 $ \lambda$ 만큼의 사건이 발생하는 경우, 두 독립적인 사건 사이의 평균 대기 시간은 모수가 $\lambda$ 인 지수 분포를 따르며, 매시간마다 발생한 사건의 횟수는 모수가 $\lambda$인 포아송 분포를 따른다.

지수분포의 경우는 그 단위가 연속적인  **시간** 이다. 독립적인 두 사건 사이의 평균시간. 반면에 포아송분포의 경우는 단위가 **횟수**이다. $exp(\lambda)$ 와 $Poi(\lambda)$ 의 모수를 1이라고 생각해보면 지수분포의 경우는 사건 간 평균 단위시간이 걸리는 것이고 포아송 분포의 경우는 단위 시간당 1번의 사건이 발생하는것이다.

포아송의 $\lambda$ 가 커지면 단위시간당 발생하는 횟수가 많아짐을 의미하고 이는 사건당 평균대기시간이 짧아짐을 생각할 수 있다. 이런 의미에서 지수분포의 모수는 $1/ \lambda$ 로, 포아송분포의 모수는 $\lambda$ 로 표기하는 것이 의미적인 유사성을 고려한 표기라고 할 수 있다.





## reference

https://freshrimpsushi.tistory.com/296?category=696569