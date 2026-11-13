# 3 충분성 UMVUE Basu

> **범위:** 12강 (11/13 수정판 — 오타가 고쳐지고 Ex 12.9–12.11, 보조통계량·Basu 정리가 추가됨). 필기 ✍p14–21.
> **자세한 판:** [ch6.1–6.3 노트 C](../ch6.1-6.3%20최소제곱·베이즈·UMVUE.md#c-충분성-raoblackwell-umvue). 연습중간고사 2·3·5번 풀이는 [5](5%20연습중간고사와%20숙제%20풀이.md).
> **표시:** ✍ 수업 필기 · 📄 강의자료만 · 📝 연습시험·hw · (보충). 목차는 [README](README.md).

---

## 1. 충분성 복습

✍p14 (10/29)

- 수업의 지도: MoM, MLE, Bayes (← 지금까지), **UMVUE = 불편추정량 중 가장 좋은 것** (← 지금부터).
- **충분통계량:** $L(x\mid T=t)$가 $\theta$와 무관. 예: $\theta=(\mu,\sigma^2)$에서 $(\bar X,S^2)$ ⟺ $(\sum X_i,\sum X_i^2)$ (일대일).
- **인수분해:** $p_\theta(x)=g_\theta(T(x))h(x)$ ⟺ $T$ 충분. 그러면 $\ell(\theta)=\log g_\theta(T(x))+C$ → **MLE는 충분통계량의 함수** (유일할 때).
- **최소충분:** $\frac{p_\theta(y)}{p_\theta(x)}$가 $\theta$와 무관 ⟺ $T(x)=T(y)$이면 $T$ 최소충분.
- ✍ Ex 12.1: $U(\theta_1-\theta_2,\theta_1+\theta_2)$, $L=(2\theta_2)^{-n}I\lbrace\theta_1-\theta_2 < x_{(1)},\ x_{(n)} < \theta_1+\theta_2\rbrace$ → $(X_{(1)},X_{(n)})$ 최소충분.

## 2. Rao–Blackwell

✍p14–15

**정리** $T$ 충분, $\delta(X)$가 $g(\theta)$의 추정량 → $\delta^\ast(T)=\mathbb E[\delta(X)\mid T]$는 ( $T$가 충분이라 $\theta$에 의존하지 않으므로) **추정량**이고 $\mathbb E\delta^\ast=\mathbb E\delta$, $\mathrm{Var}\delta^\ast\le\mathrm{Var}\delta$, $\mathrm{MSE}\delta^\ast\le\mathrm{MSE}\delta$.

**✍ 증명** $\mathbb E\delta^\ast=\mathbb E[\mathbb E(\delta\mid T)]=\mathbb E\delta$. $\mathrm{Var}_ \theta(\delta)=\mathrm{Var}_ \theta\lbrace\mathbb E(\delta\mid T)\rbrace+\mathbb E\lbrace\mathrm{Var}_ \theta(\delta\mid T)\rbrace\ge\mathrm{Var}_ \theta(\delta^\ast)$. 평균이 같아 편향도 같으므로 MSE도 줄어든다. $\blacksquare$

**✍ 수업 예: $U(0,\theta)$에서 $2\bar X$ 개선**
- $\mathbb EX=\frac\theta2$ → $\hat\theta_{\mathrm{MoM}}=2\bar X$, $\mathrm{Var}=\frac4n\cdot\frac{\theta^2}{12}=\frac{\theta^2}{3n}$. $L(\theta)=\theta^{-n}I(X_{(n)} < \theta)$ → $X_{(n)}$ 충분.
- **$X_{(n)}$이 주어지면 나머지 $n-1$개는 $U(0,X_{(n)})$** (수업 판서 메모) → $\mathbb E[2\bar X\mid X_{(n)}]=\frac2n\big\lbrace\sum_{i=1}^{n-1}\frac{X_{(n)}}2+X_{(n)}\big\rbrace=\frac{n+1}nX_{(n)}$.
- $Y=X_{(n)}$: $\Pr(Y\le t)=(\frac t\theta)^n$, $f_Y=\frac{ny^{n-1}}{\theta^n}$, $\mathbb EY=\frac n{n+1}\theta$, $\mathbb EY^2=\frac n{n+2}\theta^2$, $\mathrm{Var}Y=\frac{n\theta^2}{(n+2)(n+1)^2}$ →

$$\mathrm{Var}(\hat\theta^{RB})=\frac{(n+1)^2}{n^2}\mathrm{Var}Y=\frac{\theta^2}{n(n+2)}\lt\frac{\theta^2}{3n}=\mathrm{Var}(2\bar X)\quad(n\gt 1).$$

  (수정판 강의노트는 우변을 $\mathrm{MSE}(X_{(n)})=\frac{2\theta^2}{(n+1)(n+2)}$와 비교한다 — 둘 다 성립)
- 📝 같은 기법: hw3 #9 = 연습 2번 ( $U(\theta,2\theta)$: 최솟값·최댓값이 주어지면 가운데 값은 그 사이의 균등).

## 3. UMVUE와 유일성

✍p16 (11/3)

**정의** $\hat g$가 $g(\theta)$의 불편추정량이고 **모든** 불편추정량 $\tilde g$, **모든** $\theta$에서 $\mathrm{Var}_ \theta(\hat g)\le\mathrm{Var}_ \theta(\tilde g)$.

**Thm 12.4 (유일성)** 분산이 유한한 UMVUE는 유일하다.
**✍ 증명 (판서)** $\hat g_1,\hat g_2$ UMVUE → $\mathbb E\hat g_1=\mathbb E\hat g_2=g$, $\mathrm{Var}\hat g_1=\mathrm{Var}\hat g_2=:V$, $C=\mathrm{Cov}(\hat g_1,\hat g_2)$.
- (a) $\hat g_3=\frac{\hat g_1+\hat g_2}2$도 불편 → $\frac14\mathrm{Var}(\hat g_1+\hat g_2)=\frac12(V+C)\ge V$ → $C\ge V$.
- (b) $\mathrm{Var}\big(\frac{\hat g_1-\hat g_2}2\big)=\frac12(V-C)\le0$ → $=0$.
- (c) Chebyshev: $\forall\epsilon > 0$, $\Pr(\lvert\hat g_1-\hat g_2\rvert\gt\epsilon)\le\frac{\mathrm{Var}(\hat g_1-\hat g_2)}{\epsilon^2}=0$ ( $\mathbb E(\hat g_1-\hat g_2)=0$) → $\Pr(\hat g_1=\hat g_2)=1$. $\blacksquare$

## 4. 완비성과 Lehmann–Scheffé

✍p16–17

**완비** 모든 $a$에 대해 $\mathbb E_\theta a(T)=0\ \forall\theta\Rightarrow\Pr_\theta(a(T)=0)=1\ \forall\theta$. 충분성은 정보 보존, **완비성은 $T$의 함수인 불편추정량을 하나로** 만든다.

**Thm 12.5 (Lehmann–Scheffé)** $T$ 완비충분, $\mathbb E\delta(X)=g(\theta)$ → $\hat g^{\mathrm{UMVUE}}=\mathbb E[\delta(X)\mid T]$.
**✍ 증명** 임의의 불편 $\delta_0$에 대해 $\delta_0^\ast=\mathbb E[\delta_0\mid T]$는 $T$의 함수이고 불편 ( $\mathbb E[\mathbb E(\delta_0\mid T)]=g$). $T$의 함수인 불편추정량 $f(T)$가 있으면 $\mathbb E(\delta_0^\ast-f(T))=0$ → **완비성으로 $\delta_0^\ast=f(T)$** (a.s.). RB로 $\mathrm{Var}f(T)=\mathrm{Var}\delta_0^\ast\le\mathrm{Var}\delta_0$ — 모든 불편 $\delta_0$보다 작거나 같으니 $f(T)$가 UMVUE. $\blacksquare$

**Thm 12.6 (지수족)** $f_\alpha(x)=\exp\lbrace\alpha^\top T(x)-\psi(\alpha)\rbrace f_0(x)$에서 ① 지지집합이 $\alpha$와 무관 ② 자연모수공간 $A$가 **열린 직육면체를 포함** ③ $c\ne0$이면 $c^\top T(X)$가 상수가 아님 → $\sum_iT(X_i)$는 완비충분. (✏️ 필기에는 ②가 "A가 convex set"으로 적혀 있다 — 강의자료대로 **열린 직육면체 포함**이 정확한 조건)

**✍ 반례 $N(\mu,\mu^2)$** $\alpha_1=\frac1\mu$, $\alpha_2=-\frac1{2\mu^2}=-\frac{\alpha_1^2}2$ — 곡선이라 ②가 깨진다. $Y=(\sum X_i,\sum X_i^2)$는 충분하지만 $g(Y)=\frac{Y_2}{2n}-\frac{Y_1^2}{n(n+1)}$의 기댓값이 $\mu^2-\mu^2=0$ → **완비 아님.** (수정판: $x_1=\dots=x_n=c\ne0$에서 $g=\frac{n(n-1)c^2}{2(n+1)} > 0$이고 연속이라 양의 확률로 $g\ne0$)

## 5. 완비성을 보이는 네 가지 기법

연습중간고사 3·5번처럼 **"지수족 성질을 쓰지 말고"** 보이라고 하면 이 표에서 고른다.

| 기법 | 분포 | 핵심 한 줄 | 필기 |
| --- | --- | --- | --- |
| 다항식 | 베르누이·이항 $T\sim\mathrm{Bin}(n,\theta)$ | $\sum_ta(t)\binom nt\theta^t(1-\theta)^{n-t}=0$을 $(1-\theta)^n$으로 나누고 $z=\frac\theta{1-\theta}\in(0,\infty)$: $\sum a(t)\binom ntz^t=0\ \forall z > 0$ → 계수 0 | ✍p17 |
| 멱급수 | 포아송 $T\sim\mathrm{Poi}(n\theta)$ | $e^{-n\theta}\sum a(t)\frac{(n\theta)^t}{t!}=0$ → 멱급수가 0이면 계수 0 | ✍p19 |
| 멱급수 | 기하·음이항 $\Pr(T=t)=\binom{t-1}{n-1}p^nq^{t-n}$ | $\sum_{t\ge n}a(t)\binom{t-1}{n-1}q^{t-n}=0\ \forall q\in(0,1)$ → 계수 0 | 📝 연습 5 |
| 라플라스 | 지수·감마 $T\sim\mathrm{Gamma}(n,\theta)$ | $\int_0^\infty a(t)t^{n-1}e^{-t/\theta}dt=0\ \forall\theta$ → 라플라스 변환 유일성 → $a(t)t^{n-1}=0$ a.e. | ✍p19–20 |
| 미분 | 균등 $T=X_{(n)}$ | $\int_0^\theta a(t)t^{n-1}dt=0\ \forall\theta$ → $\theta$로 미분 → $a(\theta)\theta^{n-1}=0$ | ✍p20 |
| (비완비) | $U(-\theta,\theta)$의 $(X_{(1)},X_{(n)})$ | $a=X_{(1)}+X_{(n)}$, $\mathbb E=-\frac{n-1}{n+1}\theta+\frac{n-1}{n+1}\theta=0$ | ✍p20 |

**쓸 때 빠뜨리지 말 것:** (1) $T$의 분포를 정확히 (2) "모든 $\theta$"에서 0 → 변수 치환의 범위가 열린 구간 전체임 (3) 계수가 0 → $a(t)=0$ (지지점 전체에서) → $\Pr(a(T)=0)=1$.

## 6. UMVUE 구하기

✍p17–21 · **두 경로:** ① RB: 쉬운 불편 $\delta_0$ → $\mathbb E[\delta_0\mid T]$ ② 직접: $\mathbb E\delta(T)=g(\theta)$를 풀기 (멱급수, PGF, 미분).

### 6.1 베르누이

✍p17–18

- $T=\sum X_i$ 완비충분. $\mathbb E(T/n)=\theta$ → $\bar X$가 UMVUE.
- $g=\theta(1-\theta)$: MLE는 $\bar X(1-\bar X)$ (불변성). $\delta_0=I(X_1=1,X_2=0)$, $\mathbb E\delta_0=\theta(1-\theta)$.

$$\mathbb E[\delta_0\mid T=y]=\frac{\theta(1-\theta)\binom{n-2}{y-1}\theta^{y-1}(1-\theta)^{n-y-1}}{\binom ny\theta^y(1-\theta)^{n-y}}=\frac{y(n-y)}{n(n-1)}\ \Rightarrow\ \hat g^{\mathrm{UMVUE}}=\frac{T(n-T)}{n(n-1)} .$$

### 6.2 포아송과 PGF

✍p18–19

**PGF** $G(s)=\mathbb E(s^X)=\sum_k\Pr(X=k)s^k$, $G^{(t)}(0)=t!\Pr(X=t)$. 포아송 $(\lambda)$: $\mathbb Es^X=e^{\lambda(s-1)}$, $T=\sum X_i\sim\mathrm{Poi}(n\theta)$.

**$g=e^{-2\theta}$** (MLE는 $e^{-2\bar X}$)
- **방법 1 (RB, ✍):** $\delta_0=I(X_1=0,X_2=0)$, $\mathbb E\delta_0=e^{-2\theta}$. $X_1,X_2$와 $\sum_{i\ge3}X_i\sim\mathrm{Poi}((n-2)\theta)$가 독립이라

$$\Pr(X_1=X_2=0\mid T=t)=\frac{e^{-2\theta}\cdot e^{-(n-2)\theta}\frac{\lbrace(n-2)\theta\rbrace^t}{t!}}{e^{-n\theta}\frac{(n\theta)^t}{t!}}=\Big(1-\frac2n\Big)^t .$$

- **방법 2 (PGF):** $\mathbb E(s^T)=e^{n\theta(s-1)}=e^{-2\theta}$ ⟺ $s=1-\frac2n$ → $\mathbb E(1-\frac2n)^T=e^{-2\theta}$.
- $\hat g^{\mathrm{UMVUE}}=(1-\frac2n)^T$ ( $n\ge2$). 큰 $n$에서 $\approx e^{-2\bar X}$. $n=1$이면 $(-1)^T$ — 음수가 될 수 있어 해석에 주의.
- 일반화: $\Pr(X_1=k)$의 UMVUE는 $\binom Tk(\frac1n)^k(1-\frac1n)^{T-k}$ ( $X_1\mid T\sim\mathrm{Bin}(T,\frac1n){}$).

### 6.3 지수와 균등

✍p19–20

- $\mathrm{Exp}(\theta)$ (평균 $\theta$), $T=\sum X_i\sim\mathrm{Gamma}(n,\theta)$, 라플라스로 완비 → $\bar X$가 UMVUE.
- $U(0,\theta)$: $f_T(t)=\frac n\theta(\frac t\theta)^{n-1}$, 미분으로 완비, $\mathbb ET=\frac n{n+1}\theta$ → **$\hat\theta^{\mathrm{UMVUE}}=\frac{n+1}nX_{(n)}$** (2절의 RB 결과와 같다 — Lehmann–Scheffé대로).
- $U(-\theta,\theta)$: $(X_{(1)},X_{(n)})$은 완비 아님. $f=(2\theta)^{-n}I(\max\lvert x_i\rvert < \theta)$ → $T=\max\lvert X_i\rvert$ 충분, $\lvert X_i\rvert\sim U(0,\theta)$라 완비 → $\frac{n+1}n\max\lvert X_i\rvert$.

### 6.4 정규 신뢰도

✍p20 · 📄 Ex 12.9

$X_i\overset{\text{iid}}{\sim}N(\theta,1)$, $\eta=\Pr_\theta(X_1 > a)=1-\Phi(a-\theta)$. $\bar X$ 완비충분, $\delta_0=I(X_1 > a)$.
- $(X_1-\bar X,\bar X)$는 결합정규, $\mathrm{Cov}(X_1-\bar X,\bar X)=\frac1n-\frac1n=0$ → **독립**. $\mathrm{Var}(X_1-\bar X)=(1-\frac1n)^2+\frac{n-1}{n^2}=\frac{n-1}n$.

$$\hat\eta^{\mathrm{UMVUE}}=\Pr(X_1-\bar X\gt a-y\mid\bar X=y)\big\vert_ {y=\bar X}=1-\Phi\Big(\frac{a-\bar X}{\sqrt{(n-1)/n}}\Big).$$

  (MLE $1-\Phi(a-\bar X)$와 분모 $\sqrt{(n-1)/n}$만 다르다)

### 6.5 지수 신뢰도

✍p21 · 📄 Ex 12.11

$X_i\sim\mathrm{Exp}(\theta)=\mathrm{Gamma}(1,\theta)$, $g=\Pr(X_1 > a)=e^{-a/\theta}$ (MLE $e^{-a/\bar X}$). $T=\sum X_i$ 완비충분, $\delta_0=I(X_1 > a)$.
- $\frac{X_1}{X_1+\sum_{i\ge2}X_i}=\frac{X_1}T\sim\mathrm{Beta}(1,n-1)$ (밀도 $(n-1)(1-x)^{n-2}$), $T$와 독립 (보조통계량 + Basu).

$$\mathbb E[\delta_0\mid T=t]=\Pr\Big(\frac{X_1}T\gt \frac at\Big)=\int_{a/t}^1(n-1)(1-x)^{n-2}dx=\Big(1-\frac at\Big)^{n-1}\quad(t\gt a)$$

  → $\hat g^{\mathrm{UMVUE}}=(1-\frac aT)^{n-1}I(T > a)$.

## 7. 보조통계량과 Basu

✍p21 · 📄 12.5절

- **보조통계량:** 분포가 $\theta$와 무관한 통계량 $A=v(X)$. ✍ 예: $N(\theta,1)$의 잔차 $(X_1-\bar X,\dots,X_n-\bar X)$, $U(0,\theta)$의 $X_{(n)}/X_{(1)}$ ( $Y_i=X_i/\theta\sim U(0,1)$이라 비율의 분포가 $\theta$와 무관).
- **Basu 정리:** $T$ 완비충분, $A$ 보조 → 모든 $\theta$에서 $T\perp A$.
- **증명:** 사건 $B$에 대해 $q(t)=\Pr(A\in B\mid T=t)$ — 충분성으로 $\theta$와 무관. $\mathbb E_\theta q(T)=\Pr(A\in B)$ — 보조성으로 상수. 그러므로 $\mathbb E_\theta\lbrace q(T)-\Pr(A\in B)\rbrace=0\ \forall\theta$ → 완비성으로 $q(T)=\Pr(A\in B)$ a.s. → 독립. $\blacksquare$
- **Ex 12.10** 이동 지수 $\frac1\beta e^{-(x-\mu)/\beta}I(x > \mu)$, $\beta$ 고정: $X_{(1)}$은 $\mu$의 완비충분, $S=\sum(X_i-X_{(1)})\sim\mathrm{Gamma}(n-1,\beta)$는 $\mu$와 무관 → $X_{(1)}\perp S$.
- **쓰는 곳:** 조건부 기댓값 계산에서 "주어진 $T$와 독립"을 정당화 (6.4의 $X_1-\bar X$, 6.5의 $X_1/T$).

## 8. 자주 하는 실수

- RB에서 충분하지 않은 통계량으로 조건부 기댓값을 잡기 → 결과에 $\theta$가 남아 추정량이 아님.
- 완비성 논증에서 "모든 $\theta$"가 열린 구간 전체여야 함을 말하지 않기.
- $T$의 함수인 불편추정량을 찾고도 "완비충분"을 확인하지 않고 UMVUE라고 결론.
- 비완비 예 ( $U(\theta,2\theta)$, $N(\mu,\mu^2){}$)에서 Lehmann–Scheffé를 쓰기.
- MLE를 그대로 UMVUE라 하기 ( $\bar X(1-\bar X)$, $e^{-2\bar X}$는 편향).

**셀프 체크**
<details><summary>Q1. Lehmann–Scheffé 증명에서 완비성이 쓰이는 정확한 줄은?</summary>

RB한 $\delta_0^\ast$와 $T$의 다른 불편 함수 $f(T)$의 차이의 기댓값이 모든 θ에서 0 → 완비성으로 $\delta_0^\ast=f(T)$ a.s. (불편추정량 출발점과 무관하게 같은 함수가 나온다)

</details>
<details><summary>Q2. e^{−2θ}의 UMVUE를 RB로 구할 때 왜 δ₀ = I(X₁=0, X₂=0)인가?</summary>

$\Pr(X_1=0)\Pr(X_2=0)=e^{-\theta}e^{-\theta}=e^{-2\theta}$라 불편이고, 조건부확률 계산이 독립 포아송 합의 성질로 깔끔해진다.

</details>
<details><summary>Q3. 정규 신뢰도의 UMVUE에서 X₁ − X̄와 X̄가 독립인 근거 두 가지는?</summary>

(1) 결합정규이고 공분산이 0 (2) $X_1-\bar X$는 위치모수에 대한 보조통계량, $\bar X$는 완비충분 → Basu.

</details>
