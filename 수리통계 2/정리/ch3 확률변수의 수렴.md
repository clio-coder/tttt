# ch3 확률변수의 수렴 (보충: almost sure 수렴과 Borel–Cantelli)

> **원본:** `lecture_ch3_convergence_of_random_variables.pdf` (6쪽, 9/10 수업) · `lecture_ch3_supplementary_almost_sure_convergence.pdf` (2쪽)
> **관련 숙제:** hw1 #6 (AR(1) 표본평균의 qm 수렴) · #7 ($\log n/M_n$의 일치성) · 연결: ch1 Chebyshev·Hoeffding, ch5 일치성·점근분포
> **범례:** 📝 hw1에 나옴 · 🧩 강의노트에 증명 없음 → 보충 증명 · 📐 강의노트에 증명 있음 · 🔢 계산 · 💡 개념·해석 · (보충) 강의노트에 없는 내용 · `[T…]` = [도구상자](00%20문제풀이%20도구상자.md) 카드 번호
> **기호:** $X_n\xrightarrow{\text{a.s.}}X$, $X_n\xrightarrow{p}X$ (강의노트는 $\xrightarrow{P}$), $X_n\xrightarrow{qm}X$, $X_n\xrightarrow{d}X$. $F_n,F$ = $X_n,X$의 CDF. $X_{(n)}=\max_iX_i$, $X_{(1)}=\min_iX_i$.

---

## 0. 한눈에

### 0.1 범위

| 쪽 | 절 | 내용 | 증명 | hw1 |
| --- | --- | --- | --- | --- |
| p1 | 도입, 3.1 | a.s. 수렴 정의 (3.1), 동치 표현, 강한 대수의 법칙 (Ex 3.1) | – | – |
| p1–2 | 3.2 | 확률수렴 (3.2), 약한 대수의 법칙 (Thm 3.1), 일치추정량 | 📐 | #7 |
| p2–3 | 3.3 | qm 수렴 (3.3), qm ⟹ p (Thm 3.2), 반례 (Ex 3.2) | 📐 | #6 |
| p3–4 | 3.4 | 분포수렴 (3.4), 연속점 (Ex 3.3), p ⟹ d (Thm 3.3), 반례 (Ex 3.4), 표본최댓값 (Ex 3.5), 상수로의 d ⟹ p (Thm 3.4) | 📐 | – |
| p5–6 | 3.5 | 관계도, 모멘트 주의 (Remark), CMT (Thm 3.5), Slutsky (Thm 3.6), CLT (Thm 3.7) | 🧩 | #7 |
| 보충 p1 | 1 | $A_n$ i.o., First/Second Borel–Cantelli | 🧩 | – |
| 보충 p1–2 | 2 | a.s. 수렴의 충분조건 (Thm 2.1), 지수형 꼬리 (Ex 2.1) | 📐 | – |

### 0.2 시험 포인트 (출제 가능성 순)

**★★★ 매우 유력**

1. 📝 **"$X_n\to$ 무엇"을 직접 보이기** — qm은 편향–분산 분해, p는 Chebyshev 또는 꼬리확률 직접 계산 (최댓값·최솟값은 CDF로, **양쪽 꼬리** 모두), 극한값의 함수는 CMT로 옮기기. hw1 #6·#7이 이 유형이다. → [3.2](#32-보충-편향분산-분해), [7](#7-보충-최댓값과-최솟값-다루기), [hw1 #6](#hw1-6-ar1-표본평균의-qm-수렴), [hw1 #7](#hw1-7-log-n을-최댓값으로-나눈-것의-일치성)
2. 📐 **포함관계 증명 세 개** — qm ⟹ p (Chebyshev), p ⟹ d ($\epsilon$ 샌드위치 + lim inf/lim sup), 상수로의 d ⟹ p. → [3.1](#31-정의와-thm-32), [4.2](#42-thm-33-확률수렴이면-분포수렴), [4.5](#45-thm-34-상수로의-분포수렴)
3. 📐 **반례 세 개** — Ex 3.2 (p지만 qm 아님), Ex 3.4 (d지만 p 아님), Remark (p지만 기댓값이 발산). → [5](#5-수렴의-관계)
4. 🔢 **극한분포 유도** — $n(1-X_{(n)})\xrightarrow{d}\text{Exp}(1)$ (Ex 3.5), 원판 위 균등분포의 MLE (2021 기출), 지수분포 최댓값 → Gumbel (보충). → [4.4](#44-ex-35-표본최댓값), [V2](#v2-원판-위-균등분포의-극한분포), [V3](#v3-지수분포-최댓값과-gumbel)

**★★ 유력**

5. 📐 약한 대수의 법칙 (Chebyshev 증명), 일치추정량의 정의 → [2](#2-확률수렴과-약한-대수의-법칙)
6. 🧩📐 Borel–Cantelli와 a.s. 충분조건 (Thm 2.1 증명 있음, First BC는 증명 없음 → 보충 증명이 두 줄), 지수 꼬리 ⟹ a.s. → [8](#8-보충-almost-sure-수렴과-borelcantelli)
7. 🧩 CMT·Slutsky의 사용 (t 통계량 등)과 짧은 증명 (상수 극한의 CMT, Slutsky의 합) → [6](#6-cmt-slutsky-clt)
8. 💡 a.s.와 qm 사이에는 포함관계가 없다 (예: Ex 3.2는 a.s.이지만 qm 아님, 독립 Bernoulli$(1/n)$은 qm이지만 a.s. 아님) → [5](#5-수렴의-관계), [V6](#v6-독립-bernoulli로-본-수렴의-차이)

**★ 가능성 있음**

9. 💡 CLT의 내용과 해석 (WLLN = 일치성, CLT = 오차를 $\sqrt n$배 확대한 분포)
10. 💡 정의들 — a.s.의 $\epsilon$–$N(\omega,\epsilon)$ 표현, 분포수렴은 **연속점에서만** (Ex 3.3)

### 0.3 이 장의 도구

| 도구 | 쓰는 곳 |
| --- | --- |
| [T14] Chebyshev·Markov로 확률수렴 | WLLN, qm ⟹ p, 비동일분포 WLLN |
| [T15] 편향–분산 분해 $\mathbb E(X_n-c)^2=\operatorname{Var}X_n+(\mathbb EX_n-c)^2$ | hw1 #6 |
| [T16] 최댓값·최솟값의 CDF, 양쪽 꼬리, 합집합 | Ex 3.5, hw1 #7 |
| [T17] $(1-x/n)^n\to e^{-x}$, $1-x\le e^{-x}$ | Ex 3.5, hw1 #7 |
| [T18] CMT·Slutsky로 옮기기 | hw1 #7 마지막 단계, t 통계량 |
| [T19] Borel–Cantelli: 꼬리확률의 합이 유한 ⟹ a.s. | 보충 Thm 2.1 |
| [T20] $\epsilon$ 샌드위치 + lim inf/lim sup | Thm 3.3, Slutsky 증명 |

---

## 1. almost sure 수렴

💡 [ch3 p1]

고정된 표본 크기의 정확한 계산 대신, $n$이 커질 때 통계량의 거동(대표본 이론, 극한 이론, 점근 이론)을 보면 복잡한 문제를 단순한 근사로 바꿀 수 있다. 확률변수는 표본점 $\omega$에 따라 값이 달라지므로 "가까움"을 해석하는 방법이 여러 가지다.

**정의 (3.1)**

$$\Pr\Big(\big\{\omega:\lim_{n\to\infty}X_n(\omega)=X(\omega)\big\}\Big)=1 .$$

**동치** 확률 1인 집합 $\Omega_0$가 있어서, 모든 $\omega\in\Omega_0$와 $\epsilon > 0$에 대해 어떤 $N=N(\omega,\epsilon)$이 있고 $n\ge N$이면 $\lvert X_n(\omega)-X(\omega)\rvert < \epsilon$. 즉 확률 0인 예외를 빼면 **표본 경로마다 보통의 실수열처럼 수렴**한다.

**Example 3.1 (강한 대수의 법칙, SLLN)** $Y_1,Y_2,\dots$ iid, $\mathbb E\lvert Y_1\rvert < \infty$, $\mu=\mathbb EY_1$이면 $\bar Y_n\xrightarrow{\text{a.s.}}\mu$. (증명은 강의노트에 없음. 유계일 때는 Hoeffding + Borel–Cantelli로 증명된다 → [8.3](#83-thm-21-almost-sure-수렴의-충분조건), ch1 V9)

---

## 2. 확률수렴과 약한 대수의 법칙

📐 [ch3 p1–2]

**정의 (3.2)** 모든 $\epsilon > 0$에 대해 $\Pr(\lvert X_n-X\rvert > \epsilon)\to0$.

- $X=c$ (상수)이면 $X_n$의 분포가 $c$ 주위에 집중된다는 뜻이다.
- a.s. 수렴은 "거의 모든 **표본 경로의 값**"에 대한 말, 확률수렴은 "오차가 큰 **사건의 확률**"에 대한 말이다.

**Theorem 3.1 (약한 대수의 법칙, WLLN)** $Y_i$ iid, $\mathbb EY_1=\mu$, $\operatorname{Var}Y_1=\sigma^2 < \infty$이면 $\bar Y_n\xrightarrow{p}\mu$.

**증명** $\mathbb E\bar Y_n=\mu$, $\operatorname{Var}\bar Y_n=\sigma^2/n$ (∵ 독립 — 사실 비상관이면 충분). Chebyshev로

$$\Pr(\lvert\bar Y_n-\mu\rvert > \epsilon)\le\frac{\operatorname{Var}\bar Y_n}{\epsilon^2}=\frac{\sigma^2}{n\epsilon^2}\to0 .\qquad\blacksquare$$

**정의 (일치성)** 추정량 $\hat\theta_n$이 $\hat\theta_n\xrightarrow{p}\theta$를 만족하면 $\theta$의 **일치추정량(consistent estimator)**이다. → hw1 #7 "일치추정량인가?" = "확률수렴하는가?"

(보충) **같은 증명으로 얻는 일반화:** $X_i$가 독립(또는 비상관)이고 평균이 모두 $\mu$, 분산이 $\sigma_i^2$일 때 $\frac1{n^2}\sum_{i=1}^n\sigma_i^2\to0$이면 $\bar X_n\xrightarrow{p}\mu$. → [V5](#v5-분산이-다른-독립-변수의-대수의-법칙)

---

## 3. qm 수렴

### 3.1 정의와 Thm 3.2

📐 [ch3 p2 (3.3), Thm 3.2]

**정의 (3.3)** $\mathbb E[(X_n-X)^2]\to0$이면 $X_n\xrightarrow{qm}X$ ($L^2$ 수렴). 좌변은 평균제곱오차라서, 오차가 나는 **빈도뿐 아니라 크기**까지 통제한다.

**Theorem 3.2** $X_n\xrightarrow{qm}X\Rightarrow X_n\xrightarrow{p}X$.

**증명** 임의의 $\epsilon > 0$에 대해 Chebyshev (정확히는 $(X_n-X)^2$에 Markov)로

$$\Pr(\lvert X_n-X\rvert > \epsilon)\le\frac{\mathbb E[(X_n-X)^2]}{\epsilon^2}\to0 .\qquad\blacksquare$$

### 3.2 보충 편향–분산 분해

(보충) hw1 #6의 핵심 도구. [T15]

$$\mathbb E(X_n-c)^2=\operatorname{Var}X_n+(\mathbb EX_n-c)^2 .$$

(증명: $X_n-c=(X_n-\mathbb EX_n)+(\mathbb EX_n-c)$를 제곱해 기댓값, 교차항은 $\mathbb E(X_n-\mathbb EX_n)=0$이라 사라진다.)

따라서 **상수 $c$로의 qm 수렴** ⟺ $\mathbb EX_n\to c$ 그리고 $\operatorname{Var}X_n\to0$. qm을 보이라는 문제는 "평균과 분산을 계산하라"는 문제다.

### 3.3 Ex 3.2 역은 성립하지 않는다

📐 [ch3 p3]

$U\sim\text{Unif}(0,1)$, $X_n=\sqrt n\,I_{[0,1/n]}(U)$.
- 확률수렴: $\sqrt n > \epsilon$인 $n$에서 $\Pr(\lvert X_n\rvert > \epsilon)=\Pr(U\le1/n)=1/n\to0$ → $X_n\xrightarrow{p}0$.
- qm 아님: $\mathbb E(X_n^2)=n\Pr(U\le1/n)=1$ → $X_n\not\xrightarrow{qm}0$.
- 요점: $X_n$은 **대부분 0이지만 아주 드물게 큰 값**을 갖는다. 확률수렴은 드문 사건을 무시하지만 제곱 기댓값은 무시하지 못한다.
- (보충) 사실 $X_n\xrightarrow{\text{a.s.}}0$이기도 하다: $U(\omega) > 0$인 $\omega$ (확률 1)에서 $n > 1/U(\omega)$이면 $X_n(\omega)=0$. → "a.s.이지만 qm 아님"의 예.

---

## 4. 분포수렴

### 4.1 정의와 연속점

📐 [ch3 p3 (3.4), Ex 3.3]

**정의 (3.4)** $F$의 **모든 연속점** $x$에서 $F_n(x)\to F(x)$이면 $X_n\xrightarrow{d}X$.

- 값이 가까워질 필요 없이 **분포의 모양**만 가까워지면 된다 → 네 가지 중 가장 약한 수렴.
- 연속점만 보는 조건은 필수다. **Ex 3.3:** $X_n\sim N(0,1/n)$, $X=0$ a.s. ($F(x)=I(x\ge0)$). $x < 0$이면 $F_n(x)=\Pr(Z\le\sqrt nx)\to0$, $x > 0$이면 $\to1$. 그런데 $F_n(0)=1/2\not\to F(0)=1$. $x=0$은 $F$의 불연속점이라 상관없고, $X_n\xrightarrow{d}0$이다.

### 4.2 Thm 3.3 확률수렴이면 분포수렴

📐 [ch3 p3–4]

**결론** $X_n\xrightarrow{p}X\Rightarrow X_n\xrightarrow{d}X$.

**핵심 한 수** 사건을 $\lvert X_n-X\rvert\le\epsilon$ / $> \epsilon$으로 나눠 $F_n(x)$를 $F(x\pm\epsilon)$로 끼우고, 극한의 존재를 모르니 **lim inf / lim sup**를 따로 취한다. [T20]

**증명** $F$의 연속점 $x$와 $\epsilon > 0$을 고정한다.

$$\begin{aligned}
F_n(x)&=\Pr(X_n\le x,\ X\le x+\epsilon)+\Pr(X_n\le x,\ X > x+\epsilon)\le F(x+\epsilon)+\Pr(\lvert X_n-X\rvert > \epsilon),\\
F(x-\epsilon)&=\Pr(X\le x-\epsilon,\ X_n\le x)+\Pr(X\le x-\epsilon,\ X_n > x)\le F_n(x)+\Pr(\lvert X_n-X\rvert > \epsilon).
\end{aligned}$$

(둘째 항들: $X_n\le x < x+\epsilon < X$이면 $X-X_n > \epsilon$, $X\le x-\epsilon < x < X_n$이면 $X_n-X > \epsilon$.) 합치면

$$F(x-\epsilon)-\Pr(\lvert X_n-X\rvert > \epsilon)\le F_n(x)\le F(x+\epsilon)+\Pr(\lvert X_n-X\rvert > \epsilon).$$

$F_n(x)$의 극한이 있는지 모르므로 lim inf와 lim sup를 취한다. $X_n\xrightarrow{p}X$라 $\Pr(\lvert X_n-X\rvert > \epsilon)\to0$:

$$F(x-\epsilon)\le\liminf_{n\to\infty}F_n(x)\le\limsup_{n\to\infty}F_n(x)\le F(x+\epsilon).$$

$\epsilon$은 임의이고 $F$가 $x$에서 연속이므로 $\epsilon\downarrow0$이면 양 끝이 $F(x)$로 간다. 따라서 $\liminf=\limsup=F(x)$, 즉 $F_n(x)\to F(x)$. ∎

**셀프 체크**
<details><summary>Q1. 연속점 조건은 증명의 어디에서 쓰이나?</summary>

마지막 단계: $F(x-\epsilon)\to F(x)$와 $F(x+\epsilon)\to F(x)$ ($\epsilon\downarrow0$). 오른쪽은 CDF의 우연속성으로 항상 되지만, 왼쪽은 연속점이어야 된다.

</details>
<details><summary>Q2. 왜 바로 lim을 취하지 않나?</summary>

$\lim F_n(x)$가 존재하는지 모르기 때문에. lim inf·lim sup는 항상 존재한다.

</details>

### 4.3 Ex 3.4 역은 성립하지 않는다

📐 [ch3 p4]

$X\sim N(0,1)$, 모든 $n$에서 $X_n=-X$. 정규분포의 대칭성으로 $X_n\overset{d}{=}X$ → $X_n\xrightarrow{d}X$. 그런데 $\lvert X_n-X\rvert=2\lvert X\rvert$라 $\Pr(\lvert X_n-X\rvert > \epsilon)=\Pr(\lvert X\rvert > \epsilon/2) > 0$은 0으로 가지 않는다 → $X_n\not\xrightarrow{p}X$.

### 4.4 Ex 3.5 표본최댓값

📐🔢 [ch3 p4] — 극한분포 유도의 표준 예

$X_1,\dots,X_n\overset{\text{iid}}{\sim}\text{Unif}(0,1)$, $X_{(n)}=\max_iX_i$.
- **확률수렴:** $0 < \epsilon < 1$이면 $\Pr(\lvert X_{(n)}-1\rvert > \epsilon)=\Pr(X_{(n)} < 1-\epsilon)=\prod_i\Pr(X_i < 1-\epsilon)=(1-\epsilon)^n\to0$ (∵ 독립, max ≤ a ⟺ 모두 ≤ a). → $X_{(n)}\xrightarrow{p}1$.
- **확대한 극한:** $t\ge0$이면

$$\Pr\big(n(1-X_{(n)})\le t\big)=1-\Pr\Big(X_{(n)} < 1-\frac tn\Big)=1-\Big(1-\frac tn\Big)^n\to1-e^{-t}$$

  ($n\ge t$일 때). 따라서 $n(1-X_{(n)})\xrightarrow{d}\text{Exp}(1)$.
- **요점:** 최댓값 자체는 1로 확률수렴하지만, **적절한 속도 $n$으로 확대**하면 지수분포라는 극한분포가 나온다. (표본평균은 $\sqrt n$, 최댓값은 $n$ — 속도가 다르다)

### 4.5 Thm 3.4 상수로의 분포수렴

📐 [ch3 p4–5]

**결론** $c$가 상수이면 $X_n\xrightarrow{d}c\Rightarrow X_n\xrightarrow{p}c$. (Thm 3.3과 합치면 상수 극한에서는 d와 p가 동치)

**증명** $F(x)=I(x\ge c)$의 불연속점은 $c$뿐이라 $c\pm\epsilon$은 연속점이다.

$$\Pr(\lvert X_n-c\rvert > \epsilon)=\Pr(X_n > c+\epsilon)+\Pr(X_n < c-\epsilon)\le1-F_n(c+\epsilon)+F_n(c-\epsilon)\to1-1+0=0 .\qquad\blacksquare$$

---

## 5. 수렴의 관계

📐💡 [ch3 p5]

```
   a.s.  ==>  p  ==>  d
   qm    ==>  p  ==>  d
   (a.s. 와 qm 사이에는 일반적인 포함관계가 없다)
   d ==> p  : 극한이 상수일 때만 (Thm 3.4)
```

| 주장 | 성립? | 증명 / 반례 |
| --- | --- | --- |
| qm ⟹ p | ○ | Thm 3.2 (Chebyshev) |
| p ⟹ d | ○ | Thm 3.3 |
| a.s. ⟹ p | ○ | (강의노트는 증명 없음) |
| p ⟹ qm | × | Ex 3.2 |
| d ⟹ p | × | Ex 3.4 (단 상수 극한이면 ○, Thm 3.4) |
| a.s. ⟹ qm | × | Ex 3.2 (a.s. 수렴이기도 함, 보충) |
| qm ⟹ a.s. | × | (보충) 독립 $X_n\sim\text{Bernoulli}(1/n)$ → [V6](#v6-독립-bernoulli로-본-수렴의-차이) |
| p ⟹ 기댓값 수렴 | × | Remark 아래 |

**Remark (모멘트의 수렴)** $X_n\xrightarrow{p}0$이라도 $\mathbb EX_n\to0$일 필요는 없다. $\Pr(X_n=n^2)=\frac1n$, $\Pr(X_n=0)=1-\frac1n$이면 $\Pr(\lvert X_n\rvert > \epsilon)=\frac1n\to0$ (→ $X_n\xrightarrow{p}0$)인데 $\mathbb EX_n=n\to\infty$. **드물게 나타나는 큰 값**은 확률수렴에는 거의 영향이 없지만 모멘트에는 결정적이다.

---

## 6. CMT, Slutsky, CLT

🧩 [ch3 p5–6] — 세 정리 모두 강의노트에 증명이 없다.

**Theorem 3.5 (Continuous mapping theorem)** $g$가 연속이면

$$X_n\xrightarrow{p}X\Rightarrow g(X_n)\xrightarrow{p}g(X),\qquad X_n\xrightarrow{d}X\Rightarrow g(X_n)\xrightarrow{d}g(X).$$

**Theorem 3.6 (Slutsky)** $X_n\xrightarrow{d}X$, $Y_n\xrightarrow{p}c$ (상수)이면 $X_n+Y_n\xrightarrow{d}X+c$, $X_nY_n\xrightarrow{d}cX$.

**Theorem 3.7 (CLT)** $Y_i$ iid, $\mathbb EY_1=\mu$, $\operatorname{Var}Y_1=\sigma^2 > 0$이면 $\dfrac{\sqrt n(\bar Y_n-\mu)}{\sigma}\xrightarrow{d}Z\sim N(0,1)$.

- 두 정리(CMT, Slutsky)는 표본평균, 표본분산, MLE 등의 극한분포를 유도하는 기본 도구다. CLT + Slutsky로 **모르는 모수를 일치추정량으로 바꾼 통계량**의 점근분포를 얻는다.
- WLLN은 $\bar Y_n$이 $\mu$에 가까워진다는 것 (일치성), CLT는 그 오차를 $\sqrt n$배 확대했을 때의 분포 (신뢰구간·검정의 근거).

**(보충 증명 1) 극한이 상수인 CMT.** $g$가 $c$에서 연속이고 $X_n\xrightarrow{p}c$라 하자. 임의의 $\epsilon > 0$에 대해 $\delta > 0$이 있어 $\lvert x-c\rvert < \delta\Rightarrow\lvert g(x)-g(c)\rvert < \epsilon$. 대우로 $\{\lvert g(X_n)-g(c)\rvert\ge\epsilon\}\subset\{\lvert X_n-c\rvert\ge\delta\}$이므로 $\Pr(\lvert g(X_n)-g(c)\rvert\ge\epsilon)\le\Pr(\lvert X_n-c\rvert\ge\delta)\to0$. ∎ (hw1 #7의 마지막 단계가 이것이다: $g(x)=\lambda/x$는 $x=1$에서 연속)

**(보충 증명 2) Slutsky의 합.** $x-c$가 $F_X$의 연속점이라 하자. Thm 3.3과 같은 샌드위치로

$$\Pr(X_n+Y_n\le x)\le\Pr(X_n\le x-c+\epsilon)+\Pr(\lvert Y_n-c\rvert > \epsilon),\qquad\Pr(X_n+Y_n\le x)\ge\Pr(X_n\le x-c-\epsilon)-\Pr(\lvert Y_n-c\rvert > \epsilon).$$

$x-c\pm\epsilon$이 연속점이 되도록 $\epsilon$을 고르고 (불연속점은 셀 수 있을 만큼뿐) lim sup·lim inf를 취한 뒤 $\epsilon\downarrow0$: $\lim\Pr(X_n+Y_n\le x)=F_X(x-c)=\Pr(X+c\le x)$. ∎

**(보충) 자주 쓰는 사실**
- $X_n\xrightarrow{p}X$, $Y_n\xrightarrow{p}Y$이면 $X_n+Y_n\xrightarrow{p}X+Y$ (∵ $\{\lvert X_n+Y_n-X-Y\rvert > \epsilon\}\subset\{\lvert X_n-X\rvert > \epsilon/2\}\cup\{\lvert Y_n-Y\rvert > \epsilon/2\}$).
- Slutsky의 나눗셈: $c\ne0$이면 $X_n/Y_n\xrightarrow{d}X/c$.
- ⚠ $X_n\xrightarrow{d}X$, $Y_n\xrightarrow{d}Y$만으로는 $X_n+Y_n$의 극한을 알 수 없다 (결합분포가 정해지지 않음) → [V7](#v7-slutsky에-상수-조건이-필요한-이유). Slutsky에서 $Y_n$의 극한이 **상수**여야 하는 이유다.
- **t 통계량:** $\sqrt n(\bar X-\mu)/S_n\xrightarrow{d}N(0,1)$ (CLT + $S_n\xrightarrow{p}\sigma$ + CMT + Slutsky) → [V4](#v4-표본분산과-t-통계량)

---

## 7. 보충 최댓값과 최솟값 다루기

(보충) Ex 3.5와 hw1 #7을 푸는 공통 도구. [T16][T17]

| 도구 | 식 |
| --- | --- |
| 최댓값의 CDF (iid) | $\Pr(X_{(n)}\le x)=F(x)^n$ |
| 최솟값의 생존함수 (iid) | $\Pr(X_{(1)} > x)=\{1-F(x)\}^n$ |
| 위쪽 꼬리 (합집합) | $\Pr(X_{(n)}\ge x)\le n\Pr(X_1\ge x)$ |
| 지수 극한 | $(1-x/n)^n\to e^{-x}$, 더 일반적으로 $a_n\to a$이면 $(1-a_n/n)^n\to e^{-a}$ |
| 부등식 | $1-x\le e^{-x}$ → $(1-p)^n\le e^{-np}$ |

**"$M_n/c_n\xrightarrow{p}1$을 보여라"의 틀:** $0 < \epsilon < 1$에 대해 **두 꼬리**를 따로 보인다.
1. 아래 꼬리 $\Pr(M_n\le(1-\epsilon)c_n)=F((1-\epsilon)c_n)^n\to0$ — 보통 $1-x\le e^{-x}$로
2. 위 꼬리 $\Pr(M_n\ge(1+\epsilon)c_n)\le n\{1-F((1+\epsilon)c_n)\}\to0$ — 합집합으로
3. $\{\lvert M_n/c_n-1\rvert > \epsilon\}$은 두 사건의 합집합에 포함 → 확률 → 0

**예**
- $\text{Unif}(0,\theta)$: $X_{(n)}\xrightarrow{p}\theta$, $\frac n\theta(\theta-X_{(n)})\xrightarrow{d}\text{Exp}(1)$ → [V1](#v1-균등분포-최댓값의-세-가지-수렴)
- $\text{Exp}$ (rate $\lambda$): $M_n/\log n\xrightarrow{p}1/\lambda$ (hw1 #7), $M_n-\frac{\log n}\lambda\xrightarrow{d}$ Gumbel → [V3](#v3-지수분포-최댓값과-gumbel)
- $\text{Exp}$ (rate $\lambda$)의 최솟값: $\Pr(X_{(1)} > x)=e^{-n\lambda x}$ → $X_{(1)}\sim\text{Exp}(n\lambda)$ 정확히, $nX_{(1)}\sim\text{Exp}(\lambda)$

---

## 8. 보충 almost sure 수렴과 Borel–Cantelli

`lecture_ch3_supplementary_almost_sure_convergence.pdf` (제목은 "Almost sure 수렴과 SGD"지만 2쪽 안에 SGD 내용은 없다)

확률수렴은 "큰 $n$에서 오차가 클 확률이 작다"는 뜻이고, a.s. 수렴은 "하나의 난수열로 만든 거의 모든 표본 경로에서, 충분히 긴 시간이 지나면 오차가 임의로 작아진다"는 더 강한 뜻이다. a.s.를 보일 때는 보통 Borel–Cantelli를 쓴다.

### 8.1 무한히 자주 일어나는 사건

📐 [보충 p1 (1.1)]

$$\{A_n\ \text{i.o.}\}=\bigcap_{N=1}^\infty\bigcup_{n=N}^\infty A_n .$$

어느 시점 $N$ 이후를 보더라도 $A_n$이 적어도 한 번 다시 일어난다는 뜻이다 (i.o. = infinitely often).

### 8.2 Borel–Cantelli

🧩 [보충 p1 Thm 1.1, Remark]

**First Borel–Cantelli** $\sum_{n=1}^\infty\Pr(A_n) < \infty\Rightarrow\Pr(A_n\ \text{i.o.})=0$. **독립성이 필요 없다.**

**(보충 증명)** 모든 $N$에서 $\{A_n\text{ i.o.}\}\subset\bigcup_{n\ge N}A_n$이므로

$$\Pr(A_n\ \text{i.o.})\le\Pr\Big(\bigcup_{n\ge N}A_n\Big)\le\sum_{n\ge N}\Pr(A_n)\xrightarrow[N\to\infty]{}0$$

(합집합 부등식, 그리고 수렴하는 급수의 꼬리합은 0으로 간다). ∎

**Second Borel–Cantelli** 사건들이 **독립**이고 $\sum\Pr(A_n)=\infty$이면 $\Pr(A_n\ \text{i.o.})=1$.

### 8.3 Thm 2.1 almost sure 수렴의 충분조건

📐 [보충 p1–2 Thm 2.1]

**결론** 모든 $\epsilon > 0$에 대해 $\sum_{n=1}^\infty\Pr(\lvert X_n-X\rvert > \epsilon) < \infty$ (2.1)이면 $X_n\xrightarrow{\text{a.s.}}X$.

**핵심 한 수** $\epsilon=1/m$마다 Borel–Cantelli → 셀 수 있는 교집합.

**증명**
1. $m=1,2,\dots$에 대해 $A_n^{(m)}=\{\lvert X_n-X\rvert > 1/m\}$. (2.1)과 First BC로 $\Pr(A_n^{(m)}\text{ i.o.})=0$.
2. $\Omega_0=\bigcap_{m=1}^\infty\{A_n^{(m)}\text{가 유한 번만 일어남}\}$은 확률 0인 사건 셀 수 있게 많은 것의 여사건의 교집합이라 $\Pr(\Omega_0)=1$.
3. $\omega\in\Omega_0$, $\epsilon > 0$을 고정하고 $1/m < \epsilon$인 $m$을 고른다. $A_n^{(m)}$이 유한 번만 일어나므로 어떤 $N=N(\omega,m)$ 이후 $\lvert X_n(\omega)-X(\omega)\rvert\le1/m < \epsilon$. 즉 $X_n(\omega)\to X(\omega)$.
4. 이것이 확률 1인 $\Omega_0$에서 성립하므로 $X_n\xrightarrow{\text{a.s.}}X$. ∎

**Example 2.1 (지수형 꼬리)** 모든 $\epsilon$에 대해 $\Pr(\lvert X_n-X\rvert > \epsilon)\le2e^{-c_\epsilon n}$이면 우변의 급수가 수렴하므로 $X_n\xrightarrow{\text{a.s.}}X$. **표본평균에 대한 Hoeffding**이 이 방법으로 (유계 변수의) 강한 대수의 법칙을 주는 전형적인 예다 → ch1 [V9](ch1%20집중부등식.md#v9-hoeffding으로-강한-대수의-법칙). Chebyshev의 $\sigma^2/(n\epsilon^2)$은 합이 발산해서 이 방법이 통하지 않는다.

---

## hw1 풀이

이 장에 속하는 문제: #6, #7. 둘 다 **비유도형**이라 도구를 스스로 골라야 한다.

### hw1 6 AR(1) 표본평균의 qm 수렴

📝 [hw1 p4] · 내 풀이 [CamScanner p8]

**문제 요지** $\epsilon_1,\epsilon_2,\dots$ 독립, 평균 $\mu$, 분산 $\sigma^2$. $X_1=\epsilon_1$, $X_n=\beta X_{n-1}+\epsilon_n$ ($n\ge2$, $-1\le\beta < 1$). $\bar X_n=n^{-1}\sum_{i=1}^nX_i\xrightarrow{qm}\mu/(1-\beta)$를 보여라.

**전략** "qm → 상수"는 **편향–분산 분해** [3.2](#32-보충-편향분산-분해)로 평균과 분산을 계산하는 문제다. $X_k$가 $\epsilon$들의 선형결합이므로 먼저 닫힌 꼴을 구하고, $\sum_kX_k$에서 **이중합의 순서를 바꿔** $\epsilon_j$의 계수를 모은다.

**모범 답안**

1. **닫힌 꼴.** 귀납법으로 $X_k=\sum_{j=1}^k\beta^{k-j}\epsilon_j$. ($k=1$: $X_1=\epsilon_1$ ✓. $X_k=\beta X_{k-1}+\epsilon_k=\sum_{j=1}^{k-1}\beta^{k-j}\epsilon_j+\epsilon_k$ ✓)

2. **합의 계수.** 합의 순서를 바꾸면

$$\sum_{k=1}^nX_k=\sum_{k=1}^n\sum_{j=1}^k\beta^{k-j}\epsilon_j=\sum_{j=1}^n\Big(\sum_{k=j}^n\beta^{k-j}\Big)\epsilon_j=\sum_{j=1}^nc_{n,j}\,\epsilon_j,\qquad c_{n,j}=\sum_{m=0}^{n-j}\beta^m=\frac{1-\beta^{n-j+1}}{1-\beta}$$

   ($\beta\ne1$이라 등비합 공식을 쓸 수 있다).

3. **평균.**

$$\mathbb E\bar X_n=\frac\mu n\sum_{j=1}^nc_{n,j}=\frac{\mu}{n(1-\beta)}\sum_{j=1}^n\big(1-\beta^{n-j+1}\big)=\frac{\mu}{1-\beta}\Big(1-\frac1n\sum_{m=1}^n\beta^m\Big).$$

   $\lvert\sum_{m=1}^n\beta^m\rvert$는 $n$에 대해 유계다 ($\lvert\beta\rvert < 1$이면 $\le\frac{\lvert\beta\rvert}{1-\lvert\beta\rvert}$, $\beta=-1$이면 부분합이 $-1$ 또는 $0$). 따라서 $\frac1n\sum\beta^m\to0$이고 $\mathbb E\bar X_n\to\frac\mu{1-\beta}$.

4. **분산.** $\epsilon_j$가 독립이므로

$$\operatorname{Var}\bar X_n=\frac{\sigma^2}{n^2}\sum_{j=1}^nc_{n,j}^2 .$$

   $\lvert\beta\rvert\le1$이라 $\lvert1-\beta^{m}\rvert\le1+\lvert\beta\rvert^m\le2$, 그래서 $\lvert c_{n,j}\rvert\le\frac2{1-\beta}$. 따라서 $\operatorname{Var}\bar X_n\le\dfrac{\sigma^2}{n^2}\cdot n\cdot\dfrac4{(1-\beta)^2}=\dfrac{4\sigma^2}{n(1-\beta)^2}\to0$.

5. **결론.** 편향–분산 분해로

$$\mathbb E\Big(\bar X_n-\frac\mu{1-\beta}\Big)^2=\operatorname{Var}\bar X_n+\Big(\mathbb E\bar X_n-\frac\mu{1-\beta}\Big)^2\to0,$$

   즉 $\bar X_n\xrightarrow{qm}\dfrac\mu{1-\beta}$. ∎

**채점 포인트**
- 닫힌 꼴과 **합 순서 교환**을 식으로 보이기 ($c_{n,j}$)
- $\beta\ne1$ (등비합), $\lvert\beta\rvert\le1$ (계수가 유계)을 **어디서 쓰는지** 밝히기. $\beta=-1$도 범위에 들어 있다
- 분산 계산에서 독립(사실 비상관이면 충분)을 쓴다고 밝히기
- 편향–분산 분해로 qm을 결론 (평균·분산 → 0만 쓰고 끝내지 않기)

**내 풀이 점검**
- ✅ 닫힌 꼴, $c_{n,j}=\frac{1-\beta^{n-j+1}}{1-\beta}$, $\mathbb E\bar X_n\to\frac\mu{1-\beta}$, $\lvert c_{n,j}\rvert\le\frac2{1-\beta}$, $\operatorname{Var}\le\frac{4\sigma^2}{n(1-\beta)^2}$, 편향–분산 분해까지 모범 답안과 같다.
- ⚠ "$-1\le\beta < 1$이므로 $\frac1n\sum\beta^m\to0$"의 이유 (부분합이 유계)를 한 줄 쓰면 좋다. 특히 $\beta=-1$은 $\lvert\beta\rvert < 1$의 등비급수 논리가 안 통하므로 따로 말해야 한다.

**더 생각해 보기 (보충)** $n\operatorname{Var}\bar X_n=\frac{\sigma^2}{n}\sum c_{n,j}^2\to\frac{\sigma^2}{(1-\beta)^2}$ ($\lvert\beta\rvert < 1$일 때, "장기 분산"). iid이면 $\sigma^2$인데 양의 상관($\beta > 0$)이 있으면 표본평균의 분산이 커진다. ($\beta=-1$이면 $c_{n,j}\in\{0,1\}$이 번갈아 나와 $\sigma^2/2$로 간다.)

### hw1 7 log n을 최댓값으로 나눈 것의 일치성

📝 [hw1 p4] · 내 풀이 [CamScanner p8]

**문제 요지** $X_i\overset{\text{iid}}{\sim}$ 평균 $1/\lambda$인 지수분포, $M_n=\max_iX_i$. $\log n/M_n\xrightarrow{p}\lambda$인가?

**전략** 답은 **예**. $\log n/M_n$을 직접 다루기보다 $M_n/\log n\xrightarrow{p}1/\lambda$를 보이고 CMT로 뒤집는다. 최댓값은 CDF로 두 꼬리를 따로 누른다 [7](#7-보충-최댓값과-최솟값-다루기).

**모범 답안**

**주장:** $\log n/M_n$은 $\lambda$의 일치추정량이다.

1. **표준화.** $Y_i=\lambda X_i\sim\text{Exp}(1)$ ($\Pr(Y_i > y)=e^{-y}$), $N_n=\max_iY_i=\lambda M_n$. $N_n/\log n\xrightarrow{p}1$을 보이면 된다.
2. **아래 꼬리.** $0 < \epsilon < 1$에 대해 (∵ iid, $1-x\le e^{-x}$)

$$\Pr\big(N_n\le(1-\epsilon)\log n\big)=\big(1-e^{-(1-\epsilon)\log n}\big)^n=\big(1-n^{-(1-\epsilon)}\big)^n\le\exp\big(-n\cdot n^{-(1-\epsilon)}\big)=e^{-n^\epsilon}\to0 .$$

3. **위 꼬리.** 합집합 부등식으로

$$\Pr\big(N_n\ge(1+\epsilon)\log n\big)\le n\Pr\big(Y_1\ge(1+\epsilon)\log n\big)=n\cdot n^{-(1+\epsilon)}=n^{-\epsilon}\to0 .$$

4. **확률수렴.** $\{\lvert N_n/\log n-1\rvert > \epsilon\}\subset\{N_n\le(1-\epsilon)\log n\}\cup\{N_n\ge(1+\epsilon)\log n\}$이므로 확률 → 0. ($\epsilon\ge1$이면 아래 꼬리는 $\Pr(N_n\le0)=0$이라 자명) 따라서 $N_n/\log n\xrightarrow{p}1$.
5. **CMT.** $g(x)=\lambda/x$는 $x=1$에서 연속이므로 CMT (Thm 3.5)로

$$\frac{\log n}{M_n}=\frac{\lambda}{N_n/\log n}=g\Big(\frac{N_n}{\log n}\Big)\xrightarrow{p}g(1)=\lambda .\qquad\blacksquare$$

**채점 포인트**
- 첫 줄에 **"예, 일치추정량이다"**라고 주장
- 지수분포의 모수화 확인: 평균 $1/\lambda$ ⟹ $F(x)=1-e^{-\lambda x}$
- **두 꼬리 모두** (위 꼬리만 보이면 반쪽 증명)
- 아래 꼬리의 $1-x\le e^{-x}$, 위 꼬리의 합집합
- 마지막 CMT 단계와 $g$의 연속점 명시

**내 풀이 점검**
- ✅ 표준화 $Y_i=\lambda X_i$, 두 꼬리의 계산 ($e^{-n^\epsilon}$, $n^{-\epsilon}$), 결론 모두 맞다.
- ⚠ 마지막 줄 $\frac{\log n}{M_n}=\lambda\frac{\log n}{N_n}\xrightarrow{p}\lambda$에서 **CMT를 쓴다는 말과 $g(x)=\lambda/x$가 1에서 연속**이라는 말이 빠졌다. "일치추정량인가?"라는 질문이니 첫 줄에 "예"라고 주장하는 문장도 넣는다.

**더 생각해 보기 (보충)** $N_n-\log n\xrightarrow{d}$ Gumbel ([V3](#v3-지수분포-최댓값과-gumbel))이라 $N_n/\log n-1=O_p(1/\log n)$ — 일치이긴 하지만 **수렴이 아주 느리다** ($\sqrt n$도 $n$도 아니고 $\log n$). 실제로 쓸 추정량은 아니다 (MLE는 $1/\bar X$).

---

## 변형 문제

난이도: `●` hw 문제의 분포만 바꿈 · `●●` 도구 두세 개 조합 · `●●●` 장을 넘나듦. 힌트부터 열고, 스스로 써 본 뒤 풀이를 연다.

### V1 균등분포 최댓값의 세 가지 수렴

`●●` $X_i\overset{\text{iid}}{\sim}\text{Unif}(0,\theta)$, $\hat\theta=X_{(n)}$.
(a) $\hat\theta\xrightarrow{p}\theta$. (b) $\frac n\theta(\theta-\hat\theta)\xrightarrow{d}\text{Exp}(1)$. (c) $\hat\theta\xrightarrow{\text{a.s.}}\theta$. (d) $2\bar X$도 일치추정량이다. $\sqrt n(2\bar X-\theta)$의 극한분포를 구하고, (b)와 수렴 속도를 비교하라.

<details><summary>전략 힌트</summary>

(a)(b) Ex 3.5를 $\theta$배. (c) (a)의 꼬리확률이 등비수열 → Thm 2.1. (d) CLT, $\operatorname{Var}X=\theta^2/12$.

</details>
<details><summary>풀이</summary>

(a) $0 < \epsilon < \theta$: $\Pr(\theta-\hat\theta > \epsilon)=\Pr(X_{(n)} < \theta-\epsilon)=(1-\epsilon/\theta)^n\to0$.

(b) $t\ge0$: $\Pr\big(\frac n\theta(\theta-\hat\theta)\le t\big)=1-\Pr\big(X_{(n)} < \theta(1-\frac tn)\big)=1-(1-\frac tn)^n\to1-e^{-t}$.

(c) $\sum_n(1-\epsilon/\theta)^n < \infty$ → 보충 Thm 2.1.

(d) $\mathbb E(2X)=\theta$, $\operatorname{Var}(2X)=4\cdot\frac{\theta^2}{12}=\frac{\theta^2}3$. WLLN으로 일치, CLT로 $\sqrt n(2\bar X-\theta)\xrightarrow{d}N(0,\theta^2/3)$. $X_{(n)}$의 오차는 $1/n$, $2\bar X$의 오차는 $1/\sqrt n$ 크기 → 최댓값이 훨씬 빠르다.

</details>

### V2 원판 위 균등분포의 극한분포

`●●` [2021 중간 #2, 다른 교수 기출] $(X_i,Y_i)$가 원점 중심, 반지름 $r$인 원판 위의 균등분포 $f(x,y)=\frac1{\pi r^2}I(x^2+y^2\le r^2)$에서 iid. $\theta=r^2$.
(a) $R_i^2=X_i^2+Y_i^2\sim\text{Unif}(0,\theta)$임을 보여라.
(b) $\theta$의 MLE가 $\hat\theta=\max_iR_i^2$임을 보여라 (ch5 내용이지만 우도만 쓰면 된다).
(c) $a_n(\hat\theta-b_n)$의 극한분포가 평균 1인 연속분포가 되도록 $a_n,b_n$을 구하라.

<details><summary>전략 힌트</summary>

(a) $\Pr(R^2\le s)$ = 반지름 $\sqrt s$ 원의 넓이 / $\pi r^2$. (b) 우도 $\theta^{-n}I(\max R_i^2\le\theta)$. (c) V1(b)와 같은 구조 — 부호에 주의.

</details>
<details><summary>풀이</summary>

(a) $0\le s\le\theta$: $\Pr(R^2\le s)=\frac{\pi s}{\pi\theta}=\frac s\theta$ → $\text{Unif}(0,\theta)$.

(b) $L(\theta)=\prod\frac1{\pi\theta}I(x_i^2+y_i^2\le\theta)=(\pi\theta)^{-n}I(\max R_i^2\le\theta)$ — $\theta\ge\max R_i^2$에서 감소함수라 $\hat\theta=\max R_i^2$.

(c) V1(b)로 $\frac n\theta(\theta-\hat\theta)\xrightarrow{d}\text{Exp}(1)$ (평균 1). 즉 $a_n=-\frac n\theta$, $b_n=\theta$. (기출 풀이도 $-\frac n\theta(\hat\theta-\theta)$)

</details>

### V3 지수분포 최댓값과 Gumbel

`●●` $Y_i\overset{\text{iid}}{\sim}\text{Exp}(1)$, $N_n=\max_iY_i$.
(a) $N_n-\log n\xrightarrow{d}G$, $\Pr(G\le x)=\exp(-e^{-x})$ (Gumbel)을 보여라.
(b) (a)와 Slutsky, Thm 3.4로 $N_n/\log n\xrightarrow{p}1$을 다시 보여라 (hw1 #7의 다른 증명).

<details><summary>전략 힌트</summary>

(a) $\Pr(N_n\le x+\log n)=(1-e^{-x}/n)^n$. (b) $\frac{N_n}{\log n}-1=(N_n-\log n)\cdot\frac1{\log n}$, 뒤의 인자 → 0 (상수).

</details>
<details><summary>풀이</summary>

(a) 고정된 $x$에 대해 $n$이 크면 $x+\log n > 0$이고 $\Pr(N_n-\log n\le x)=(1-e^{-x-\log n})^n=(1-e^{-x}/n)^n\to\exp(-e^{-x})$. 모든 $x$에서 성립 (극한 CDF가 연속).

(b) Slutsky (곱): $(N_n-\log n)\cdot\frac1{\log n}\xrightarrow{d}G\cdot0=0$. 상수로의 분포수렴이므로 Thm 3.4로 확률수렴. 따라서 $N_n/\log n-1\xrightarrow{p}0$.

</details>

### V4 표본분산과 t 통계량

`●●` $X_i$ iid, 평균 $\mu$, 분산 $\sigma^2\in(0,\infty)$. $S_n^2=\frac1{n-1}\sum(X_i-\bar X)^2$.
(a) $S_n^2\xrightarrow{p}\sigma^2$. (b) $T_n=\sqrt n(\bar X-\mu)/S_n\xrightarrow{d}N(0,1)$.

<details><summary>전략 힌트</summary>

(a) $S_n^2=\frac n{n-1}\big(\frac1n\sum X_i^2-\bar X^2\big)$ + 대수의 법칙 두 번 + CMT. $\frac1n\sum X_i^2$에는 SLLN (Ex 3.1: 1차 적률만 필요 — $\mathbb EX^2 < \infty$)을 쓰면 4차 적률 가정이 필요 없다. (b) CLT + CMT + Slutsky.

</details>
<details><summary>풀이</summary>

(a) SLLN으로 $\frac1n\sum X_i^2\xrightarrow{\text{a.s.}}\mathbb EX^2$ (따라서 $\xrightarrow{p}$), WLLN으로 $\bar X\xrightarrow{p}\mu$, CMT로 $\bar X^2\xrightarrow{p}\mu^2$, 합의 확률수렴과 $\frac n{n-1}\to1$로 $S_n^2\xrightarrow{p}\mathbb EX^2-\mu^2=\sigma^2$.

(b) $T_n=\frac{\sqrt n(\bar X-\mu)}{\sigma}\cdot\frac\sigma{S_n}$. 첫 인자 $\xrightarrow{d}Z$ (CLT). 둘째 인자: $g(s)=\sigma/\sqrt s$는 $s=\sigma^2 > 0$에서 연속이라 CMT로 $\xrightarrow{p}1$. Slutsky (곱)로 $T_n\xrightarrow{d}Z\sim N(0,1)$.

</details>

### V5 분산이 다른 독립 변수의 대수의 법칙

`●` $X_i$ 독립, $\mathbb EX_i=\mu$, $\operatorname{Var}X_i=i^a$ ($a\ge0$). 어떤 $a$에서 $\bar X_n\xrightarrow{qm}\mu$인가?

<details><summary>풀이</summary>

$\mathbb E\bar X_n=\mu$ (편향 0), $\operatorname{Var}\bar X_n=\frac1{n^2}\sum_{i=1}^ni^a$. $\sum_{i\le n}i^a$는 $\frac{n^{a+1}}{a+1}$ 크기이므로 $\operatorname{Var}\bar X_n\approx\frac{n^{a-1}}{a+1}\to0\iff a < 1$. 따라서 $0\le a < 1$이면 qm (그리고 p) 수렴. ($a=1$이면 분산 → 1/2)

</details>

### V6 독립 Bernoulli로 본 수렴의 차이

`●●` $X_n$ 독립, $X_n\sim\text{Bernoulli}(p_n)$.
(a) $p_n=1/n$: $X_n\xrightarrow{p}0$, $X_n\xrightarrow{qm}0$이지만 $X_n\not\xrightarrow{\text{a.s.}}0$임을 보여라.
(b) $p_n=1/n^2$: $X_n\xrightarrow{\text{a.s.}}0$임을 보여라.

<details><summary>풀이</summary>

(a) $\Pr(\lvert X_n\rvert > \epsilon)=\frac1n\to0$ ($0 < \epsilon < 1$), $\mathbb EX_n^2=\frac1n\to0$. 사건 $\{X_n=1\}$은 독립이고 $\sum\frac1n=\infty$ → Second Borel–Cantelli로 확률 1로 무한히 자주 $X_n=1$ → 거의 모든 경로에서 $X_n$은 0으로 수렴하지 않는다. → **qm이지만 a.s. 아님** (5의 표).

(b) $\sum\Pr(\lvert X_n\rvert > \epsilon)=\sum\frac1{n^2} < \infty$ → Thm 2.1.

</details>

### V7 Slutsky에 상수 조건이 필요한 이유

`●` $Z\sim N(0,1)$. (a) $X_n=Z$, $Y_n=Z$이면 $X_n\xrightarrow{d}Z$, $Y_n\xrightarrow{d}Z$이고 $X_n+Y_n=2Z$. (b) $X_n=Z$, $Y_n=-Z$이면 $X_n\xrightarrow{d}Z$, $Y_n\xrightarrow{d}Z$인데 $X_n+Y_n=0$. 이 두 예가 보여 주는 것은?

<details><summary>풀이</summary>

(b)에서 $-Z\overset{d}{=}Z$라 $Y_n\xrightarrow{d}Z$. 두 경우 모두 주변분포의 극한은 같은데 합의 극한은 $N(0,4)$와 $0$으로 다르다 → 각각의 분포수렴만으로는 합의 극한이 정해지지 않는다 (결합분포가 필요). Slutsky는 한쪽 극한이 **상수**라서 결합분포 문제가 없다.

</details>

### V8 AR 대신 MA(1)

`●●` $\epsilon_0,\epsilon_1,\dots$ 독립, 평균 $\mu$, 분산 $\sigma^2$. $X_n=\epsilon_n+\theta\epsilon_{n-1}$ ($n\ge1$). (a) $\bar X_n\xrightarrow{qm}(1+\theta)\mu$. (b) $\lim n\operatorname{Var}\bar X_n$을 구하라.

<details><summary>전략 힌트</summary>

hw1 #6과 같다: $\sum X_k$에서 $\epsilon_j$의 계수를 모은다.

</details>
<details><summary>풀이</summary>

$\sum_{k=1}^nX_k=\theta\epsilon_0+(1+\theta)\sum_{j=1}^{n-1}\epsilon_j+\epsilon_n$.

(a) $\mathbb E\sum X_k=\theta\mu+(1+\theta)(n-1)\mu+\mu=(1+\theta)n\mu$ → $\mathbb E\bar X_n=(1+\theta)\mu$ (편향 0). $\operatorname{Var}\bar X_n=\frac{\sigma^2}{n^2}\{\theta^2+(1+\theta)^2(n-1)+1\}\to0$. 편향–분산 분해로 qm.

(b) $n\operatorname{Var}\bar X_n\to(1+\theta)^2\sigma^2$.

</details>

### V9 최솟값의 정확한 분포

`●` $X_i\overset{\text{iid}}{\sim}\text{Exp}$ (rate $\lambda$). (a) $X_{(1)}\sim\text{Exp}(n\lambda)$. (b) $X_{(1)}\xrightarrow{p}0$이고 $X_{(1)}\xrightarrow{qm}0$. (c) $nX_{(1)}$의 분포는?

<details><summary>풀이</summary>

(a) $\Pr(X_{(1)} > x)=\prod\Pr(X_i > x)=e^{-n\lambda x}$. (b) $\Pr(X_{(1)} > \epsilon)=e^{-n\lambda\epsilon}\to0$, $\mathbb EX_{(1)}^2=\frac2{(n\lambda)^2}\to0$. (c) $\Pr(nX_{(1)} > x)=e^{-\lambda x}$ → $\text{Exp}(\lambda)$, $n$과 무관 (최솟값은 $n$배로 확대하면 퇴화하지 않는다).

</details>

### V10 확률수렴과 기댓값

`●●` $U\sim\text{Unif}(0,1)$, $X_n=n\,I(U\le1/n)$. (a) $X_n\xrightarrow{\text{a.s.}}0$. (b) $\mathbb EX_n$은? (c) $X_n\xrightarrow{qm}0$인가? (d) 이 예에서 $X_n\xrightarrow{p}0$인데 기댓값이 0으로 가지 않는 것이 Remark와 어떻게 같은가?

<details><summary>풀이</summary>

(a) $U(\omega) > 0$이면 $n > 1/U(\omega)$에서 $X_n(\omega)=0$. (b) $\mathbb EX_n=n\cdot\frac1n=1$. (c) $\mathbb EX_n^2=n^2\cdot\frac1n=n\to\infty$ → 아님. (d) 드문 사건 (확률 $1/n$) 위의 큰 값 ($n$)이 기댓값을 1로 붙잡는다 — 확률수렴은 드문 사건을 무시하지만 모멘트는 무시하지 않는다.

</details>

---

## 셀프 테스트

### 백지 증명 체크리스트

| ☐ | 증명 | 뼈대 | 위치 |
| --- | --- | --- | --- |
| ☐ | WLLN | $\operatorname{Var}\bar Y=\sigma^2/n$ → Chebyshev | [2](#2-확률수렴과-약한-대수의-법칙) |
| ☐ | qm ⟹ p | $(X_n-X)^2$에 Markov | [3.1](#31-정의와-thm-32) |
| ☐ | 편향–분산 분해 | 가운데 $\mathbb EX_n$ 더하고 빼기, 교차항 0 | [3.2](#32-보충-편향분산-분해) |
| ☐ | Ex 3.2 | $\Pr=1/n$, $\mathbb EX_n^2=1$ | [3.3](#33-ex-32-역은-성립하지-않는다) |
| ☐ | **p ⟹ d** | 두 사건 분할 → $F(x-\epsilon)-\Pr\le F_n\le F(x+\epsilon)+\Pr$ → lim inf/sup → $\epsilon\downarrow0$ (연속점) | [4.2](#42-thm-33-확률수렴이면-분포수렴) |
| ☐ | Ex 3.4 | $X_n=-X$, $\lvert X_n-X\rvert=2\lvert X\rvert$ | [4.3](#43-ex-34-역은-성립하지-않는다) |
| ☐ | **Ex 3.5** | $(1-\epsilon)^n$, $1-(1-t/n)^n\to1-e^{-t}$ | [4.4](#44-ex-35-표본최댓값) |
| ☐ | 상수로의 d ⟹ p | $\le1-F_n(c+\epsilon)+F_n(c-\epsilon)$, $c\pm\epsilon$은 연속점 | [4.5](#45-thm-34-상수로의-분포수렴) |
| ☐ | CMT (상수 극한, 보충) | $\epsilon$–$\delta$ 연속성의 대우 → 사건 포함 | [6](#6-cmt-slutsky-clt) |
| ☐ | Slutsky 합 (보충) | Thm 3.3과 같은 샌드위치, $x-c\pm\epsilon$ 연속점 | [6](#6-cmt-slutsky-clt) |
| ☐ | First BC (보충) | $\le\sum_{n\ge N}\Pr(A_n)\to0$ | [8.2](#82-borelcantelli) |
| ☐ | **Thm 2.1** | $\epsilon=1/m$마다 BC → $\Omega_0$ 셀 수 있는 교집합 → 경로별 수렴 | [8.3](#83-thm-21-almost-sure-수렴의-충분조건) |
| ☐ | **hw1 #6** | 닫힌 꼴 → 합 순서 교환 $c_{n,j}$ → 평균 → 분산 $\le\frac{4\sigma^2}{n(1-\beta)^2}$ → 편향–분산 | [hw1 #6](#hw1-6-ar1-표본평균의-qm-수렴) |
| ☐ | **hw1 #7** | $Y=\lambda X$ → 아래 꼬리 $e^{-n^\epsilon}$ → 위 꼬리 $n^{-\epsilon}$ → CMT $g(x)=\lambda/x$ | [hw1 #7](#hw1-7-log-n을-최댓값으로-나눈-것의-일치성) |

### 짧은 문답

<details><summary>1. 네 가지 수렴의 포함관계를 그려라.</summary>

a.s. ⟹ p ⟹ d, qm ⟹ p ⟹ d. a.s.와 qm은 서로 무관. d ⟹ p는 극한이 상수일 때만.

</details>
<details><summary>2. "상수로의 qm 수렴"은 무엇을 확인하면 되나?</summary>

$\mathbb EX_n\to c$와 $\operatorname{Var}X_n\to0$ (편향–분산 분해).

</details>
<details><summary>3. 분포수렴의 정의에서 연속점 조건이 필요함을 보이는 예는?</summary>

Ex 3.3: $X_n\sim N(0,1/n)$, $F_n(0)=1/2$, $F(0)=1$인데 $X_n\xrightarrow{d}0$.

</details>
<details><summary>4. 확률수렴하지만 qm 수렴하지 않는 예와, a.s.이지만 qm 아닌 예는?</summary>

둘 다 Ex 3.2: $X_n=\sqrt n\,I(U\le1/n)$.

</details>
<details><summary>5. qm이지만 a.s. 아닌 예는?</summary>

독립 $X_n\sim\text{Bernoulli}(1/n)$ (Second Borel–Cantelli).

</details>
<details><summary>6. Uniform(0,1) 최댓값을 확대하는 속도와 극한분포는?</summary>

$n(1-X_{(n)})\xrightarrow{d}\text{Exp}(1)$.

</details>
<details><summary>7. Slutsky에서 Yₙ의 극한이 상수여야 하는 이유는?</summary>

각각의 분포수렴만으로는 합의 극한이 정해지지 않는다 ($X_n=Z$, $Y_n=\pm Z$ 예).

</details>
<details><summary>8. Borel–Cantelli로 a.s.를 보일 때 Chebyshev가 보통 안 통하는 이유는?</summary>

$\sum_n\frac{\sigma^2}{n\epsilon^2}=\infty$. 지수형 꼬리(Hoeffding)나 더 빠른 꼬리가 필요하다.

</details>

---

## 강의노트 확인

강의노트 6쪽과 보충 2쪽을 텍스트로 전부 읽었다 (수식이 유니코드로 잘 추출됨). 수식 오류는 찾지 못했다. 철자 오타(Fitst)는 조용히 고쳤다.

| 쪽 | 강의노트 원문 | 노트에 반영한 것 | 근거 | 영향 |
| --- | --- | --- | --- | --- |
| 보충 p1 제목 | "Almost sure 수렴과 SGD" | SGD 내용은 2쪽 안에 없음을 명시 | 본문 확인 | · |
| p3 Ex 3.2 | "충분히 큰 $n$에서 $\Pr(\lvert X_n\rvert > \epsilon)=1/n$" | "$\sqrt n > \epsilon$인 $n$에서"로 조건을 구체화 | $X_n\in\{0,\sqrt n\}$ | · |
| p5 관계도 | a.s. ⟹ p를 증명 없이 사용 | 표에 "(강의노트는 증명 없음)" 표시 | – | · |
| p5 Thm 3.5–3.7 | 증명 없음 | CMT(상수 극한)·Slutsky(합) 보충 증명 | 무증명 정리 = 출제 후보 | · |
