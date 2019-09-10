IEEE-II(2)

생성모델과 식별모델에 관한 연구가 활발히 진행되고 두 모델의 대립적 관계를 활용하여 두 모델을 학습시키는 GAN(Generative adversarial nets)은 학습방법에 대한 새로운 접근을 제시하였다. 각각의 모델이 반드시 신경망을 통해서 구현되어야하는 것은 아니지만 손쉽게 사용할 수 있는 API들이 많이 개발되면서 생성모델 또한 신경망으로 많이 구현하는 것 같다. 하지만 Point Process에 대해 공부하면서 독립변수가 특정사건의 시간과 관련된 경우, 신경망보다 전통적인 확률에 기반한 생성모델(stochastic-based generative model)이 연산량과 구현이라는 측면에서 보다 효율적이고 간편할 수 있겠다는 생각을 했다. 그래서 전통적인 확률에 기반한 생성모델로 클릭 분포를 modeling, 쉽게 구현하는 동시에 하이퍼 파라미터의 개수를 줄여 연산의 효율성도 재고하기로 하였다. 

 모델링하고자 하는 point process의 단위 시간당 이벤트 기대값은 단위 시간당 특정 사건이 발생할 확률 $$P$$, 발생하지 않은 확률 $$(1-P)$$을 갖는 베르누이 실험의 기대값으로 볼 수 있다. 하지만 베르누이 실험의 이벤트 횟수(성공 횟수)를 확률변수로 가지는 이항분포 $$X \sim B(n,p)$$에서 시행횟수가 무한히 커지는 경우 ($$n \to \infty$$), 연산량의 증가로 인하여 확률을 계산하는데 어려움이 있다. 이를 해결하기 위해 포아송 근사를 활용, 특정 사건의 발생 확률과 단위 시간당 이벤트 발생의 기대값을 계산하는데 포아송 분포를 많이 사용해왔다. 포아송 분포가 시행횟수가 큰 이항분포의 확률을 계산하는데 효율적으로 사용되고 단위시간당 발생하는 특정 사건의 횟수를 확률변수로 가지기 때문에 패킷 충돌 확률 분석[1], 의료 사망 데이터 분석[2]과 같은 많은 연구에서 활용되어왔다. 포아송 근사의 full proof 는 *proof* 에 기술한다.

 하지만 포아송 분포는 1)각각의 시행이 독립적이며(independent increment) 2)확률은 변하지 않는다(stationary increment)는 성질로 인해 과거 기록을 바탕으로하는 survival analysis에는 한계를 갖는다. 이를 Memoryless Property(무기억성)이라고 부르며 무기억성을 가지는 프로세스(혹은 분포)를 homogenous process라고 부른다. 따라서 단위시간에 따라 이벤트의 분포가 일정하지 않은 point process를 homogenous Poisson process로 모델링하는 것에는 한계가 있다. 본 연구에서는 무기억성을 갖지 않는 non-homogenous Poisson process 중 하나인 Hawkes Process를 활용하여 클릭 event의 point process와  사후 분포를 모델링한다.

IEEE-II(3)에서는 이항분포의 포아송로 근사(Poisson as Approximation to Binomial Distribution)와 homogeneous poisson process를 살펴보고 IEEE-II(3) 2.2에서는 non-homogeneous poisson process를 살펴본다. 최종적으로 2.3에서는 모바일 광고의 클릭 분포의 생성모델을 위한 Hawkes process의 memory kernel을 설계한다.

 *proof*



 이항분포의 확률변수 $$X$$는 성공할 확률이 인 $$P$$ 베르누이 실험을 $$n$$번 반복했을 때, 성공횟수를 의미하고 다음과 같은 indicator fuunction으로 나타낼 수 있다. 이때 이항분포의 확률질량함수는 (2)와 같다. 

$$\mathbf{1}_A(x)=\begin{Bmatrix}1,\  x\in A
\\ 1,\  x \notin A
\end{Bmatrix}$$	(1)

$$f(x)= {^n}C{_x}p^x(1-p)^{n-x},\ x=0,1, ..., n$$	(2)

 이항분포 $$X \sim B(n,p)$$, $$E(X)=\lambda=np$$ 라고 가정하면 $$p=\lambda/n$$ 이 성립한다. $$X$$가 이항분포를 따르기 때문에 $$X$$의 질량함수 (2)는 $$f(x)={n!}/{x!(n-x)!}\left ( \lambda/n \right )^x(1-\frac{\lambda}{n})^{n-x}$$가 성립하고 $$n$$이 양의 무한대로 가까워질 때 함수$$f(x)$$는 $$\lim\limits_{n \to \infty} f(x)=\lambda^x\cdot e^{-\lambda}/x!$$  (3)로 근사한다. 이를 포아송분포의 확률질량함수로 정의한다.



1) 수열 $x_n=( 1+1/n)^n$ 으로 정의할 때, 수열 $\{{x_n}\}$은 수렴한다. $x_n= ( 1+1/n)^n = {^n}C{_0} + {^n}C{_1}(1/n)+{^n}C{_2}(1/n)^2 + \cdots + {^n}C{_n}(1/n)^n$

$=1+1+\frac{n(n-1)}{2!}\frac{1}{n^2}+\frac{n(n-1)(n-2)}{3!}\frac{1}{n^3}+\cdots +\frac{n(n-1)(n-2)\cdots 2\cdot 1}{n!}\frac{1}{n^n}$

$=1+1+\frac{1}{2!}(1-\frac{1}{n})+\frac{1}{3!}(1-\frac{1}{n})(1-\frac{2}{n})+\cdots+\frac{1}{n!}(1-\frac{1}{n})(1-\frac{2}{n})\cdots(1-\frac{n-1}{n})$ (a)

$<1+1+\frac{1}{2!}+\frac{1}{3!}+\cdots +\frac{1}{n!} <1+1+\frac{1}{2}+\frac{1}{2^2}+\cdots+\frac{1}{2^{n-1}}=1+\frac{1-(\frac{1}{2})^n}{1-\frac{1}{2}}=1+2-(\frac{1}{2})^{n-1}$

따라서$\{x_n \}$은 위로유계(upper bounded)이다. 또한,

$x_{n+1}=1+1+\frac{1}{2!}( 1-\frac{1}{n+1})+\frac{1}{3!}(1-\frac{1}{n+1})(1-\frac{2}{n+1})+\cdots+\frac{1}{(n+1)!}(1-\frac{1}{n+1})(1-\frac{2}{n+1})\cdots(1-\frac{n}{n+1})%0$ (b)가 성립한다.

(a), (b)로부터 $\left ( 1-\frac{1}{n} \right )<(1-\frac{1}{n+1}), (1-\frac{1}{n})(1-\frac{2}{n})<(1-\frac{1}{n+1})(1-\frac{2}{n+1})\cdots$ 이므로 $x_n<x_{n+1}$ 가 성립하여 $\{x_n\}$은 단조증가수열이 된다. 따라서 $\{x_n\}$은 위로 유계이면서 단조증가수열이므로 단조수렴정리에 의하여 $\{x_n\}$ 수렴한다.



$\lim\limits_{n \to \infty} x_n=e$라 두자. $\lim\limits_{n \to \infty}\left ( 1+\frac{1}{n} \right )^n=e$ 이고 $\lim\limits_{n \to \infty}(1-\frac{\lambda}{n})^n = \lim\limits_{n \to \infty}( 1+( \frac{-\lambda}{n}))^n=e^{-\lambda}$가 성립하다.



2) $\lim\limits_{n \to \infty} (n! / (n-x)!n^x) = \lim\limits_{n \to \infty} {n(n-1)(n-2)\cdots (n-x+1)(n-x)!}/{(n-x)!n^x}$

$=\lim\limits_{n \to \infty}{n(n-1)\cdots(n-x+1)}/{n^x}= \lim\limits_{n \to \infty}{n}/{n}\cdot {(n-1)}/{n}\cdots {(n-x+1)}/{n}=1$



3) $\lim\limits_{n\rightarrow \infty  }\left ( \frac{\lambda^x}{x!}(1-\frac{\lambda}{n})^{n-x} \right )=\frac{\lambda^x}{x!}\lim\limits_{n\rightarrow\infty}\left \{ \left ( 1-\frac{\lambda}{n} \right )^n\times (1-\frac{\lambda}{n})^{-x} \right \}=\frac{\lambda^x}{x!}\cdot e^{-\lambda}\cdot\left ( 1-\frac{\lambda}{\infty } \right )^{-x}=\frac{\lambda^x\cdot e^{-\lambda}}{x!}$



1), 2), 3)에 의하여 $\lim\limits_{n \to \infty} f(x)=\lambda^x\cdot e^{-\lambda}/x!$ 가 성립한다. 따라서 시행 횟수가 커질 때 $$n \to \infty$$ $E(X)=\lambda=np$ 라고 가정하면 이항분포의 확률은 포아송분포의 확률질량함수로 근사한다.

 

