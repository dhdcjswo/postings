# reparameterization MAP vs MLE



**reparameterization** 은 베이지안 통계학을 공부하다 보면 자주 접할 수 있는 용어이다. 아마 접해본 사람들이 있다면 기존의 모수를 변환시켜서 다른 형태로 접근하려는 시도이거나 Variation AutoEncoder(VAE)에서 등장하는 'reparameterization trick' 또는 코딩을 하면서 샘플러가 쉽게 수렴하지 않을 때 'reparameterizaiton' 의 방법을 사용해 풀어본 경험에서 일 것이다.



그렇다면 오늘 포스팅하려고하는 내용인 MAP 추정은 reparametrization에 대해서 invariance 하지않는 반면에 MLE는 invariant할까?



사후분포는 probability **density** function(pdf) 이다. 확률밀도함수이기 때문에 integral은 항상 1인 상수이고, 이를 유지하기 위해서는 파라미터 공간의 변화에따라 확률도 변해야한다.

![1](img/REPARAM_1.PNG)



반면 MLE는 확률밀도로 구하지 않는다. Likelihood 함수를 통해서 구하고 그렇기에 변화의 제한이 없다. 최댓값이 무엇이든 높이는 변하지 않는다. reparametrization 이후에도 최댓값은 변하지 않는다. 



likelihood는 data-space에서는 probability density 이다 p(x|&theta;) . 하지만 function은 L(&theta;|X) ,parameter-space에서는 더이상 probability density가 아니다. 이 말인 즉슨 intergral시에 공간상의 제약이 없다.반면 probability density는 제약을 받는다. 


$$
\int_A{p(y)dy} = \int_A {p(x)dx}
$$



이러한 제약이 reparametrization 시 probability densitiy를 변하게 만든다. 왜냐면 그냥 같은 함수값이 integral 했을때 같음을 보장하지 않기 때문에.



예를들어 보자 [0,10] uniform pdf 가 있다. 해당 공간에서 샘플링한 10000개의 데이터를 histogram으로 그리면 아래와 같다.

![2](img/REPARAM_2.PNG)



reparameterization을 다음과 같이 시행했다.
$$
x=[0,10] \rightarrow cos \space x
$$


바뀐 모수 공간에서 그려본 histogram은 아래와 같다

![3](img/REPARAM_3.PNG)



기존의 모든 공간에서 동일한 확률을 가지던것이 이제는 -1과 1 부근에서 더높은 확률을 가지게 되었다. 확률밀도함수 이기 때문에 제약을 유지하기 위해서 모수들의 공간도 변화한것이다.





## summary

reparameterization은 특히 베이지안 분석 시에는 주의해야 한다. 그 이유는 위에서 설명했듯이 모수들의 공간이 달라지기 때문이다. 반면 MLE를 통한 분석시에는 robust하다. Likelihood함수가 모수에 대한 밀도함수가 아니기 때문에 제약이 없기 떄문이다.









# reference

https://twiecki.io/blog/2017/02/08/bayesian-hierchical-non-centered/