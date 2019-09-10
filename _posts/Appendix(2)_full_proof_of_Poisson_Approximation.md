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

 

