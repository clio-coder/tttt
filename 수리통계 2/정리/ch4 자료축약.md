# ch4 자료축약 (Data Reduction)

> **원본:** `lecture_ch4_data_reduction.pdf` (4쪽, 9/15 수업, 교재 8.2절)
> **관련 숙제:** hw1 #8 ($\min X_i$가 충분통계량) · #9 (2차원 충분통계량) · hw1 #5(e)도 충분통계량 · 연결: ch2 (2.9), ch5 MLE, 이후 Rao–Blackwell·UMVUE (교재 8.3절)
> **범례:** 📝 hw1에 나옴 · 🧩 강의노트에 증명 없음 → 보충 증명 · 📐 강의노트에 증명 있음 · 🔢 계산 · 💡 개념·해석 · (보충) 강의노트에 없는 내용 · `[T…]` = [도구상자](00%20문제풀이%20도구상자.md) 카드 번호
> **기호:** $X=(X_1,\dots,X_n)$, $p_\theta(x)$ = 결합 pmf/pdf, $T(X)$ = 통계량, $X_{(1)}=\min_iX_i$, $X_{(n)}=\max_iX_i$, $I(\cdot)$ = 지시함수

---

## 0. 한눈에

### 0.1 범위

| 쪽 | 절 | 내용 | 증명 | hw1 |
| --- | --- | --- | --- | --- |
| p1 | 4.1–4.2 | 왜 자료축약인가, 모수·비모수 모형, 통계량 | – | – |
| p2 | 4.3 | 충분통계량의 정의, Ex 4.1 (포아송), Ex 4.2 (베르누이), 분할로 보기, 비유일성 | 📐 | – |
| p3 | 4.4 | 분해정리 (Thm 4.1) + 이산형 증명, Ex 4.3 (정규) | 📐 | #8, #9, #5(e) |
| p3–4 | 4.5 | 우도함수, 우도 ∝ $g_\theta(T(x))$ | – | – |
| p4 | 4.6 | 최소충분통계량, 우도비 판정법 (Thm 4.2), Ex 4.4 (포아송), Cauchy Remark | 🧩 | #9 (보충) |
| p4 | 4.7 | Summary | – | – |

### 0.2 시험 포인트 (출제 가능성 순)

**★★★ 매우 유력**

1. 📝 **분해정리로 충분통계량 보이기·찾기** — support가 모수에 의존하면 **지시함수**로 처리한다: $\prod_iI(x_i\ge\theta)=I(\min_ix_i\ge\theta)$를 반드시 쓴다 (hw1 #8, #9). → [3.3](#33-보충-support가-모수에-의존할-때), [hw1 #8](#hw1-8-최솟값이-충분통계량), [hw1 #9](#hw1-9-2차원-충분통계량)
2. 🧩 **최소충분통계량 — 우도비 판정법 (Thm 4.2)** 사용과 (보충) 증명. 포아송 (Ex 4.4), 정규, $\text{Unif}(\theta,\theta+1)$ (2차원), hw1 #9는 사실 **1차원** 최소충분. → [5](#5-최소충분통계량)
3. 📐 **분해정리의 증명** (이산형, 양방향) → [3.1](#31-분해정리와-이산형-증명)
4. 📐 **정의(조건부분포)로 충분성 보이기·반박하기** — Ex 4.1, 4.2, $X_1$은 충분하지 않음, (보충) 연속형 예 → [2](#2-충분통계량의-정의), [V6](#v6-충분하지-않음을-보이기), [V7](#v7-연속형에서-정의로-충분성-보이기)

**★★ 유력**

5. 💡 충분통계량은 유일하지 않다 (일대일 변환, 더 잘게 나눈 분할, 전체 자료). 최소충분통계량은 일대일 변환까지만 유일하고 **분할은 유일**하다.
6. 📐 정규: $(\sum X_i,\sum X_i^2)$, $\sigma^2$을 알면 $\bar X$ (항등식) → [3.2](#32-ex-43-정규-표본)
7. 💡 우도 $\propto g_\theta(T(x))$ → 우도에만 근거하는 추론은 $T$로 충분하다 → [4](#4-우도함수)
8. (보충) 지수족의 $\sum t(X_i)$는 최소충분 (모수공간이 열린 집합을 포함할 때) → [5.3](#53-보충-예-정규-균등-지수족)

**★ 가능성 있음**

9. 💡 Cauchy 위치모형: 순서통계량이 최소충분 — 모수 차원이 작아도 저차원 충분통계량이 없을 수 있다.
10. 💡 모수적·비모수적 모형, 통계량의 정의

### 0.3 이 장의 도구

| 도구 | 쓰는 곳 |
| --- | --- |
| [T21] 정의로: $\Pr_\theta(X=x\mid T=t)$를 계산해 $\theta$가 사라지는지 | Ex 4.1, 4.2 |
| [T22] 분해정리 + 지시함수: $\prod I(\cdot)$ → $\min$· $\max$의 지시함수 | hw1 #8, #9 |
| [T23] 우도비 판정법: $p_\theta(y)/p_\theta(x)$가 $\theta$와 무관 ⟺ $T(x)=T(y)$ | 최소충분 |
| [T24] 충분하지 **않음**: $T$가 같은데 조건부 확률이 $\theta$에 의존하는 점 찾기 | $X_1$, $X_1+2X_2$ |
| [T12] 이차형식 항등식 $\sum(x_i-\mu)^2=\sum(x_i-\bar x)^2+n(\bar x-\mu)^2$ | Ex 4.3 |

---

## 1. 왜 자료축약인가

💡 [ch4 p1]

- iid 표본 $X_1,\dots,X_n\sim P_\theta$에서 추론의 목표는 모르는 $\theta$에 관한 질문에 답하는 것이다. 그런데 원자료의 모든 좌표가 $\theta$에 대한 새로운 정보를 주는 것은 아니다. 질문: **어떤 통계량 $T(X)$만 남겨도 $\theta$에 관한 정보를 잃지 않는가?**
- 좋은 축약은 저장 공간을 줄이는 것 이상이다. 우도 계산, 추정, 검정이 $T$만으로 되면 분석 구조가 단순해진다. 단 **"차원이 작다"만으로 좋은 축약이 되지는 않는다.** 핵심은 모수에 관한 정보가 보존되는가다.
- **통계모형** $\mathcal P=\lbrace p_\theta:\theta\in\Theta\rbrace$.
  - 모수적 모형: $\Theta\subseteq\mathbb R^d$ (예: $N(\mu,\sigma^2)$에서 $\theta=(\mu,\sigma){}$)
  - 비모수적 모형: 유한 차원 모수로 기술되지 않음 (예: 실수축 위의 모든 분포, 일정한 매끄러움을 만족하는 모든 밀도)
  - 충분성은 주로 모수적 틀에서 자연스럽게 쓰인다.
- **통계량:** 표본 $X$의 측정 가능한 함수 $T(X)$. 자료의 함수이므로 확률변수다. 예: $\bar X$, $S^2$, $X_{(1)}$, $X_{(n)}$, 순서통계량 $(X_{(1)},\dots,X_{(n)})$.

---

## 2. 충분통계량의 정의

📐 [ch4 p2]

**정의** $T(X)$가 $\theta$에 대해 **충분(sufficient)** ⟺ 모든 $t$에 대해 조건부분포 $p_\theta(X\mid T(X)=t)$가 $\theta$에 의존하지 않는다.

- 표본공간을 $A_t=\lbrace x:T(x)=t\rbrace$로 분할하면, 각 $A_t$ 안에서는 $\theta$의 정보가 같다. $T=t$를 안 뒤의 세부 배열은 더 이상 $\theta$를 구별하지 못한다.
- 연속형에서는 $T=t$가 확률 0인 사건이라 정의로 직접 계산하기 어렵다 → 보통 **분해정리**를 쓴다.

**Example 4.1 (포아송)** $X_i\overset{\text{iid}}{\sim}\text{Poi}(\theta)$, $T=\sum X_i\sim\text{Poi}(n\theta)$. $\sum x_i=t$이면

$$\Pr_\theta(X=x\mid T=t)=\frac{\Pr_\theta(X=x)}{\Pr_\theta(T=t)}=\frac{e^{-n\theta}\theta^t/\prod_ix_i!}{e^{-n\theta}(n\theta)^t/t!}=\frac{t!}{n^t\prod_{i=1}^nx_i!}$$

($\lbrace X=x\rbrace\subset\lbrace T=t\rbrace$라 분자가 결합확률과 같다). $\theta$가 없으므로 $\sum X_i$는 충분통계량이고, $\bar X$는 그 일대일 변환이라 역시 충분하다. (조건부분포는 다항분포 $\text{Mult}(t;\frac1n,\dots,\frac1n){}$)

**Example 4.2 (베르누이)** $X_i\overset{\text{iid}}{\sim}\text{Ber}(\theta)$, $T=\sum X_i$ = 성공 횟수. $T=t$이면 성공 위치의 배열 $\binom nt$개가 모두 같은 확률:

$$\Pr_\theta(X=x\mid T=t)=\frac{\theta^t(1-\theta)^{n-t}}{\binom nt\theta^t(1-\theta)^{n-t}}=\frac1{\binom nt}\qquad\Big(\sum x_i=t\Big).$$

반대로 **$T=X_1$만 관측하면 충분하지 않다.** 예: $X_1=0$일 때 나머지 $(X_2,\dots,X_n)$의 조건부분포는 여전히 $\prod_{i\ge2}\theta^{x_i}(1-\theta)^{1-x_i}$라 $\theta$에 의존한다.

**분할로 보기 [p2]**
- $T$는 표본공간을 $\lbrace x:T(x)=t\rbrace$들로 나눈다. 충분성 = 각 칸 안의 조건부분포가 $\theta$와 무관.
- 칸을 **더 잘게 나눠도** 충분성은 유지된다. 예: 충분통계량이 $\sum X_i$일 때 자료를 임의로 둘로 나눠 각 부분의 합 $(\sum_{i\le m}X_i,\ \sum_{i > m}X_i)$로 분할해도 충분하다.
- 따라서 충분통계량은 **유일하지 않다.** 전체 자료 $X$ 자체도 (축약 없는) 충분통계량이다.

---

## 3. 분해정리

### 3.1 분해정리와 이산형 증명

📐 [ch4 p3 Thm 4.1]

**정리** $T(X)$가 $\theta$에 대한 충분통계량 ⟺ $\theta$와 무관한 함수 $h$와 함수 $g_\theta$가 있어

$$p_\theta(x)=h(x)\thinspace g_\theta\big(T(x)\big)\quad\text{(모든 }x,\theta\text{)} .$$

**핵심 한 수** (⇐) 조건부확률의 분모 합에서 $g_\theta(t)$가 **공통인자로 빠져** 약분된다. (⇒) $g_\theta(t)=\Pr_\theta(T=t)$, $h(x)=\Pr(X=x\mid T=T(x))$로 정의.

**증명 (이산형)**

(⇐) 분해가 성립한다고 하자. $T(x)=t$인 $x$에 대해

$$\Pr_\theta(X=x\mid T=t)=\frac{p_\theta(x_1,\dots,x_n,\ T=t)}{p_\theta(T=t)}=\frac{h(x)g_\theta(t)}{\sum_{z:T(z)=t}h(z)g_\theta(t)}=\frac{h(x)}{\sum_{z:T(z)=t}h(z)}$$

이므로 $\theta$에 의존하지 않는다. ($T(x)\ne t$이면 조건부확률은 0으로 역시 $\theta$와 무관)

(⇒) 충분하다고 하자. $g_\theta(t)=\Pr_\theta(T=t)$, $h(x)=\Pr_\theta(X=x\mid T=T(x))$로 두면 충분성 때문에 $h$는 $\theta$와 무관하다. 곱셈법칙으로

$$p_\theta(x)=\Pr_\theta(X=x,\ T=T(x))=\Pr_\theta(T=T(x))\thinspace\Pr(X=x\mid T=T(x))=g_\theta(T(x))\thinspace h(x).\qquad\blacksquare$$

**(보충) 연속형** 분해정리는 연속형에서도 그대로 쓴다 (증명은 측도론이 필요해 생략). 시험에서는 "분해정리에 의해"라고 쓰면 된다.

**자주 하는 실수**
- $h$에 $\theta$가 숨어 있음 (특히 support의 지시함수를 $h$에 넣는 실수 → 3.3)
- $g_\theta$가 $x$에 **$T(x)$를 통하지 않고** 의존함
- " $T$의 일대일 변환도 충분"은 되지만, $T$의 **일대일이 아닌** 함수는 충분하지 않을 수 있음

### 3.2 Ex 4.3 정규 표본

📐 [ch4 p3]

$X_i\overset{\text{iid}}{\sim}N(\mu,\sigma^2)$이면

$$p_{\mu,\sigma^2}(x)=(2\pi\sigma^2)^{-n/2}\exp\Big\lbrace-\frac1{2\sigma^2}\Big(\sum_ix_i^2-2\mu\sum_ix_i+n\mu^2\Big)\Big\rbrace$$

이라 $h(x)=1$로 두면 $(\sum X_i,\ \sum X_i^2)$가 $(\mu,\sigma^2)$에 대한 충분통계량이다.

**$\sigma^2$을 알 때:** 항등식 $\sum_i(x_i-\mu)^2=\sum_i(x_i-\bar x)^2+n(\bar x-\mu)^2$로

$$p_\mu(x)=\underbrace{(2\pi\sigma^2)^{-n/2}\exp\Big\lbrace-\frac{\sum(x_i-\bar x)^2}{2\sigma^2}\Big\rbrace}_ {h(x)}\ \underbrace{\exp\Big\lbrace-\frac{n(\bar x-\mu)^2}{2\sigma^2}\Big\rbrace}_ {g_\mu(\bar x)}$$

→ $\bar X$ 하나가 $\mu$에 대한 충분통계량. (항등식은 $x_i-\mu=(x_i-\bar x)+(\bar x-\mu)$를 제곱해 더하면 교차항 $2(\bar x-\mu)\sum(x_i-\bar x)=0$)

### 3.3 보충 support가 모수에 의존할 때

(보충) hw1 #8·#9의 핵심. [T22]

**틀** 각 관측의 밀도에 붙은 지시함수를 모두 곱한 뒤 **$\min$· $\max$ 하나의 지시함수로** 바꾼다.

$$\prod_{i=1}^nI(x_i\ge\theta)=I\big(\min_ix_i\ge\theta\big),\qquad\prod_{i=1}^nI(x_i\le\theta)=I\big(\max_ix_i\le\theta\big),\qquad\prod_{i=1}^nI(a \lt  x_i \lt  b)=I\big(a \lt  \min_ix_i\big)\thinspace I\big(\max_ix_i \lt  b\big).$$

(모든 $x_i$가 조건을 만족 ⟺ 가장 작은(큰) 것이 만족)

**예: $\text{Unif}(0,\theta)$**

$$p_\theta(x)=\prod_{i=1}^n\frac1\theta I(0\le x_i\le\theta)=\underbrace{I\big(\min_ix_i\ge0\big)}_ {h(x)}\ \underbrace{\theta^{-n}I\big(\max_ix_i\le\theta\big)}_ {g_\theta(x_{(n)})}$$

→ $X_{(n)}$이 충분통계량. ⚠ $I(\max x_i\le\theta)$는 $\theta$에 의존하므로 **반드시 $g_\theta$ 쪽**에 둔다. $I(\min x_i\ge0)$는 $\theta$와 무관하므로 $h$에 둔다.

---

## 4. 우도함수

💡 [ch4 p3–4]

- 관측값 $x$를 고정하고 결합밀도를 $\theta$의 함수로 본 것: $L(\theta;x)=p_\theta(x)$.
- 우도는 $\theta$에 대한 확률분포가 **아니다.** $\theta$와 무관한 양의 상수배는 우도의 상대적 비교를 바꾸지 않는다.
- $T$가 충분하면 분해정리로 $L(\theta;x)=h(x)g_\theta(T(x))\propto g_\theta(T(x))$. 그래서 **우도에만 근거하는 추론(MLE, 우도비 검정 등)은 $T$만 알면 원자료가 주는 것을 모두 재현한다.** 이것이 "data reduction"이라는 역할의 정확한 뜻이다.
- (보충) 결과: MLE는 (유일하면) 충분통계량의 함수다 → ch5.

---

## 5. 최소충분통계량

### 5.1 정의와 우도비 판정법

📐🧩 [ch4 p4 Definition, Thm 4.2]

- 충분통계량 중에는 축약이 덜 된 것이 있다. 예: 포아송 표본에서 $(\sum X_i,\ X_1)$도 충분하지만 $\sum X_i$만으로 충분하다.
- **정의** $T$가 **최소충분통계량** ⟺ $T$가 충분하고, **임의의** 충분통계량 $S$에 대해 어떤 함수 $a$가 있어 $T=a(S)$.
  - 즉 모든 충분통계량에서 $T$를 복원할 수 있다 = 모든 충분통계량이 반드시 담고 있는 가장 축약된 정보.
  - 최소충분통계량 자체는 일대일 변환까지만 유일하지만, 그것이 만드는 **표본공간의 분할은 유일**하다.

**Theorem 4.2 (우도비 판정법)** 두 표본점 $x,y$에 대해 $R_\theta(x,y)=\dfrac{p_\theta(y)}{p_\theta(x)}$ (정의되는 곳에서). 어떤 통계량 $T$가

$$R_\theta(x,y)\ \text{가}\ \theta\ \text{와 무관}\iff T(x)=T(y)$$

를 만족하면 $T$는 최소충분통계량이다.

- support가 $\theta$에 의존하면 비가 0/0이 될 수 있다. 그때는 "**모든 $\theta$에서 $p_\theta(y)=k(x,y)\thinspace p_\theta(x)$인 $\theta$와 무관한 $k(x,y) > 0$이 있다**"로 읽는다.

**핵심 한 수 (쓰는 법)** $p_\theta(y)/p_\theta(x)$를 계산해서 $\theta$가 들어간 부분이 **$T(y)-T(x)$ (또는 지시함수의 비교)** 꼴로만 남게 정리하고, "무관 ⟺ $T(x)=T(y)$"의 **양방향**을 모두 확인한다.

### 5.2 보충 증명

🧩 (보충 증명) 강의노트에 증명이 없다. 양의 확률(밀도)을 갖는 점에서 논의한다.

**(a) $T$는 충분하다.** 각 값 $t$마다 $T(x_t)=t$인 대표점 $x_t$를 하나씩 고정한다. 임의의 $x$에 대해 $T(x)=T(x_{T(x)})$이므로 가정에 의해 $p_\theta(x)/p_\theta(x_{T(x)})$는 $\theta$와 무관하다. 이를 $h(x)$라 하고 $g_\theta(t)=p_\theta(x_t)$로 두면

$$p_\theta(x)=p_\theta(x_{T(x)})\cdot\frac{p_\theta(x)}{p_\theta(x_{T(x)})}=g_\theta(T(x))\thinspace h(x)$$

이므로 분해정리로 $T$는 충분하다.

**(b) $T$는 최소다.** $S$를 임의의 충분통계량이라 하면 분해정리로 $p_\theta(x)=h'(x)g'_ \theta(S(x))$. $S(x)=S(y)$이면

$$\frac{p_\theta(y)}{p_\theta(x)}=\frac{h'(y)g'_ \theta(S(y))}{h'(x)g'_ \theta(S(x))}=\frac{h'(y)}{h'(x)}$$

로 $\theta$와 무관 → 가정의 (⟹) 방향으로 $T(x)=T(y)$. 즉 " $S(x)=S(y)\Rightarrow T(x)=T(y)$"이므로 $T$는 $S$의 함수다. $S$가 임의였으므로 $T$는 최소충분. ∎

### 5.3 보충 예: 정규, 균등, 지수족

**Example 4.4 (포아송)** [p4]

$$\frac{p_\theta(y)}{p_\theta(x)}=\theta^{\sum_iy_i-\sum_ix_i}\thinspace\frac{\prod_ix_i!}{\prod_iy_i!}$$

($e^{-n\theta}$는 약분). $\theta$와 무관 ⟺ 지수 $\sum y_i-\sum x_i=0$. 따라서 $\sum X_i$는 최소충분통계량이다.

**(보충) 정규 $N(\mu,\sigma^2)$** — 3.2의 식으로

$$\frac{p(y)}{p(x)}=\exp\Big\lbrace-\frac1{2\sigma^2}\Big(\sum y_i^2-\sum x_i^2\Big)+\frac\mu{\sigma^2}\Big(\sum y_i-\sum x_i\Big)\Big\rbrace.$$

모든 $(\mu,\sigma^2)$에서 무관 ⟺ 두 괄호가 모두 0 → $(\sum X_i,\sum X_i^2)$, 동치로 $(\bar X,S^2)$가 최소충분. (한 괄호라도 0이 아니면 $\mu$나 $\sigma^2$을 움직일 때 비가 변한다)

**(보충) $\text{Unif}(\theta,\theta+1)$** — 1차원 모수에 2차원 최소충분통계량

$$p_\theta(x)=\prod I(\theta \lt  x_i \lt  \theta+1)=I\big(x_{(n)}-1 \lt  \theta \lt  x_{(1)}\big).$$

$p_\theta(y)=k\thinspace p_\theta(x)$가 모든 $\theta$에서 성립하려면 두 지시함수가 같은 $\theta$ 구간에서 1이어야 한다 (한쪽만 1인 $\theta$가 있으면 $0=k\cdot1$ 또는 $1=k\cdot0$). 즉 $(x_{(1)},x_{(n)})=(y_{(1)},y_{(n)})$. 따라서 $(X_{(1)},X_{(n)})$이 최소충분.

**(보충) 지수족** $p_\alpha(x)=\prod e^{\alpha^\top t(x_i)-\psi(\alpha)}f_0(x_i)$이면

$$\frac{p_\alpha(y)}{p_\alpha(x)}=\exp\Big\lbrace\alpha^\top\Big(\sum t(y_i)-\sum t(x_i)\Big)\Big\rbrace\prod\frac{f_0(y_i)}{f_0(x_i)} .$$

자연모수공간 $A$가 열린 집합을 포함하면, $\alpha$에 대해 무관 ⟺ $\sum t(y_i)=\sum t(x_i)$. 따라서 $\sum t(X_i)$는 최소충분. (hw1 #5(e)의 $(\sum X_i,\sum X_iX_i^\top)$도 최소충분)

### 5.4 Remark 모형에 따라 축약의 정도가 다르다

💡 [ch4 p4]

위치모수 Cauchy 모형 $p_\theta(x)=\lbrace\pi[1+(x-\theta)^2]\rbrace^{-1}$에서는 일반적으로 **순서통계량 $(X_{(1)},\dots,X_{(n)})$이 최소충분**하다. 표본의 **순서는 버릴 수 있지만** 값들을 몇 개의 합으로 더 줄일 수는 없다. "모수의 차원이 작다"고 해서 저차원 충분통계량이 항상 있는 것은 아니다. (Cauchy는 지수족이 아니다 — ch2의 "고정 차원 충분통계량 ⟺ 지수족"과 맞물린다)

---

## 6. Summary

[ch4 p4]

1. 충분성: $T$를 안 뒤 원자료의 조건부분포가 모수와 무관한 성질.
2. 실제 계산에는 분해정리 $p_\theta(x)=h(x)g_\theta(T(x))$를 쓴다.
3. 최소충분통계량: 모든 충분통계량이 반드시 담고 있는 가장 축약된 정보.

---

## hw1 풀이

이 장에 속하는 문제: #8, #9. 둘 다 **support가 모수에 의존**하는 경우라 지시함수 처리가 핵심이다.

### hw1 8 최솟값이 충분통계량

📝 [hw1 p5] · 내 풀이 [CamScanner p9]

**문제 요지** $X_i$ 독립, $f_{X_i}(x\mid\theta)=e^{\theta-x}$ ($x\ge\theta$), $0$ ($x < \theta$). $T=\min_iX_i$가 $\theta$에 대한 충분통계량임을 보여라.

**전략** 분해정리. 지시함수를 붙여 결합밀도를 쓰고, $\prod I(x_i\ge\theta)=I(\min x_i\ge\theta)$로 바꾼다 [3.3](#33-보충-support가-모수에-의존할-때).

**모범 답안** 지시함수를 쓰면 $f(x_i\mid\theta)=e^{\theta-x_i}I(x_i\ge\theta)$. 독립이므로

$$f(x_1,\dots,x_n\mid\theta)=\prod_{i=1}^ne^{\theta-x_i}I(x_i\ge\theta)=\underbrace{e^{-\sum_ix_i}}_ {h(x)}\ \underbrace{e^{n\theta}\thinspace I\big(\min_ix_i\ge\theta\big)}_ {g_\theta(T(x))} .$$

여기서 $\prod_iI(x_i\ge\theta)=I(\min_ix_i\ge\theta)$ (모든 $x_i\ge\theta$ ⟺ 가장 작은 값 $\ge\theta$)를 썼다. $h$는 $\theta$와 무관하고, $g_\theta$는 자료에 $T(x)=\min_ix_i$를 통해서만 의존한다. 분해정리 (Thm 4.1)로 $T=\min_iX_i$는 $\theta$에 대한 충분통계량이다. ∎

**(보충) 최소충분이기도 하다.** $p_\theta(y)=k\thinspace p_\theta(x)$가 모든 $\theta$에서 성립하려면 $e^{-\sum y_i}I(\min y_i\ge\theta)=k\thinspace e^{-\sum x_i}I(\min x_i\ge\theta)$ — 지시함수가 1인 $\theta$ 범위 $(-\infty,\min]$가 같아야 하므로 $\min x_i=\min y_i$. 역도 성립. 우도비 판정법으로 최소충분.

**채점 포인트**
- **지시함수를 명시**해서 결합밀도를 쓰기 (support 조건을 빼먹으면 $h=e^{-\sum x_i}$, $g=e^{n\theta}$로 "상수가 충분"이라는 틀린 결론이 된다)
- $\prod I(x_i\ge\theta)=I(\min x_i\ge\theta)$의 근거 한 줄
- $h$가 $\theta$와 무관, $g$는 $T$를 통해서만 → 분해정리 인용

**내 풀이 점검**
- ✅ 결합밀도, $h(x)=e^{-\sum x_i}$, $g_\theta(T)=e^{n\theta}I_{\theta\le T}$, 분해정리 인용 모두 맞다.
- ⚠ $\prod I(x_i\ge\theta)=I(\theta\le\min x_i)$로 바꾸는 이유를 한 줄 쓰면 완전하다.

**연결** 이 밀도는 support가 $\theta$에 의존해서 **지수족이 아니다** (ch2 [10](ch2%20모수모형과%20지수족.md#10-보충-지수족인지-판정하기)). 그래서 충분통계량이 합이 아니라 최솟값이다. MLE도 $\hat\theta=X_{(1)}$이다 (우도 $e^{n\theta}$가 $\theta\le\min x_i$에서 증가) → ch5.

### hw1 9 2차원 충분통계량

📝 [hw1 p5] · 내 풀이 [CamScanner p9]

**문제 요지** $X_i$ 독립, $f_i(x_i\mid\theta)=\dfrac1{2i\theta}$ ($-i(\theta-1) < x_i < i(\theta+1){}$), $\theta > 0$. $\theta$에 대한 2차원 충분통계량을 찾아라.

**전략** 각 $x_i$의 범위를 **$i$로 나눠** 모든 $i$에 공통인 조건으로 바꾼다. 그러면 지시함수의 곱이 $\min$· $\max$ 하나로 묶인다.

**모범 답안**
1. 범위를 $i$로 나누면: $-i(\theta-1) < x_i < i(\theta+1)\iff1-\theta < \dfrac{x_i}i < 1+\theta$.
2. 독립이므로

$$f(x\mid\theta)=\prod_{i=1}^n\frac1{2i\theta}I\Big(1-\theta \lt  \frac{x_i}i \lt  1+\theta\Big)=\underbrace{\Big(\prod_{i=1}^n\frac1{2i}\Big)}_ {h(x)}\ \underbrace{\theta^{-n}\thinspace I\Big(1-\theta \lt  \min_i\frac{x_i}i\Big)\thinspace I\Big(\max_i\frac{x_i}i \lt  1+\theta\Big)}_ {g_\theta(T(x))}.$$

3. 분해정리로 $T(X)=\Big(\min_i\dfrac{X_i}i,\ \max_i\dfrac{X_i}i\Big)$가 $\theta$에 대한 2차원 충분통계량이다. ∎

**(보충) 사실 1차원으로 충분하다 — 최소충분통계량.** 조건 $1-\theta < x_i/i < 1+\theta$는 $\lvert x_i/i-1\rvert < \theta$와 같다. 따라서

$$f(x\mid\theta)=\Big(\prod_i\frac1{2i}\Big)\theta^{-n}I\big(T^\ast(x) \lt  \theta\big),\qquad T^\ast(x)=\max_i\Big\lvert\frac{x_i}i-1\Big\rvert=\max\Big(1-\min_i\frac{x_i}i,\ \max_i\frac{x_i}i-1\Big).$$

- 분해정리로 $T^\ast$가 충분하다.
- 우도비 판정법: $\theta^{-n}I(T^\ast(y) < \theta)=k\thinspace\theta^{-n}I(T^\ast(x) < \theta)$가 모든 $\theta > 0$에서 성립 ⟺ $k=1$, $T^\ast(x)=T^\ast(y)$. 따라서 **$T^\ast$가 최소충분**이다.
- $T^\ast$는 $T$의 함수지만 $T$는 $T^\ast$의 함수가 아니다 → hw가 요구한 2차원 $T$는 충분하지만 최소는 아니다. (시험에서 "최소충분통계량을 구하라"로 바뀌면 답은 $T^\ast$)

**채점 포인트**
- $i$로 나눠 공통 조건 $1-\theta < x_i/i < 1+\theta$로 바꾸기
- 두 지시함수를 $\min$과 $\max$로 묶기
- $h=\prod\frac1{2i}$는 $\theta$와 무관함을 확인 ($\frac1\theta$ 인자는 $g$ 쪽)

**내 풀이 점검**
- ✅ $1-\theta < x_i/i < 1+\theta$ 변환, $f=\big(\prod\frac1{2i}\big)\theta^{-n}I(1-\theta < \min\frac{x_i}i,\ \max\frac{x_i}i < 1+\theta)$, $T=(\min\frac{X_i}i,\max\frac{X_i}i)$ 모두 맞다.
- ➕ 더 나아가면 위의 1차원 최소충분통계량 $T^\ast$까지 볼 수 있다.

---

## 변형 문제

난이도: `●` hw 문제의 분포만 바꿈 · `●●` 도구 두세 개 조합 · `●●●` 장을 넘나듦. 힌트부터 열고, 스스로 써 본 뒤 풀이를 연다.

### V1 균등분포의 최댓값

`●` $X_i\overset{\text{iid}}{\sim}\text{Unif}(0,\theta)$. $X_{(n)}$이 충분통계량이고 최소충분임을 보여라.

<details><summary>풀이</summary>

충분: 본문 [3.3](#33-보충-support가-모수에-의존할-때). 최소: $p_\theta(x)=\theta^{-n}I(x_{(n)}\le\theta)I(x_{(1)}\ge0)$. 표본점을 support 안($x_{(1)},y_{(1)}\ge0$)에서 비교하면 $p_\theta(y)=k\thinspace p_\theta(x)$ ∀θ ⟺ $I(y_{(n)}\le\theta)=k\thinspace I(x_{(n)}\le\theta)$ ∀θ ⟺ $k=1$, $x_{(n)}=y_{(n)}$.

</details>

### V2 길이 1인 균등분포

`●●` $X_i\overset{\text{iid}}{\sim}\text{Unif}(\theta,\theta+1)$. (a) 충분통계량을 구하라. (b) 그것이 최소충분임을 보여라. (c) 모수는 1차원인데 최소충분통계량은 2차원이다. $X_{(n)}$ 하나만으로는 왜 충분하지 않은가?

<details><summary>전략 힌트</summary>

$\prod I(\theta < x_i < \theta+1)=I(x_{(n)}-1 < \theta < x_{(1)})$ — $\theta$가 들어갈 수 있는 구간.

</details>
<details><summary>풀이</summary>

(a) $p_\theta(x)=I(x_{(n)}-1 < \theta < x_{(1)})$ → 분해정리로 $(X_{(1)},X_{(n)})$.

(b) 본문 [5.3](#53-보충-예-정규-균등-지수족): 두 지시함수가 모든 $\theta$에서 비례하려면 $\theta$ 구간 $(x_{(n)}-1,x_{(1)})$이 같아야 한다.

(c) $X_{(n)}=m$을 알아도 $\theta$의 가능한 범위의 위 끝 $x_{(1)}$은 모른다. 예: $n=2$에서 $X_{(n)}=m$이 주어졌을 때 $X_{(1)}$의 조건부분포는 구간 $(\theta,m)$ 위에 있어 $\theta$에 의존한다.

</details>

### V3 정규분포의 최소충분통계량

`●●` $X_i\overset{\text{iid}}{\sim}N(\mu,\sigma^2)$, 둘 다 미지. 우도비 판정법으로 $(\bar X,S^2)$가 최소충분임을 보여라.

<details><summary>풀이</summary>

본문 [5.3](#53-보충-예-정규-균등-지수족)의 비: 모든 $(\mu,\sigma^2)$에서 무관 ⟺ $\sum y_i=\sum x_i$, $\sum y_i^2=\sum x_i^2$ ⟺ $(\bar y,s_y^2)=(\bar x,s_x^2)$ ($\sum x_i^2=(n-1)s^2+n\bar x^2$라 일대일). (⟸는 비가 1이 되어 명백. ⟹: $\sum y_i\ne\sum x_i$이면 $\sigma^2$을 고정하고 $\mu$를 바꿀 때 비가 변하고, $\sum y_i=\sum x_i$인데 $\sum y_i^2\ne\sum x_i^2$이면 $\sigma^2$을 바꿀 때 비가 변한다.)

</details>

### V4 위치가 i배인 이동 지수분포

`●●` $X_i$ 독립, $f_i(x\mid\theta)=e^{i\theta-x}I(x\ge i\theta)$. $T=\min_i(X_i/i)$가 충분통계량임을 보여라. (hw1 #8의 일반화 — 모수가 관측마다 $i$배)

<details><summary>전략 힌트</summary>

hw1 #9처럼 조건을 $i$로 나눠 공통 조건으로: $x_i\ge i\theta\iff x_i/i\ge\theta$.

</details>
<details><summary>풀이</summary>

$$f(x\mid\theta)=\prod_ie^{i\theta-x_i}I\Big(\frac{x_i}i\ge\theta\Big)=\underbrace{e^{-\sum x_i}}_ {h}\ \underbrace{e^{\theta\sum_i i}\thinspace I\Big(\min_i\frac{x_i}i\ge\theta\Big)}_ {g_\theta(T)},\qquad\sum_{i=1}^ni=\frac{n(n+1)}2 .$$

분해정리로 $T=\min_i(X_i/i)$.

</details>

### V5 곱과 합

`●` (a) $X_i\overset{\text{iid}}{\sim}\text{Beta}(\theta,1)$, 즉 $f(x)=\theta x^{\theta-1}$ ($0 < x < 1$). 충분통계량은? (b) $X_i\overset{\text{iid}}{\sim}\text{Gam}(\nu,\sigma)$, 둘 다 미지. 충분통계량은?

<details><summary>풀이</summary>

(a) $\prod\theta x_i^{\theta-1}=\theta^n\big(\prod x_i\big)^{\theta-1}$ ($h=1$ on $(0,1)^n$) → $\prod X_i$ (동치로 $\sum\log X_i$).

(b) $\prod\frac{x_i^{\nu-1}e^{-x_i/\sigma}}{\sigma^\nu\Gamma(\nu)}=\big(\sigma^\nu\Gamma(\nu)\big)^{-n}\big(\prod x_i\big)^{\nu-1}e^{-\sum x_i/\sigma}$ → $(\prod X_i,\ \sum X_i)$ (ch2 감마의 $t=(x,\log x)$와 같은 결론).

</details>

### V6 충분하지 않음을 보이기

`●●` $X_1,X_2\overset{\text{iid}}{\sim}\text{Poi}(\theta)$. (a) $T=X_1+X_2$는 충분하다. (b) $U=X_1+2X_2$는 충분하지 않음을 보여라.

<details><summary>전략 힌트</summary>

(b) $U$의 값 하나를 정하고 그 값을 주는 $(x_1,x_2)$들을 나열 → 조건부확률 하나가 $\theta$에 의존함을 보이면 끝.

</details>
<details><summary>풀이</summary>

(a) Ex 4.1 ($n=2$).

(b) $U=2$를 주는 점은 $(2,0)$과 $(0,1)$뿐이다.

$$\Pr(X=(2,0)\mid U=2)=\frac{e^{-2\theta}\theta^2/2}{e^{-2\theta}\theta^2/2+e^{-2\theta}\theta}=\frac{\theta}{\theta+2},$$

$\theta$에 의존한다 → 충분하지 않다. (정의를 반박할 때는 **반례 하나**면 충분하다)

</details>

### V7 연속형에서 정의로 충분성 보이기

`●●` $X_1,X_2\overset{\text{iid}}{\sim}\text{Exp}$ (rate $\lambda$). $T=X_1+X_2$가 주어졌을 때 $X_1$의 조건부분포를 구해 $T$가 충분함을 정의로 보여라.

<details><summary>풀이</summary>

$(X_1,T)$의 결합밀도: 변환 $(x_1,x_2)\mapsto(x_1,t=x_1+x_2)$의 야코비안이 1이므로 $f(x_1,t)=\lambda^2e^{-\lambda t}$ ($0 < x_1 < t$). $T\sim\text{Gam}(2,\text{rate }\lambda)$, $f_T(t)=\lambda^2te^{-\lambda t}$. 따라서 $f(x_1\mid t)=1/t$ ($0 < x_1 < t$), 즉 $X_1\mid T=t\sim\text{Unif}(0,t)$ — $\lambda$와 무관. ($X_2=T-X_1$도 정해지므로 $(X_1,X_2)\mid T$의 분포가 $\lambda$와 무관)

</details>

### V8 curved 지수족의 최소충분통계량

`●●●` $X_i\overset{\text{iid}}{\sim}N(\theta,\theta^2)$, $\theta > 0$ (ch2 V11). $(\sum X_i,\sum X_i^2)$가 최소충분임을 보여라.

<details><summary>전략 힌트</summary>

비의 지수가 $\frac1\theta a-\frac1{2\theta^2}b$ 꼴 ($a,b$는 합의 차이). 이것이 모든 $\theta > 0$에서 상수이려면? — $1/\theta$와 $1/\theta^2$는 일차독립인 함수.

</details>
<details><summary>풀이</summary>

$a=\sum y_i-\sum x_i$, $b=\sum y_i^2-\sum x_i^2$라 하면 $\frac{p_\theta(y)}{p_\theta(x)}=\exp\big(\frac a\theta-\frac b{2\theta^2}\big)$. $\theta$와 무관하면 $u=1/\theta$에 대해 $au-\frac b2u^2$가 모든 $u > 0$에서 상수 → 다항식이므로 $a=b=0$. 역은 명백. 따라서 2차원 $(\sum X_i,\sum X_i^2)$가 최소충분 (1차원 모수인데도).

</details>

### V9 파레토와 대칭 균등

`●` (a) $f(x\mid\theta)=\theta x^{-2}I(x\ge\theta)$ ($\theta > 0$). (b) $\text{Unif}(-\theta,\theta)$. 각각 충분통계량을 구하라.

<details><summary>풀이</summary>

(a) $\theta^n\big(\prod x_i^{-2}\big)I(\min x_i\ge\theta)$ → $X_{(1)}$.

(b) $(2\theta)^{-n}\prod I(\lvert x_i\rvert\le\theta)=(2\theta)^{-n}I(\max_i\lvert x_i\rvert\le\theta)$ → $\max_i\lvert X_i\rvert$.

</details>

### V10 두 배 구간의 균등분포

`●●●` $X_i\overset{\text{iid}}{\sim}\text{Unif}(\theta,2\theta)$, $\theta > 0$. 최소충분통계량을 구하라.

<details><summary>전략 힌트</summary>

$\theta < x_i < 2\theta\iff x_i/2 < \theta < x_i$ → 모든 $i$에 대해 모으면 $\theta$의 구간.

</details>
<details><summary>풀이</summary>

$p_\theta(x)=\theta^{-n}I\big(\tfrac{x_{(n)}}2 < \theta < x_{(1)}\big)$. 분해정리로 $(X_{(1)},X_{(n)})$이 충분. $p_\theta(y)=k\thinspace p_\theta(x)$ ∀θ이려면 $\theta^{-n}$은 약분되고 두 지시함수의 $\theta$ 구간이 같아야 한다 → $(x_{(1)},x_{(n)})=(y_{(1)},y_{(n)})$. 최소충분통계량은 2차원 $(X_{(1)},X_{(n)})$. (V2와 같은 구조)

</details>

---

## 셀프 테스트

### 백지 증명 체크리스트

| ☐ | 증명·유도 | 뼈대 | 위치 |
| --- | --- | --- | --- |
| ☐ | Ex 4.1 포아송 | 조건부확률 = $\frac{t!}{n^t\prod x_i!}$ | [2](#2-충분통계량의-정의) |
| ☐ | Ex 4.2 베르누이 + $X_1$ 반례 | $1/\binom nt$ · $X_1=0$일 때 나머지가 $\theta$에 의존 | [2](#2-충분통계량의-정의) |
| ☐ | **분해정리 (⇐)** | 분모 합에서 $g_\theta(t)$가 빠져 약분 | [3.1](#31-분해정리와-이산형-증명) |
| ☐ | **분해정리 (⇒)** | $g=\Pr_\theta(T=t)$, $h=\Pr(X=x\mid T=T(x))$, 곱셈법칙 | [3.1](#31-분해정리와-이산형-증명) |
| ☐ | Ex 4.3 정규 | 전개 → $(\sum x,\sum x^2)$ · 항등식 → $\bar X$ | [3.2](#32-ex-43-정규-표본) |
| ☐ | **우도비 판정법 (보충)** | (a) 대표점 $x_t$로 $h,g$ 만들기 (b) $S(x)=S(y)$ ⟹ 비 = $h'(y)/h'(x)$ ⟹ $T(x)=T(y)$ | [5.2](#52-보충-증명) |
| ☐ | Ex 4.4 포아송 최소 | 비 $=\theta^{\sum y-\sum x}\cdot$ 상수 | [5.3](#53-보충-예-정규-균등-지수족) |
| ☐ | **hw1 #8** | 지시함수 → $\prod I=I(\min\ge\theta)$ → $h=e^{-\sum x}$, $g=e^{n\theta}I(\cdot)$ | [hw1 #8](#hw1-8-최솟값이-충분통계량) |
| ☐ | **hw1 #9** | $i$로 나누기 → $\min,\max$ → 2차원 · (보충) $\max\lvert x_i/i-1\rvert$ | [hw1 #9](#hw1-9-2차원-충분통계량) |

### 짧은 문답

<details><summary>1. 충분통계량의 정의는?</summary>

$T=t$가 주어졌을 때 $X$의 조건부분포가 $\theta$에 의존하지 않는다.

</details>
<details><summary>2. 분해정리에서 support의 지시함수는 h와 g 중 어디에 두나?</summary>

$\theta$에 의존하면 $g_\theta$, 의존하지 않으면 $h$.

</details>
<details><summary>3. 충분통계량이 유일하지 않은 이유 세 가지는?</summary>

일대일 변환, 더 잘게 나눈 분할, 전체 자료 $X$도 충분.

</details>
<details><summary>4. 최소충분통계량의 정의는?</summary>

충분하고, 임의의 충분통계량 $S$의 함수로 쓸 수 있는 통계량.

</details>
<details><summary>5. Unif(θ, θ+1)의 최소충분통계량은?</summary>

$(X_{(1)},X_{(n)})$ — 모수는 1차원이지만 2차원.

</details>
<details><summary>6. hw1 #9의 최소충분통계량은?</summary>

$\max_i\lvert X_i/i-1\rvert$ (1차원).

</details>
<details><summary>7. Cauchy 위치모형의 최소충분통계량은?</summary>

순서통계량 $(X_{(1)},\dots,X_{(n)})$.

</details>

---

## 강의노트 확인

강의노트 4쪽을 텍스트로 전부 읽었다 (수식이 유니코드로 잘 추출됨).

| 쪽 | 강의노트 원문 | 노트에 반영한 것 | 근거 | 영향 |
| --- | --- | --- | --- | --- |
| p3 Thm 4.1 증명 | $p_\theta(x_1,\dots,x_n.T=t)$ | ✏️ $p_\theta(x_1,\dots,x_n,\ T=t)$ | 마침표 → 쉼표 | · |
| p2 Ex 4.1 | 모형($X_i\overset{\text{iid}}{\sim}\text{Poi}(\theta){}$)을 밝히지 않고 " $T\sim\text{Poi}(n\theta)$이므로"로 시작 | 모형을 명시 | 문맥 | · |
| p4 Thm 4.2 | 증명 없음, 0/0 처리 언급 없음 | 보충 증명 + " $p_\theta(y)=k\thinspace p_\theta(x)$ ∀θ"로 읽는 법 | support가 모수에 의존하는 예 (hw1 #8·#9) | ★ |
