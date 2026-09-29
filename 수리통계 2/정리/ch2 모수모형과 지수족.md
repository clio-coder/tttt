# ch2 모수모형과 지수족 (Parametric Models and Exponential Families)

> **원본:** `lecture_ch2_parametric_models_and_exponential_families.pdf` (11쪽, 9/8 수업)
> **관련 숙제:** hw1 #3 (이항분포의 지수족 표현) · #5 (2변량 정규의 지수족 표현) · 연결: ch4 충분통계량, ch5 MLE
> **범례:** 📝 hw1에 나옴 · 🧩 강의노트에 증명 없음 → 보충 증명 · 📐 강의노트에 증명 있음 · 🔢 계산 · 💡 개념·해석 · (보충) 강의노트에 없는 내용 · `[T…]` = [도구상자](00%20문제풀이%20도구상자.md) 카드 번호

**기호 대응표 — 강의노트와 hw가 다르다**

| 뜻 | 강의노트 | hw1 | 주의 |
| --- | --- | --- | --- |
| 자연모수 (natural parameter) | $\alpha$ | $\eta$ (#5는 $\eta,\Lambda$) | |
| 충분통계량 | $t(x)$, $y=t(x)$ | $T(x)$ | |
| 기준밀도 | $f_0(x)$ | $h(x)$ | |
| normalizing function = log-partition = 누적생성함수 | $\psi(\alpha)$ | $A(\eta)$ | |
| 자연모수공간 | $A=\lbrace\alpha:e^{\psi(\alpha)} < \infty\rbrace$ | natural parameter space | ⚠ 강의의 $A$는 **집합**, hw의 $A(\eta)$는 **함수** |

두 꼴은 같은 것이다: $f_\alpha(x)=\exp\lbrace\alpha^\top t(x)-\psi(\alpha)\rbrace f_0(x)$ ⟺ $f_\eta(x)=h(x)\exp\lbrace\eta^\top T(x)-A(\eta)\rbrace$.

---

## 0. 한눈에

### 0.1 범위

| 쪽 | 절 | 내용 | 증명 | hw1 |
| --- | --- | --- | --- | --- |
| p1 | 도입, 2.1 | 모수모형, 5개 일변량 분포, $N(\mu,\sigma^2)=\mu+\sigma N(0,1)$, 감마로 베타 만들기, 이항 ≈ 포아송 | – | – |
| p2–3 | 표 2.1, 그림 2.1–2.2 | 밀도, 표본공간, 모수공간, 평균, 분산 | – | – |
| p3–4 | 2.2 | 지수족의 역할 (최대 엔트로피, GLM, 변분추론, 켤레사전분포) | – | – |
| p4 | Definition | 포아송 기울이기 (2.1)–(2.4), $p$-차원 지수족 (2.5)–(2.6) | – | #3, #5 |
| p5 | | 자연모수공간의 볼록성, 감마 예 (2.7)–(2.8), iid 표본 (2.9) | 📐 | #5(e) |
| p6 | Thm 2.1 | $\dot\psi=\mathbb E(y)$, $\ddot\psi=\mathop{\mathrm{Var}}\nolimits(y)$, 다차원 (2.11)–(2.12) | 📐 | – |
| p6–7 | 2.2.1 | GLM, 로지스틱 = 교차엔트로피, softmax, $\psi$의 볼록성, Fisher 정보 | 🧩 | – |
| p7 | 2.2.2 | 베타–베르누이 켤레, LDA와 변분추론 | – | – |
| p8–9 | 2.2.3 | Lindsey 방법 (GFR 자료), 네트워크 모형 | – | – |
| p9–10 | | 최대 엔트로피 ⟹ 지수족 (라그랑주) | 📐 | – |

### 0.2 시험 포인트 (출제 가능성 순)

**★★★ 매우 유력**

1. 📝 **주어진 분포를 지수족 꼴로 쓰고 자연모수·자연모수공간·log-partition· $h$· $T$를 명시** (hw1 #3, #5) → [레시피 3.2](#32-지수족-꼴로-쓰는-4단계-레시피)
2. 📐 **Theorem 2.1** $\dot\psi(\alpha)=\mathbb E_\alpha(y)$, $\ddot\psi(\alpha)=\mathop{\mathrm{Var}}\nolimits_ \alpha(y)$ 증명과 다차원 판 (그래디언트 = 평균, 헤시안 = 공분산 ⟹ $\psi$ 볼록) → [5](#5-누적생성함수와-theorem-21)
3. 📐 **자연모수공간이 볼록집합**임을 증명 (강의노트: 지수함수의 볼록성) + (보충) Hölder로 $\psi$의 볼록성까지 → [2.3](#23-자연모수공간은-볼록집합)
4. 📝 **iid 표본의 결합밀도**가 $e^{n(\alpha^\top\bar y-\psi(\alpha))}$ 꼴 → $\sum t(x_i)$가 충분통계량 (hw1 #5(e), ch4 분해정리) → [4](#4-iid-표본과-충분통계량)

**★★ 유력**

5. 📐 포아송 기울이기 (2.1)–(2.4): $\alpha=\log(\mu/\mu_0)$, $\psi(\alpha)=\mu_0(e^\alpha-1)$, $e^{\psi(\alpha)}=\sum_xe^{\alpha x}f_{\mu_0}(x)$ → [2.1](#21-포아송-분포를-기울여서-만들기)
6. 🔢 감마를 2모수 지수족으로: $\alpha=(-1/\sigma,\ \nu-1)$, $t=(x,\log x)$, $\psi$, $A=\lbrace\alpha_1 < 0,\ \alpha_2 > -1\rbrace$ → [3.1](#31-감마분포)
7. 📐 최대 엔트로피 ⟹ 지수족 (라그랑주 승수) → [9](#9-최대-엔트로피와-지수족)
8. 💡 GLM의 canonical link, 로지스틱 음의 로그우도 = 이진 교차엔트로피, softmax, 베타–베르누이 사후분포 $\text{Be}(a+s,\thinspace b+n-s)$ → [6](#6-머신러닝-응용-glm), [7](#7-켤레사전분포와-변분추론)
9. 🔢 표 2.1의 평균·분산, $\chi^2_n=\text{Gam}(n/2,2)$, 감마 → 베타, $\text{Bi}(n,\pi)\approx\text{Poi}(n\pi)$ → [1](#1-모수모형과-일변량-분포)

**★ 가능성 있음**

10. (보충) 지수족이 **아닌** 것 판정 (support가 모수에 의존: $\text{Unif}(0,\theta)$, hw1 #8의 밀도), curved 지수족 $N(\theta,\theta^2)$ → [10](#10-보충-지수족인지-판정하기)
11. (보충) $t$의 MGF $=e^{\psi(\alpha+s)-\psi(\alpha)}$, 두 분포 사이의 KL, MLE의 $\dot\psi(\hat\alpha)=\bar t$ (ch5 대비) → [5.3](#53-보충-theorem-21에서-나오는-것들)
12. 💡 Lindsey 방법 3단계, 네트워크 모형, LDA → [8](#8-lindsey-방법으로-밀도-추정)

### 0.3 이 장의 도구

| 도구 | 쓰는 곳 |
| --- | --- |
| [T11] 지수족 레시피: $\log f$ 전개 → 섞인 항 = $\eta^\top T$ → $A$를 $\eta$로 → 적분 가능성으로 자연모수공간 | hw1 #3, #5 |
| [T12] 이차형식: 전개, $x^\top\Lambda x=\mathop{\mathrm{tr}}\nolimits(\Lambda xx^\top)$ | hw1 #5 |
| [T13] 정규화 항등식 $e^{\psi(\alpha)}=\int e^{\alpha^\top t}f_0$를 미분 | Theorem 2.1 |
| [T4] 볼록성 (지수함수, Hölder) | 자연모수공간, $\psi$의 볼록성 |
| [T22] 분해정리 + 지시함수 (ch4) | iid 표본의 충분통계량 |

---

## 1. 모수모형과 일변량 분포

💡🔢 [ch2 p1–3]

- **분포족** $\mathcal F=\lbrace f_\mu(x);\ x\in\mathcal X,\ \mu\in\Omega\rbrace$. 모수 $\mu$의 차원은 보통 5–10, 커도 20을 넘지 않는다고 본다. 반대 개념은 "distribution-free"보다 **비모수(nonparametric)**라는 말을 많이 쓴다.
- 모수모형을 선호하는 이유는 **수학적 접근 가능성**이다. 컴퓨팅이 발달해 더 현실적인 모형을 다룰 수 있게 됐지만, 모수모형은 여전히 중요하다.

**표 2.1** (자유도 $n$인 $\chi^2$ = $\text{Gam}(n/2,2){}$)

| 이름 | 밀도 | $\mathcal X$ | $\Omega$ | 평균 | 분산 |
| --- | --- | --- | --- | --- | --- |
| $N(\mu,\sigma^2)$ | $\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac12(\frac{x-\mu}\sigma)^2}$ | $\mathbb R$ | $\mu\in\mathbb R,\ \sigma^2 > 0$ | $\mu$ | $\sigma^2$ |
| $\text{Poi}(\mu)$ | $e^{-\mu}\mu^x/x!$ | $\lbrace0,1,\dots\rbrace$ | $\mu > 0$ | $\mu$ | $\mu$ |
| $\text{Bi}(n,\pi)$ | $\frac{n!}{x!(n-x)!}\pi^x(1-\pi)^{n-x}$ | $\lbrace0,\dots,n\rbrace$ | $0 < \pi < 1$ | $n\pi$ | $n\pi(1-\pi)$ |
| $\text{Gam}(\nu,\sigma)$ | $\frac{x^{\nu-1}e^{-x/\sigma}}{\sigma^\nu\Gamma(\nu)}$ | $x > 0$ | $\nu > 0,\ \sigma > 0$ | $\sigma\nu$ | $\sigma^2\nu$ |
| $\text{Be}(\nu_1,\nu_2)$ | $\frac{\Gamma(\nu_1+\nu_2)}{\Gamma(\nu_1)\Gamma(\nu_2)}x^{\nu_1-1}(1-x)^{\nu_2-1}$ | $[0,1]$ | $\nu_1,\nu_2 > 0$ | $\frac{\nu_1}{\nu_1+\nu_2}$ | $\frac{\nu_1\nu_2}{(\nu_1+\nu_2)^2(\nu_1+\nu_2+1)}$ |

⚠ 감마는 **척도(scale)** 모수 $\sigma$로 쓴다 (평균 $\sigma\nu$). 비율(rate) 모수로 쓰는 교재와 헷갈리지 않는다.

**관계** [p1]
- $N(\mu,\sigma^2)\overset{d}{=}\mu+\sigma N(0,1)$
- $G_j\sim\text{Gam}(\nu_j,\sigma)$ 독립 (척도 같음)이면 $\dfrac{G_1}{G_1+G_2}\sim\text{Be}(\nu_1,\nu_2)$
- $n$이 크고 $\pi$가 작으면 $\text{Bi}(n,\pi)\approx\text{Poi}(n\pi)$. 그림 2.1: $\text{Bi}(30,0.2)$와 $\text{Poi}(6)$이 거의 겹친다.
- 베타는 두 모수로 $[0,1]$ 위의 다양한 모양을 만든다 (그림 2.2).

---

## 2. 지수족의 정의

### 2.1 포아송 분포를 기울여서 만들기

📐 [ch2 p4 (2.1)–(2.4)]

평균이 $\mu,\mu_0$인 포아송 pmf의 비는

$$\frac{f_\mu(x)}{f_{\mu_0}(x)}=e^{-(\mu-\mu_0)}\Big(\frac{\mu}{\mu_0}\Big)^x\quad(2.1)\qquad\Longrightarrow\qquad f_\mu(x)=e^{\alpha x-\psi(\alpha)}f_{\mu_0}(x)\quad(2.2),$$

$$\alpha=\log(\mu/\mu_0),\qquad\psi(\alpha)=\mu_0(e^\alpha-1)\quad(2.3).$$

**검산:** $e^{\alpha x-\psi(\alpha)}=(\mu/\mu_0)^xe^{-\mu_0(\mu/\mu_0-1)}=(\mu/\mu_0)^xe^{-(\mu-\mu_0)}$ ✓

**만드는 순서**
1. 아무 포아송 $f_{\mu_0}$에서 시작한다.
2. $\alpha=\log(\mu/\mu_0)$로 두고 $\tilde f_\mu(x)=e^{\alpha x}f_{\mu_0}(x)$로 **기울인다(tilt)**.
3. $e^{\psi(\alpha)}$로 나눠 **표준화**한다. 정규화 상수는

$$e^{\psi(\alpha)}=\sum_{x=0}^\infty e^{\alpha x}f_{\mu_0}(x)\qquad(2.4).$$

예 (그림 2.3): $f_6$은 $f_{12}$를 $\alpha=\log(6/12)=-0.693$만큼 기울여 얻는다. 포아송은 $x$와 $\alpha$가 모두 1차원 → **one-parameter exponential family**.

(보충) (2.4)를 직접 계산하면 $\sum e^{\alpha x}e^{-\mu_0}\mu_0^x/x!=e^{-\mu_0}e^{\mu_0e^\alpha}$ → $\psi(\alpha)=\mu_0(e^\alpha-1)$ ✓. 즉 $\psi$는 $f_{\mu_0}$ 아래에서 $X$의 누적생성함수 $\log\mathbb E_{\mu_0}e^{\alpha X}$다.

### 2.2 p차원 지수족

📐 [ch2 p4 (2.5)–(2.6)]

**정의** 표본공간(밀도의 support)이 **모수에 의존하지 않고**, 밀도가

$$f_\alpha(x)=\exp\lbrace\alpha^\top t(x)-\psi(\alpha)\rbrace f_0(x),\qquad\alpha\in A\subset\mathbb R^p\qquad(2.5)$$

꼴이면 $p$-차원 지수족이다.
- $\alpha$: **자연모수** ($p$차원), $t(x)=(t_1(x),\dots,t_p(x))^\top$: **충분통계량** 벡터, $f_0$: 기준밀도(질량함수)
- normalizing function: $e^{\psi(\alpha)}=\int_{\mathcal X}\exp\lbrace\alpha^\top t(x)\rbrace f_0(x)\thinspace dx < \infty$ (2.6)
- **자연모수공간** $A=\lbrace\alpha: e^{\psi(\alpha)} < \infty\rbrace$ — 볼록집합이다.

⚠ "support가 모수에 의존하지 않는다"는 조건이 정의에 들어 있다. 이것이 [10](#10-보충-지수족인지-판정하기)의 판정 기준이다.

### 2.3 자연모수공간은 볼록집합

📐 [ch2 p5]

**주장** $\alpha_1,\alpha_2\in A$, $c\in[0,1]$이면 $c\alpha_1+(1-c)\alpha_2\in A$.

**증명 (강의노트)** 지수함수의 볼록성으로 모든 $y$에 대해

$$e^{[c\alpha_1+(1-c)\alpha_2]^\top y}\le c\thinspace e^{\alpha_1^\top y}+(1-c)\thinspace e^{\alpha_2^\top y}.$$

$y=t(x)$를 넣고 양변을 $f_0(x)\thinspace dx$로 적분하면 우변이 $ce^{\psi(\alpha_1)}+(1-c)e^{\psi(\alpha_2)} < \infty$ ($\alpha_1,\alpha_2\in A$). 따라서 좌변의 적분도 유한, 즉 $c\alpha_1+(1-c)\alpha_2\in A$. ∎

**(보충 증명) $\psi$도 볼록 — Hölder.** $p=1/c$, $q=1/(1-c)$로 Hölder 부등식을 쓰면

$$e^{\psi(c\alpha_1+(1-c)\alpha_2)}=\int\big(e^{\alpha_1^\top t}\big)^c\big(e^{\alpha_2^\top t}\big)^{1-c}f_0\le\Big(\int e^{\alpha_1^\top t}f_0\Big)^c\Big(\int e^{\alpha_2^\top t}f_0\Big)^{1-c}=e^{c\psi(\alpha_1)+(1-c)\psi(\alpha_2)}.$$

로그를 취하면 $\psi(c\alpha_1+(1-c)\alpha_2)\le c\psi(\alpha_1)+(1-c)\psi(\alpha_2)$. 우변이 유한하므로 $A$의 볼록성도 한꺼번에 나온다. (다른 증명: 헤시안 = 공분산 ⪰ 0 → [5.2](#52-다차원과-psi의-볼록성))

---

## 3. 예와 레시피

### 3.1 감마분포

🔢 [ch2 p5 (2.7)–(2.8)] — 여러 모수를 가진 지수족의 예

$$\frac{x^{\nu-1}e^{-x/\sigma}}{\sigma^\nu\Gamma(\nu)}=\exp\Big\lbrace-\frac1\sigma x+(\nu-1)\log x-\big(\nu\log\sigma+\log\Gamma(\nu)\big)\Big\rbrace\qquad(x \gt  0)$$

$$\alpha=(\alpha_1,\alpha_2)=\Big(-\frac1\sigma,\ \nu-1\Big),\qquad t(x)=(x,\ \log x),\qquad f_0(x)=1\ (x \gt  0),$$

$$\psi(\alpha)=\nu\log\sigma+\log\Gamma(\nu)=-(\alpha_2+1)\log(-\alpha_1)+\log\Gamma(\alpha_2+1),\qquad A=\lbrace(\alpha_1,\alpha_2):\alpha_1 \lt  0,\ \alpha_2 \gt  -1\rbrace.$$

- 검산: $\sigma=-1/\alpha_1$이라 $\log\sigma=-\log(-\alpha_1)$, $\nu=\alpha_2+1$.
- $A$의 이유: $\int_0^\infty x^{\alpha_2}e^{\alpha_1x}dx < \infty\iff\alpha_1 < 0$ (무한대 쪽) 그리고 $\alpha_2 > -1$ (0 근처).
- (보충) Theorem 2.1로 검산: $\partial\psi/\partial\alpha_1=-(\alpha_2+1)/\alpha_1=\sigma\nu=\mathbb EX$ ✓

### 3.2 지수족 꼴로 쓰는 4단계 레시피

(보충) hw1 #3·#5처럼 "지수족으로 나타내고 natural parameter, natural parameter space, log partition function을 보여라" 문제의 고정 순서다. [T11]

1. **log를 전개한다.** $\log f(x)$를 (모수와 $x$가 섞인 항) + (모수만의 항) + ($x$만의 항)으로 나눈다. support의 지시함수는 $h$에 넣는다 (모수에 의존하면 지수족이 아니다).
2. **짝짓기.** 섞인 항을 $\sum_j\eta_j(\theta)T_j(x)$로 쓴다 → $\eta$ = 자연모수, $T$ = 충분통계량. $x$만의 항 → $\log h(x)$. 모수만의 항 → $-A$.
3. **$A$를 $\eta$로 다시 쓴다.** $\theta=\theta(\eta)$를 풀어 대입한다. (log-partition은 $\eta$의 함수여야 Theorem 2.1을 쓸 수 있다)
4. **자연모수공간은 정의로 확인한다.** $\lbrace\eta:\int h(x)e^{\eta^\top T(x)}dx < \infty\rbrace$를 직접 계산한다. 원래 모수공간의 상(image)과 같은지도 확인한다.

**검산 두 가지:** ① $h\thinspace e^{\eta^\top T-A}$를 전개해서 원래 밀도로 돌아오는지 (부호!) ② $A'(\eta)$가 알려진 평균과 같은지.

### 3.3 자주 나오는 지수족 표

(보충) hw 기호로 쓴다: $f=h(x)\exp\lbrace\eta^\top T(x)-A(\eta)\rbrace$.

| 분포 | 자연모수 $\eta$ | $T(x)$ | $A(\eta)$ | $h(x)$ | 자연모수공간 |
| --- | --- | --- | --- | --- | --- |
| Bernoulli $(p)$ | $\log\frac p{1-p}$ | $x$ | $\log(1+e^\eta)$ | $1$ | $\mathbb R$ |
| Bin $(n,p)$, $n$ 알려짐 | $\log\frac p{1-p}$ | $x$ | $n\log(1+e^\eta)$ | $\binom nx$ | $\mathbb R$ |
| Poi $(\mu)$ | $\log\mu$ | $x$ | $e^\eta$ | $1/x!$ | $\mathbb R$ |
| 기하 $p(1-p)^x$, $x\ge0$ | $\log(1-p)$ | $x$ | $-\log(1-e^\eta)$ | $1$ | $(-\infty,0)$ |
| Exp (rate $\lambda$) | $-\lambda$ | $x$ | $-\log(-\eta)$ | $1$ ($x > 0$) | $(-\infty,0)$ |
| $N(\mu,\sigma^2)$, $\sigma^2$ 알려짐 | $\mu/\sigma^2$ | $x$ | $\sigma^2\eta^2/2$ | $\frac{e^{-x^2/(2\sigma^2)}}{\sqrt{2\pi\sigma^2}}$ | $\mathbb R$ |
| $N(\mu,\sigma^2)$ | $\big(\frac\mu{\sigma^2},-\frac1{2\sigma^2}\big)$ | $(x,x^2)$ | $-\frac{\eta_1^2}{4\eta_2}-\frac12\log(-2\eta_2)$ | $\frac1{\sqrt{2\pi}}$ | $\mathbb R\times(-\infty,0)$ |
| Gam $(\nu,\sigma)$ | $\big(-\frac1\sigma,\nu-1\big)$ | $(x,\log x)$ | $-(\eta_2+1)\log(-\eta_1)+\log\Gamma(\eta_2+1)$ | $1$ ($x > 0$) | $\eta_1 < 0,\ \eta_2 > -1$ |
| Be $(\nu_1,\nu_2)$ | $(\nu_1-1,\nu_2-1)$ | $(\log x,\log(1-x))$ | $\log\frac{\Gamma(\eta_1+1)\Gamma(\eta_2+1)}{\Gamma(\eta_1+\eta_2+2)}$ | $1$ ($0 < x < 1$) | $(-1,\infty)^2$ |
| 2변량 정규 (hw1 #5) | $(\Sigma^{-1}\mu,\ -\frac12\Sigma^{-1})$ | $(x,\ xx^\top)$ | $-\frac14\eta^\top\Lambda^{-1}\eta-\frac12\log\lvert-2\Lambda\rvert$ | $(2\pi)^{-1}$ | $\eta\in\mathbb R^2$, $\Lambda$ 대칭 음의 정부호 |

---

## 4. iid 표본과 충분통계량

📐📝 [ch2 p5–6 (2.9)] · hw1 #5(e)

$x=(x_1,\dots,x_n)$이 지수족의 iid 표본이고 $y_i=t(x_i)$이면

$$f_\alpha(x)=\prod_{i=1}^ne^{\alpha^\top y_i-\psi(\alpha)}f_0(x_i)=e^{n(\alpha^\top\bar y-\psi(\alpha))}\prod_{i=1}^nf_0(x_i),\qquad\bar y=\frac1n\sum_{i=1}^ny_i .\qquad(2.9)$$

- 우도에 관한 정보는 **표본 크기와 무관하게 $p$차원 통계량 $\bar y$ (또는 $\sum t(x_i){}$)에 모인다.** ch4의 분해정리로 보면 $h(x)=\prod f_0(x_i)$, $g_\alpha(T)=e^{\alpha^\top T-n\psi(\alpha)}$, $T=\sum t(x_i)$ → 충분통계량.
- 역으로, support가 모수에 의존하지 않는 등 정규성 조건 아래에서 **고정 차원의 충분통계량을 갖는 iid 모형은 본질적으로 지수족뿐**이다 (보충: Pitman–Koopman–Darmois 정리). → support가 모수에 의존하는 hw1 #8·#9에서 $\min$, $\max$ 같은 통계량이 나오는 이유.
- 한 모수 지수족에서 $\psi(\alpha)$는 기준분포 $f_0$ 아래에서 $y$의 **누적생성함수(cumulant generating function)**이고, 확률 그래프 모형에서는 **log-partition function**이라 부른다.

---

## 5. 누적생성함수와 Theorem 2.1

### 5.1 평균과 분산

📐 [ch2 p6 Thm 2.1]

**결론** $y=t(x)$에 대해

$$\dot\psi(\alpha)=\mathbb E_\alpha(y),\qquad\ddot\psi(\alpha)=\mathop{\mathrm{Var}}\nolimits_ \alpha(y).\qquad(2.10)$$

**핵심 한 수** 정규화 항등식 $e^{\psi(\alpha)}=\int e^{\alpha y}f_0\thinspace dx$를 $\alpha$로 미분한다. [T13]

**증명**
1. (2.6)의 양변을 $\alpha$로 미분: $\dot\psi(\alpha)e^{\psi(\alpha)}=\int y\thinspace e^{\alpha y}f_0(x)\thinspace dx$. (적분과 미분의 교환은 자연모수공간의 내부에서 허용된다 — 보충)
2. 양변을 $e^{\psi(\alpha)}$로 나누면 우변이 $\int y\thinspace f_\alpha(x)\thinspace dx=\mathbb E_\alpha(y)$ → 첫 관계.
3. 1의 식을 한 번 더 미분: $\big(\ddot\psi(\alpha)+\dot\psi(\alpha)^2\big)e^{\psi(\alpha)}=\int y^2e^{\alpha y}f_0(x)\thinspace dx$.
4. $e^{\psi}$로 나누면 $\ddot\psi+\dot\psi^2=\mathbb E_\alpha(y^2)$ → $\ddot\psi(\alpha)=\mathbb E_\alpha(y^2)-\lbrace\mathbb E_\alpha(y)\rbrace^2=\mathop{\mathrm{Var}}\nolimits_ \alpha(y)$. ∎

**셀프 체크**
<details><summary>Q1. 3단계에서 좌변이 ψ̈ + ψ̇² 인 이유는?</summary>

$\frac{d}{d\alpha}\big(\dot\psi e^{\psi}\big)=\ddot\psi e^\psi+\dot\psi\cdot\dot\psi e^\psi$ (곱의 미분 + 연쇄법칙).

</details>
<details><summary>Q2. 포아송(μ)에서 η = log μ, A = e^η 로 평균·분산을 구하면?</summary>

$A'(\eta)=e^\eta=\mu$, $A''(\eta)=e^\eta=\mu$.

</details>

### 5.2 다차원과 psi의 볼록성

📐🧩 [ch2 p6 (2.11)–(2.12), p7]

$$\dot\psi(\alpha)=\Big(\frac{\partial\psi}{\partial\alpha_j}\Big)_ j=\mathbb E_\alpha(y),\qquad\ddot\psi(\alpha)=\Big(\frac{\partial^2\psi(\alpha)}{\partial\alpha_j\thinspace\partial\alpha_k}\Big)_ {jk}=\mathop{\mathrm{Cov}}\nolimits_ \alpha(y)\equiv V_\alpha .$$

- (p7) 헤시안이 공분산행렬이라 양의 준정부호 → **$\psi$는 볼록함수**다. (2.3의 Hölder 증명과 같은 결론)
- (p7) 최소 정규(minimal regular) 지수족에서는 $V_\alpha$가 **Fisher 정보행렬**과 같다. 이 성질이 GLM의 뉴턴 방법, IRLS, 자연경사법(natural gradient)의 바탕이다.

### 5.3 보충 Theorem 2.1에서 나오는 것들

(보충) ch5 MLE와 이어지는 사실들. 증명이 짧아서 변형 문제로 쓰기 좋다.

1. **$t$의 MGF.** $\alpha+s\in A$이면 $\mathbb E_\alpha e^{s^\top t(X)}=\int e^{s^\top t}e^{\alpha^\top t-\psi(\alpha)}f_0=e^{\psi(\alpha+s)-\psi(\alpha)}$. 그래서 $s\mapsto\psi(\alpha+s)-\psi(\alpha)$가 $t$의 누적생성함수이고, 그 도함수들이 누율(평균, 분산, …)이다. Theorem 2.1은 그 1·2차 판이다.
2. **Fisher 정보.** $\log f_\alpha=\alpha^\top t-\psi(\alpha)+\log f_0$ → score $=t-\dot\psi(\alpha)$ → 정보 $=\mathop{\mathrm{Var}}\nolimits_ \alpha(t)=\ddot\psi(\alpha)$.
3. **MLE = 적률 맞추기.** iid 표본의 로그우도 $\ell(\alpha)=n\lbrace\alpha^\top\bar t-\psi(\alpha)\rbrace+\text{const}$ → $\nabla\ell=n\lbrace\bar t-\dot\psi(\alpha)\rbrace=0$ → $\dot\psi(\hat\alpha)=\bar t$, 즉 $\mathbb E_{\hat\alpha}t=\bar t$. $\nabla^2\ell=-n\ddot\psi\preceq0$이라 오목하고, $\ddot\psi\succ0$이면 해가 유일하다.
4. **두 분포 사이의 KL.** $D(f_{\alpha_1}\Vert f_{\alpha_2})=\mathbb E_{\alpha_1}\big[(\alpha_1-\alpha_2)^\top t-\psi(\alpha_1)+\psi(\alpha_2)\big]=\psi(\alpha_2)-\psi(\alpha_1)-(\alpha_2-\alpha_1)^\top\dot\psi(\alpha_1)\ge0$. 볼록함수 $\psi$의 접선 위 간격(Bregman divergence)이다.

---

## 6. 머신러닝 응용 GLM

💡 [ch2 p6–7 2.2.1]

- **GLM:** $Y\mid x$가 지수족이고 평균 $\mu(x)=\mathbb E(Y\mid x)$가 링크함수 $g$로 $g\lbrace\mu(x)\rbrace=x^\top\beta$. **정준연결(canonical link)**을 쓰면 $x^\top\beta$ = 자연모수.
  - 정규 + 항등 링크 → 선형회귀 · 베르누이 + logit → 로지스틱 회귀 · 포아송 + log → 포아송 회귀
- **로지스틱 회귀:** $\Pr(Y=1\mid x)=\dfrac{e^{x^\top\beta}}{1+e^{x^\top\beta}}$. 음의 로그우도

$$-\ell(\beta)=-\sum_{i=1}^n\big[y_i\log p_i+(1-y_i)\log(1-p_i)\big],\qquad p_i=\Pr(Y_i=1\mid x_i)$$

  는 **이진 교차엔트로피(BCE)**와 같다. 즉 교차엔트로피 최소화 = 베르누이 지수족의 MLE.
- **softmax:** $K$개 범주에서 $\eta_k(x)=x^\top\beta_k$, $\Pr(Y=k\mid x)=\dfrac{e^{\eta_k(x)}}{\sum_{j=1}^Ke^{\eta_j(x)}}$. 다중분류 교차엔트로피 = 범주형 지수족의 음의 로그우도. 신경망 마지막 층이 softmax + 교차엔트로피인 이유.
- **최적화:** $\psi$의 헤시안 = 공분산 ⪰ 0 → $\psi$ 볼록 → 음의 로그우도가 볼록 → 뉴턴, IRLS, natural gradient.

---

## 7. 켤레사전분포와 변분추론

💡 [ch2 p7 2.2.2]

- **베타–베르누이:** $\pi\sim\text{Be}(a,b)$, $X_i\mid\pi\overset{\text{ind}}{\sim}\text{Bernoulli}(\pi)$, $s=\sum X_i$이면

$$\pi\mid X_1,\dots,X_n\sim\text{Be}(a+s,\ b+n-s).$$

  새 자료가 들어오면 **충분통계량(성공·실패 수)만 갱신**하면 된다. 다른 켤레쌍: 디리클레–다항, 감마–포아송.
  - (보충) 증명: $\pi^{a-1}(1-\pi)^{b-1}\cdot\pi^s(1-\pi)^{n-s}=\pi^{a+s-1}(1-\pi)^{b+n-s-1}$ (사후 ∝ 사전 × 우도).
- **변분추론:** 조건부 켤레 지수족 구조 덕분에 LDA (문서별 토픽 비율 = 디리클레, 토픽별 단어 확률 = 다항)에서 평균장 변분분포의 갱신식을 **기대 충분통계량**의 꼴로 얻는다. 대규모 자료는 미니배치로 확률적 변분추론. 켤레성이 없는 깊은 생성모형은 재매개화 기울기나 몬테카를로가 필요하다.

---

## 8. Lindsey 방법으로 밀도 추정

💡 [ch2 p8–9 2.2.3]

GFR (사구체여과율) 자료 211명: 평균 54.25, $s/\sqrt n=0.95$. $f_0(x)=1$, $t(x)=(x,x^2,\dots,x^7)$로 두면 $\alpha^\top t(x)$가 7차 다항식이다. (2차면 정규분포 적합과 같은데, 이 자료에는 정규분포가 맞지 않는다.) 그림 2.4는 7모수 지수족(실선)과 감마(점선)를 비교한다.

**MLE를 구하는 3단계**
1. $x$ 축을 같은 폭의 bin $K$개로 나누고 bin $k$의 개수를 $y_k$라 하면 $(y_1,\dots,y_K)\sim\text{Mult}_ K(n,\pi)$, $\pi_k\approx e^{\alpha^\top t(x_{(k)})-\psi(\alpha)}\cdot c$ ($x_{(k)}$ = bin 중점, $c$ = bin 폭). 정규화 $\tilde\pi_k=e^{\alpha^\top t(x_{(k)})}/\sum_je^{\alpha^\top t(x_{(j)})}$ — $\psi$에 의존하지 않는다.
2. 다항분포를 포아송으로 근사: $y_k\overset{\text{ind}}{\sim}\text{Poi}(n\tilde\pi_k)$.
3. 포아송 회귀: $\log\mathbb E(y_k)=\tilde\alpha_0+\alpha_1x_{(k)}+\cdots+\alpha_px_{(k)}^p$, 여기서 $\tilde\alpha_0=\alpha_0+\log n$, $e^{-\alpha_0}=\sum_ke^{\alpha^\top t(x_{(k)})}$. 적합하면 $\hat\alpha$.

**네트워크 모형:** 노드 $N$개 그래프에서 $E$ = 간선 수, $T$ = 삼각형 수, $y=(E,T)$인 2모수 지수족. $\alpha_1,\alpha_2$가 클수록 촘촘한 네트워크.

---

## 9. 최대 엔트로피와 지수족

📐 [ch2 p9–10]

- **엔트로피** $H_f(X)=-\mathbb E\log f(X)$. 유한 표본공간에서는 균등분포가 최대. 실수 전체에서 평균·분산을 고정하면 정규분포, 유한 구간만 제약하면 균등분포가 최대.
- **KL 발산** $D_{KL}(f\Vert g)=\mathbb E_f\log\frac{f(X)}{g(X)}$. $f_0\equiv c$ (상수)이면 $D_{KL}(f\Vert f_0)=-H_f(X)+\text{const}$ → **KL 최소화 = 엔트로피 최대화**.

**주장** 제약 $\mathbb E_f[t_k(X)]=\beta_k$ ($k=1,\dots,K$) 아래 $D_{KL}(f\Vert f_0)$를 최소화하는 $f$는 지수족이다.

**증명 (이산형, 라그랑주)**

$$J(f,\alpha)=-\sum_xf(x)\log\frac{f(x)}{f_0(x)}+\alpha_0\Big(\sum_xf(x)-1\Big)+\sum_{k=1}^K\alpha_k\Big(\sum_xt_k(x)f(x)-\beta_k\Big).$$

$f$를 벡터로 보고 성분 $f(c)$로 미분하면

$$\frac{\partial J}{\partial f(c)}=-1-\log\frac{f(c)}{f_0(c)}+\alpha_0+\sum_{k=1}^K\alpha_kt_k(c)=0\ \Longrightarrow\ f(x)=e^{\alpha^\top t(x)-\psi(\alpha)}f_0(x),\quad\psi(\alpha)=1-\alpha_0 .$$

즉 해는 지수족이다. 연속형에도 똑같이 성립한다. ∎

**의미** 주어진 정보(제약)만 반영하고 나머지는 가장 비정보적인 분포 → 사전분포로 쓰면 weakly informative. NLP의 다항 로지스틱 회귀도 maximum entropy classifier에 기반한다.

---

## 10. 보충 지수족인지 판정하기

(보충) 강의노트 정의 (2.5)에서 바로 나온다.

**support 판정법.** 지수족이면 $f_\alpha(x) > 0\iff f_0(x) > 0$이므로 **support가 모수와 무관**하다. 따라서 support가 모수에 따라 움직이면 지수족이 아니다.
- $\text{Unif}(0,\theta)$: support $(0,\theta)$가 $\theta$에 의존 → 아님
- hw1 #8의 $f(x\mid\theta)=e^{\theta-x}I(x\ge\theta)$: support $[\theta,\infty)$ → 아님. 그래서 충분통계량이 $\sum x_i$가 아니라 $\min x_i$다.
- 파레토 $\theta x^{-2}I(x\ge\theta)$ → 아님

**curved 지수족.** $N(\theta,\theta^2)$ ($\theta > 0$):

$$f(x)=\frac1{\sqrt{2\pi}\thinspace\theta}\exp\Big(-\frac{(x-\theta)^2}{2\theta^2}\Big)=\frac1{\sqrt{2\pi}}\exp\Big\lbrace\frac1\theta x-\frac1{2\theta^2}x^2-\frac12-\log\theta\Big\rbrace.$$

$T=(x,x^2)$는 2차원인데 자연모수 $(1/\theta,\ -1/(2\theta^2))$가 곡선 $\eta_2=-\eta_1^2/2$ 위에만 있다 (1차원 모수). 지수족이지만 자연모수공간 전체를 채우지 않는다.

---

## hw1 풀이

이 장에 속하는 문제: #3, #5. (hw 기호 $\eta, T, A, h$로 쓴다.)

### hw1 3 이항분포의 지수족 표현

📝 [hw1 p2] · 내 풀이 [CamScanner p4]

**문제 요지** $X\sim\text{Binomial}(n,p)$, $p\in(0,1)$을 지수족 꼴로 쓰고 natural parameter, natural parameter space, log partition function을 보여라.

**전략** 4단계 레시피 [3.2](#32-지수족-꼴로-쓰는-4단계-레시피). $p^x(1-p)^{n-x}=\exp\lbrace x\log p+(n-x)\log(1-p)\rbrace$에서 $x$와 $p$가 섞인 항을 모은다.

**모범 답안** $x=0,1,\dots,n$에 대해

$$\Pr_p(X=x)=\binom nx\exp\Big\lbrace x\log\frac p{1-p}+n\log(1-p)\Big\rbrace.$$

- **natural parameter:** $\eta=\log\dfrac p{1-p}$ (logit), 충분통계량 $T(x)=x$, $h(x)=\binom nx I(x\in\lbrace0,\dots,n\rbrace)$.
- **log partition function:** $1-p=\dfrac1{1+e^\eta}$이므로 $-n\log(1-p)=n\log(1+e^\eta)$, 즉 $A(\eta)=n\log(1+e^\eta)$. 그래서 $\Pr_\eta(X=x)=h(x)\exp\lbrace\eta x-A(\eta)\rbrace$.
- **natural parameter space:** $\lbrace\eta:\sum_{x=0}^n\binom nxe^{\eta x} < \infty\rbrace$인데 이 합은 유한합으로 $(1+e^\eta)^n$이므로 모든 $\eta$에서 유한하다 → **$\mathbb R$**. (원래 모수 $p\in(0,1)$의 상 $\lbrace\log\frac p{1-p}\rbrace$도 $\mathbb R$ 전체라 둘이 일치한다.)
- 검산: $A'(\eta)=\dfrac{ne^\eta}{1+e^\eta}=np=\mathbb EX$, $A''(\eta)=\dfrac{ne^\eta}{(1+e^\eta)^2}=np(1-p)=\mathop{\mathrm{Var}}\nolimits X$ ✓

**채점 포인트**
- $n$은 **알려진 상수**여야 한다 (모르면 support $\lbrace0,\dots,n\rbrace$이 모수에 의존해 지수족이 아니다)
- support가 $p$와 무관함을 언급
- $A$를 **$\eta$의 함수로** 쓰기 ($-n\log(1-p)$로 끝내지 않기)
- 자연모수공간은 정의(합의 유한성)로 확인

**내 풀이 점검**
- ✅ $\eta=\log\frac p{1-p}$, $T(x)=x$, $h(x)=\binom nx$, $A(\eta)=n\log(1+e^\eta)$, 공간 $\mathbb R$ 모두 맞다.
- ⚠ 자연모수공간이 $\mathbb R$인 **이유** (합 $=(1+e^\eta)^n < \infty$)를 한 줄 쓰면 완전하다.

### hw1 5 2변량 정규분포의 지수족 표현

📝 [hw1 p4] · 내 풀이 [CamScanner p7]

**문제 요지** $X\sim N_2(\mu,\Sigma)$, $\Sigma$ 대칭 양의 정부호. $f_{\mu,\Sigma}(x)=\frac1{2\pi\lvert\Sigma\rvert^{1/2}}\exp\lbrace-\frac12(x-\mu)^\top\Sigma^{-1}(x-\mu)\rbrace$를 지수족으로 쓰고 자연모수공간과 충분통계량을 밝혀라.

**전략** 이차형식 전개 → 이차항은 trace trick으로 "행렬 모수 × 행렬 통계량" → 모수만의 항과 $\lvert\Sigma\rvert^{-1/2}$을 모두 $A$로.

**모범 답안**

**(a)** $\Sigma^{-1}$이 대칭이라 $x^\top\Sigma^{-1}\mu=\mu^\top\Sigma^{-1}x$이므로

$$-\frac12(x-\mu)^\top\Sigma^{-1}(x-\mu)=\underbrace{-\frac12x^\top\Sigma^{-1}x}_ {\text{이차항}}+\underbrace{\mu^\top\Sigma^{-1}x}_ {\text{선형항}}\ \underbrace{-\frac12\mu^\top\Sigma^{-1}\mu}_ {x\text{ 무관}} .$$

**(b)** $\eta=\Sigma^{-1}\mu$, $\Lambda=-\frac12\Sigma^{-1}$로 두면 선형항 $=(\Sigma^{-1}\mu)^\top x=\eta^\top x$. 이차항 $=x^\top\Lambda x=\mathop{\mathrm{tr}}\nolimits(x^\top\Lambda x)=\mathop{\mathrm{tr}}\nolimits(\Lambda xx^\top)$ (스칼라는 자기 trace, trace의 순환성). 앞의 $\lvert\Sigma\rvert^{-1/2}$은 $e^{-\frac12\log\lvert\Sigma\rvert}$로 지수에 넣는다:

$$f_{\mu,\Sigma}(x)=\underbrace{(2\pi)^{-1}}_ {h(x)}\exp\Big\lbrace\eta^\top\underbrace{x}_ {T_1(x)}+\mathop{\mathrm{tr}}\nolimits\big(\Lambda\underbrace{xx^\top}_ {T_2(x)}\big)-\Big(\frac12\mu^\top\Sigma^{-1}\mu+\frac12\log\lvert\Sigma\rvert\Big)\Big\rbrace.$$

**(c)** $\Lambda=-\frac12\Sigma^{-1}$에서 $\Sigma^{-1}=-2\Lambda$, $\Sigma=(-2\Lambda)^{-1}=-\frac12\Lambda^{-1}$. $\mu=\Sigma\eta=-\frac12\Lambda^{-1}\eta$. 그러면

$$\frac12\mu^\top\Sigma^{-1}\mu=\frac12\mu^\top\eta=-\frac14\eta^\top\Lambda^{-1}\eta,\qquad\frac12\log\lvert\Sigma\rvert=\frac12\log\lvert(-2\Lambda)^{-1}\rvert=-\frac12\log\lvert-2\Lambda\rvert$$

($\Lambda^{-1}$ 대칭). 따라서 $A(\eta,\Lambda)=-\dfrac14\eta^\top\Lambda^{-1}\eta-\dfrac12\log\lvert-2\Lambda\rvert$.

**(d)** 자연모수공간 $=\lbrace(\eta,\Lambda):\int\exp\lbrace\eta^\top x+x^\top\Lambda x\rbrace dx < \infty\rbrace$ ($\Lambda$ 대칭) $=\lbrace\eta\in\mathbb R^2,\ \Lambda\ \text{대칭 음의 정부호}\rbrace$.
- **왜 음의 정부호:** $\Lambda=Q\thinspace\text{diag}(\lambda_1,\lambda_2)Q^\top$로 대각화하고 $x=Qy$로 바꾸면 적분이 $\prod_j\int\exp(c_jy_j+\lambda_jy_j^2)dy_j$ ($c=Q^\top\eta$)로 갈라진다. $\lambda_j < 0$이면 가우스 적분이라 유한, $\lambda_j\ge0$이면 $\int e^{cy+\lambda y^2}dy=\infty$. 따라서 유한 ⟺ 모든 고유값 음수. (원래 모수로 보면 $\Sigma\succ0$ ⟺ $\Lambda=-\frac12\Sigma^{-1}\prec0$.)
- **왜 대칭:** $T_2=xx^\top$가 대칭이라 $\mathop{\mathrm{tr}}\nolimits(\Lambda xx^\top)=\mathop{\mathrm{tr}}\nolimits\big(\frac{\Lambda+\Lambda^\top}2xx^\top\big)$ — 비대칭 부분은 밀도에 나타나지 않는다. 모수화가 일대일이 되도록 대칭으로 제한한다.
- $\eta$에는 제약이 없다: 어떤 $\eta$와 대칭 음의 정부호 $\Lambda$에도 $\Sigma=-\frac12\Lambda^{-1}\succ0$, $\mu=\Sigma\eta$가 정해진다.

**(e)** 독립이므로

$$\prod_{i=1}^nf(x_i)=(2\pi)^{-n}\exp\Big\lbrace\eta^\top\sum_{i=1}^nx_i+\mathop{\mathrm{tr}}\nolimits\Big(\Lambda\sum_{i=1}^nx_ix_i^\top\Big)-nA(\eta,\Lambda)\Big\rbrace$$

(trace의 선형성). 자료는 $\big(\sum x_i,\ \sum x_ix_i^\top\big)$를 통해서만 들어가므로 분해정리 ($h=(2\pi)^{-n}$)로 $\big(\sum X_i,\ \sum X_iX_i^\top\big)$가 $(\mu,\Sigma)$에 대한 충분통계량이다. ($X_iX_i^\top$가 대칭이라 서로 다른 성분은 $2+3=5$개 = 모수 개수)

**채점 포인트**
- (a) 교차항 두 개가 합쳐지는 이유 ($\Sigma^{-1}$ 대칭)
- (b) **$\lvert\Sigma\rvert^{-1/2}$을 $A$에 넣기**, $-A$의 부호, trace trick의 근거
- (c) $\mu,\Sigma$를 $\eta,\Lambda$로 푸는 과정
- (d) **적분 가능성**으로 설명 (단지 " $\Sigma$가 양의 정부호라서"만으로는 약함), 대칭의 이유
- (e) 분해정리를 명시

**내 풀이 점검**
- ✅ (a)의 전개, (c)의 $A$ 유도, (d)의 결론, (e) 모두 맞다.
- ⚠ **(b) 부호 실수:** $\frac1{2\pi\lvert\Sigma\rvert^{1/2}}\exp\big(\eta^\top x+\mathop{\mathrm{tr}}\nolimits(\Lambda xx^\top)\ \mathbf{+}\ A(\eta,\Lambda)\big)$라고 썼다. 맞는 식은 $(2\pi)^{-1}\exp\big(\eta^\top x+\mathop{\mathrm{tr}}\nolimits(\Lambda xx^\top)\ \mathbf{-}\ A(\eta,\Lambda)\big)$. 두 가지가 틀렸다: ① 지수의 $A$ 부호 ② $\lvert\Sigma\rvert^{-1/2}$을 앞에 남긴 채 $h(x)=(2\pi)^{-1}$이라고 함 (그러면 $\lvert\Sigma\rvert$ 인자가 두 번 들어간다). (c)에서 $A=\frac12\mu^\top\Sigma^{-1}\mu+\frac12\log\lvert\Sigma\rvert$를 맞게 구한 것을 보면 개념은 알고 있으니, **쓴 뒤 원래 밀도로 되돌려 보는 검산**만 하면 잡을 수 있는 실수다.
- ⚠ (d) " $\Sigma$가 positive definite이라서 $\Lambda$가 negative definite"만 썼다 → 자연모수공간은 **적분이 유한한 모수의 집합**으로 정의되므로 적분 가능성으로 설명해야 완전하다.

---

## 변형 문제

난이도: `●` hw 문제의 분포만 바꿈 · `●●` 도구 두세 개 조합 · `●●●` 장을 넘나듦. 힌트부터 열고, 스스로 써 본 뒤 풀이를 연다.

### V1 한 모수 지수족 네 개

`●` 다음 각각을 $h(x)\exp\lbrace\eta T(x)-A(\eta)\rbrace$ 꼴로 쓰고 $\eta$, $T$, $A(\eta)$, 자연모수공간을 구한 뒤, $A'$, $A''$로 평균과 분산을 구하라.
(a) $\text{Poi}(\mu)$ (b) 기하분포 $p(1-p)^x$, $x=0,1,\dots$ (c) 지수분포 $\lambda e^{-\lambda x}$ (d) $N(\mu,\sigma^2)$, $\sigma^2$ 알려짐

<details><summary>전략 힌트</summary>

4단계 레시피. 자연모수공간은 $\sum$ 또는 $\int h\thinspace e^{\eta T}$가 유한한 $\eta$ — 등비급수, 지수적분.

</details>
<details><summary>풀이</summary>

(a) $e^{-\mu}\mu^x/x!=\frac1{x!}e^{x\log\mu-\mu}$: $\eta=\log\mu$, $T=x$, $A=e^\eta$, 공간 $\mathbb R$ ($\sum e^{\eta x}/x!=e^{e^\eta}$). $A'=A''=e^\eta=\mu$.

(b) $p(1-p)^x=e^{x\log(1-p)+\log p}$: $\eta=\log(1-p)$, $A=-\log p=-\log(1-e^\eta)$, 공간 $(-\infty,0)$ ($\sum e^{\eta x} < \infty\iff\eta < 0$). $A'=\frac{e^\eta}{1-e^\eta}=\frac{1-p}p$, $A''=\frac{e^\eta}{(1-e^\eta)^2}=\frac{1-p}{p^2}$.

(c) $\lambda e^{-\lambda x}=e^{-\lambda x+\log\lambda}$ ($x > 0$): $\eta=-\lambda$, $A=-\log(-\eta)$, 공간 $(-\infty,0)$. $A'=-1/\eta=1/\lambda$, $A''=1/\eta^2=1/\lambda^2$.

(d) $\frac1{\sqrt{2\pi\sigma^2}}e^{-x^2/(2\sigma^2)}\cdot e^{\frac\mu{\sigma^2}x-\frac{\mu^2}{2\sigma^2}}$: $\eta=\mu/\sigma^2$, $A=\frac{\mu^2}{2\sigma^2}=\frac{\sigma^2\eta^2}2$, 공간 $\mathbb R$. $A'=\sigma^2\eta=\mu$, $A''=\sigma^2$.

</details>

### V2 정규분포 두 모수 모두 미지

`●●` $X\sim N(\mu,\sigma^2)$.
(a) 2모수 지수족으로 쓰고 $\eta$, $T$, $A(\eta)$, 자연모수공간을 구하라.
(b) $\nabla A(\eta)=(\mathbb EX,\ \mathbb EX^2)$임을 확인하라.
(c) $\partial^2A/\partial\eta_1^2$, $\partial^2A/\partial\eta_1\partial\eta_2$를 계산해 $\mathop{\mathrm{Var}}\nolimits X$, $\mathop{\mathrm{Cov}}\nolimits(X,X^2)$와 비교하라.

<details><summary>전략 힌트</summary>

$-\frac{(x-\mu)^2}{2\sigma^2}=\frac\mu{\sigma^2}x-\frac1{2\sigma^2}x^2-\frac{\mu^2}{2\sigma^2}$. $\sigma^2=-\frac1{2\eta_2}$, $\mu=-\frac{\eta_1}{2\eta_2}$로 되돌린다.

</details>
<details><summary>풀이</summary>

(a) $\eta=(\frac\mu{\sigma^2},-\frac1{2\sigma^2})$, $T=(x,x^2)$, $h=\frac1{\sqrt{2\pi}}$, $A=\frac{\mu^2}{2\sigma^2}+\frac12\log\sigma^2=-\frac{\eta_1^2}{4\eta_2}-\frac12\log(-2\eta_2)$. 공간: $\int e^{\eta_1x+\eta_2x^2}dx < \infty\iff\eta_2 < 0$ → $\mathbb R\times(-\infty,0)$.

(b) $\partial A/\partial\eta_1=-\frac{\eta_1}{2\eta_2}=\mu$ ✓. $\partial A/\partial\eta_2=\frac{\eta_1^2}{4\eta_2^2}-\frac1{2\eta_2}=\mu^2+\sigma^2=\mathbb EX^2$ ✓.

(c) $\partial^2A/\partial\eta_1^2=-\frac1{2\eta_2}=\sigma^2=\mathop{\mathrm{Var}}\nolimits X$ ✓. $\partial^2A/\partial\eta_1\partial\eta_2=\frac{\eta_1}{2\eta_2^2}=2\mu\sigma^2$, 그리고 $\mathop{\mathrm{Cov}}\nolimits(X,X^2)=\mathbb EX^3-\mu\thinspace\mathbb EX^2=(\mu^3+3\mu\sigma^2)-\mu(\mu^2+\sigma^2)=2\mu\sigma^2$ ✓.

</details>

### V3 베타분포와 로그의 기댓값

`●●` $X\sim\text{Be}(\nu_1,\nu_2)$. (a) 2모수 지수족으로 쓰고 자연모수공간을 구하라. (b) Theorem 2.1로 $\mathbb E\log X=\Psi(\nu_1)-\Psi(\nu_1+\nu_2)$를 보여라 ($\Psi=\Gamma'/\Gamma$는 디감마 함수. 소문자 $\psi$는 log-partition이라 대문자로 쓴다).

<details><summary>풀이</summary>

(a) $\eta=(\nu_1-1,\nu_2-1)$, $T=(\log x,\log(1-x))$, $h=1$ ($0 < x < 1$), $A=\log\Gamma(\eta_1+1)+\log\Gamma(\eta_2+1)-\log\Gamma(\eta_1+\eta_2+2)$. 공간: $\int_0^1x^{\eta_1}(1-x)^{\eta_2}dx < \infty\iff\eta_1,\eta_2 > -1$ → $(-1,\infty)^2$.

(b) $\mathbb E\log X=\partial A/\partial\eta_1=\Psi(\eta_1+1)-\Psi(\eta_1+\eta_2+2)=\Psi(\nu_1)-\Psi(\nu_1+\nu_2)$.

</details>

### V4 지수족이 아님을 보이기

`●●` (a) 지수족이면 support $\lbrace x:f_\theta(x) > 0\rbrace$가 $\theta$와 무관함을 보여라. (b) $\text{Unif}(0,\theta)$와 hw1 #8의 $e^{\theta-x}I(x\ge\theta)$가 지수족이 아님을 보여라. (c) 이 사실과 hw1 #8·#9의 충분통계량 모양($\min$, $\max$)의 관계를 설명하라.

<details><summary>풀이</summary>

(a) $f_\theta(x)=h(x)e^{\eta(\theta)^\top T(x)-A(\theta)}$에서 지수 인자는 항상 양수이므로 $f_\theta(x) > 0\iff h(x) > 0$ — $\theta$와 무관.

(b) $\text{Unif}(0,\theta)$의 support $(0,\theta)$, $e^{\theta-x}I(x\ge\theta)$의 support $[\theta,\infty)$는 $\theta$에 따라 달라진다 → (a)에 모순.

(c) 지수족이면 충분통계량이 $\sum t(x_i)$ 꼴 (합). support가 모수에 의존하면 결합밀도에 $\prod I(\cdot)$가 생기고, 이 곱이 $\min$· $\max$의 지시함수로 바뀌어 충분통계량이 순서통계량 꼴이 된다 (→ ch4).

</details>

### V5 충분통계량의 MGF

`●●` (a) $\alpha+s\in A$이면 $\mathbb E_\alpha e^{s^\top t(X)}=e^{\psi(\alpha+s)-\psi(\alpha)}$임을 보여라.
(b) (a)와 (2.9)로 $X_i\overset{\text{iid}}{\sim}\text{Poi}(\mu)$일 때 $\sum X_i\sim\text{Poi}(n\mu)$임을 보여라.

<details><summary>풀이</summary>

(a) $\int e^{s^\top t}e^{\alpha^\top t-\psi(\alpha)}f_0=e^{-\psi(\alpha)}\int e^{(\alpha+s)^\top t}f_0=e^{\psi(\alpha+s)-\psi(\alpha)}$.

(b) 포아송: $\alpha=\log\mu$, $\psi(\alpha)=e^\alpha$ ($f_0=1/x!$). $\mathbb Ee^{sX_1}=e^{e^{\alpha+s}-e^\alpha}=e^{\mu(e^s-1)}$. 독립이라 $\mathbb Ee^{s\sum X_i}=e^{n\mu(e^s-1)}$ = $\text{Poi}(n\mu)$의 MGF. MGF가 0 근방에서 같으면 분포가 같다.

</details>

### V6 psi의 볼록성과 MLE

`●●●` (a) Hölder로 $\psi$가 볼록임을 보여라 ([2.3](#23-자연모수공간은-볼록집합)).
(b) iid 표본의 MLE가 $\dot\psi(\hat\alpha)=\bar t$를 만족하고, $\mathop{\mathrm{Var}}\nolimits_ \alpha(t) > 0$이면 유일함을 보여라.
(c) 포아송 $\mu$, 베르누이 $p$, 지수분포(rate) $\lambda$의 MLE를 (b)로 구하라.

<details><summary>전략 힌트</summary>

(b) $\ell(\alpha)=n(\alpha\bar t-\psi(\alpha))$ → 1계 조건, $\ell''=-n\ddot\psi=-n\mathop{\mathrm{Var}}\nolimits(t) < 0$ → 순오목. (c) $\dot\psi(\eta)$ = 평균을 $\bar x$와 같게.

</details>
<details><summary>풀이</summary>

(a) 본문 2.3의 보충 증명.

(b) $\ell'(\alpha)=n(\bar t-\dot\psi(\alpha))=0$ ⟺ $\dot\psi(\hat\alpha)=\bar t$. $\ell''=-n\ddot\psi=-n\mathop{\mathrm{Var}}\nolimits_ \alpha(t) < 0$이면 $\ell$이 순오목 → 극대점은 많아야 하나.

(c) 포아송: $\dot\psi=e^\eta=\mu$ → $\hat\mu=\bar x$. 베르누이: $\dot\psi=\frac{e^\eta}{1+e^\eta}=p$ → $\hat p=\bar x$. 지수분포: $T=x$, $\eta=-\lambda$, $\dot\psi=-1/\eta=1/\lambda$ → $\hat\lambda=1/\bar x$. (포아송·베르누이는 $\bar x$가 경계값 0이나 1이면 $\hat\eta=\pm\infty$ — 자연모수공간 안에 MLE가 없다)

</details>

### V7 지수족 사이의 KL

`●●` (a) $D(f_{\alpha_1}\Vert f_{\alpha_2})=\psi(\alpha_2)-\psi(\alpha_1)-(\alpha_2-\alpha_1)^\top\dot\psi(\alpha_1)$을 보여라.
(b) $D(\text{Poi}(\mu_1)\Vert\text{Poi}(\mu_2))$와 $D(N(\mu_1,1)\Vert N(\mu_2,1))$을 구하라.

<details><summary>풀이</summary>

(a) $\log\frac{f_{\alpha_1}}{f_{\alpha_2}}=(\alpha_1-\alpha_2)^\top t-\psi(\alpha_1)+\psi(\alpha_2)$에 $\mathbb E_{\alpha_1}$ ($\mathbb E_{\alpha_1}t=\dot\psi(\alpha_1){}$).

(b) 포아송 ($\alpha=\log\mu$, $\psi=e^\alpha$): $\mu_2-\mu_1-(\log\mu_2-\log\mu_1)\mu_1=\mu_1\log\frac{\mu_1}{\mu_2}-\mu_1+\mu_2$. 정규 ($\alpha=\mu$, $\psi=\alpha^2/2$): $\frac{\mu_2^2}2-\frac{\mu_1^2}2-(\mu_2-\mu_1)\mu_1=\frac{(\mu_1-\mu_2)^2}2$.

</details>

### V8 최대 엔트로피 분포 찾기

`●●` 강의노트의 라그랑주 방법으로 (a) $(0,\infty)$ 위에서 $\mathbb EX=m$을 만족하며 엔트로피가 최대인 밀도, (b) $\lbrace0,1,2,\dots\rbrace$ 위에서 $\mathbb EX=m$을 만족하며 엔트로피가 최대인 pmf를 구하라.

<details><summary>풀이</summary>

$f_0\equiv1$로 두면 해는 $f(x)=e^{\alpha_1x-\psi(\alpha_1)}$.

(a) 적분 가능하려면 $\alpha_1 < 0$. $\lambda=-\alpha_1$로 두면 $f(x)=\lambda e^{-\lambda x}$, 평균 조건 $1/\lambda=m$ → **지수분포 (평균 $m$)**.

(b) $r=e^{\alpha_1} < 1$로 두면 $f(x)=(1-r)r^x$ (기하분포), 평균 $\frac r{1-r}=m$ → $r=\frac m{1+m}$.

</details>

### V9 로지스틱 회귀의 볼록성

`●●` $\ell(\beta)=\sum_i\big[y_ix_i^\top\beta-\log(1+e^{x_i^\top\beta})\big]$ (베르누이 로그우도).
(a) $\nabla\ell=\sum(y_i-p_i)x_i$, $\nabla^2\ell=-\sum p_i(1-p_i)x_ix_i^\top$를 보여라 ($p_i=\frac{e^{x_i^\top\beta}}{1+e^{x_i^\top\beta}}$).
(b) 음의 로그우도(= BCE)가 볼록임을 보이고 Theorem 2.1과 연결하라.

<details><summary>풀이</summary>

(a) $\frac{d}{d\eta}\log(1+e^\eta)=\frac{e^\eta}{1+e^\eta}=p$, $\frac{d^2}{d\eta^2}=p(1-p)$. 연쇄법칙 ($\eta_i=x_i^\top\beta$)으로 $\nabla\ell=\sum(y_i-p_i)x_i$, $\nabla^2\ell=-\sum p_i(1-p_i)x_ix_i^\top$.

(b) $v^\top(-\nabla^2\ell)v=\sum p_i(1-p_i)(x_i^\top v)^2\ge0$ → $-\ell$ 볼록. $p(1-p)$는 베르누이의 $A''(\eta)=\mathop{\mathrm{Var}}\nolimits(Y)$ — "log-partition의 2계도함수 = 분산 ≥ 0"이 볼록성의 이유다.

</details>

### V10 베타–이항 사후평균

`●` $\pi\sim\text{Be}(a,b)$, $X\mid\pi\sim\text{Bin}(n,\pi)$. 사후분포를 구하고, 사후평균이 사전평균 $\frac a{a+b}$와 MLE $\frac xn$의 가중평균임을 보여라.

<details><summary>풀이</summary>

사후 ∝ $\pi^{a-1}(1-\pi)^{b-1}\pi^x(1-\pi)^{n-x}$ → $\text{Be}(a+x,\ b+n-x)$. 평균

$$\frac{a+x}{a+b+n}=\frac{a+b}{a+b+n}\cdot\frac a{a+b}+\frac n{a+b+n}\cdot\frac xn .$$

$a+b$ = 사전 표본 크기처럼 작용. $n\to\infty$이면 MLE 쪽 가중치 → 1.

</details>

### V11 curved 지수족

`●●` $X\sim N(\theta,\theta^2)$, $\theta > 0$. (a) 지수족 꼴로 쓰고, 자연모수가 놓인 곡선을 구하라. (b) iid 표본의 충분통계량을 구하고, 모수 차원과 비교하라.

<details><summary>풀이</summary>

(a) 본문 [10](#10-보충-지수족인지-판정하기): $\eta=(1/\theta,\ -1/(2\theta^2))$, $T=(x,x^2)$, 곡선 $\eta_2=-\eta_1^2/2$ ($\eta_1 > 0$).

(b) (2.9)로 $(\sum X_i,\sum X_i^2)$ — 2차원. 모수는 1차원이라 충분통계량의 차원이 더 크다 (ch4 V8에서 이것이 최소충분임을 보인다).

</details>

---

## 셀프 테스트

### 백지 증명 체크리스트

| ☐ | 증명·유도 | 뼈대 | 위치 |
| --- | --- | --- | --- |
| ☐ | 포아송 기울이기 | 비 (2.1) → $\alpha=\log(\mu/\mu_0)$, $\psi=\mu_0(e^\alpha-1)$ → $e^\psi=\sum e^{\alpha x}f_{\mu_0}$ | [2.1](#21-포아송-분포를-기울여서-만들기) |
| ☐ | **자연모수공간의 볼록성** | $e^{(c\alpha_1+(1-c)\alpha_2)^\top y}\le ce^{\alpha_1^\top y}+(1-c)e^{\alpha_2^\top y}$ → $f_0$로 적분 | [2.3](#23-자연모수공간은-볼록집합) |
| ☐ | $\psi$의 볼록성 (보충) | Hölder $p=1/c$, $q=1/(1-c)$ | [2.3](#23-자연모수공간은-볼록집합) |
| ☐ | 감마의 지수족 꼴 | $\alpha=(-1/\sigma,\nu-1)$, $t=(x,\log x)$, $\psi$를 $\alpha$로, $A$ | [3.1](#31-감마분포) |
| ☐ | **Theorem 2.1** | $e^\psi=\int e^{\alpha y}f_0$ 미분 → 나누기 → 한 번 더 → $\ddot\psi+\dot\psi^2=\mathbb Ey^2$ | [5.1](#51-평균과-분산) |
| ☐ | 최대 엔트로피 | 라그랑주 $J$ → 성분 미분 → $f=e^{\alpha^\top t-\psi}f_0$, $\psi=1-\alpha_0$ | [9](#9-최대-엔트로피와-지수족) |
| ☐ | **hw1 #3** | logit, $A=n\log(1+e^\eta)$, 공간 $\mathbb R$ (합 유한) | [hw1 #3](#hw1-3-이항분포의-지수족-표현) |
| ☐ | **hw1 #5** | 전개 → $\eta=\Sigma^{-1}\mu$, $\Lambda=-\frac12\Sigma^{-1}$, trace → $A=-\frac14\eta^\top\Lambda^{-1}\eta-\frac12\log\lvert-2\Lambda\rvert$ → 공간(적분) → 곱해서 충분통계량 | [hw1 #5](#hw1-5-2변량-정규분포의-지수족-표현) |
| ☐ | 지수족이 아님 (보충) | support = $\lbrace h > 0\rbrace$, 모수와 무관 | [10](#10-보충-지수족인지-판정하기) |

### 짧은 문답

<details><summary>1. 강의노트의 A와 hw의 A(η)는 같은 것인가?</summary>

아니다. 강의의 $A$는 자연모수공간(집합), hw의 $A(\eta)$는 log-partition(함수) = 강의의 $\psi$.

</details>
<details><summary>2. 지수족 정의에서 빠뜨리기 쉬운 조건은?</summary>

support가 모수에 의존하지 않는다.

</details>
<details><summary>3. 감마의 자연모수공간과 그 이유는?</summary>

$\alpha_1 < 0$ (무한대 쪽 적분), $\alpha_2 > -1$ (0 근처 적분).

</details>
<details><summary>4. ψ가 볼록인 이유 두 가지는?</summary>

① 헤시안 = $\mathop{\mathrm{Cov}}\nolimits(t)\succeq0$ ② Hölder.

</details>
<details><summary>5. 베타–베르누이 사후분포는?</summary>

$\text{Be}(a+s,\ b+n-s)$, $s=\sum X_i$.

</details>
<details><summary>6. 로지스틱 회귀의 음의 로그우도는 어떤 손실과 같은가?</summary>

이진 교차엔트로피 (BCE).

</details>
<details><summary>7. hw1 #5에서 Λ가 대칭이어야 하는 이유는?</summary>

$T_2=xx^\top$가 대칭이라 $\Lambda$의 비대칭 부분이 밀도에 나타나지 않는다 → 일대일 모수화를 위해 대칭으로 제한.

</details>

---

## 강의노트 확인

강의노트 11쪽을 텍스트로 전부 읽고 표 2.1과 p5는 이미지로 확인했다. 수식 오류는 찾지 못했다. 철자 오타(covex)는 조용히 고쳤다.

| 쪽 | 강의노트 원문 | 노트에 반영한 것 | 근거 | 영향 |
| --- | --- | --- | --- | --- |
| p4–5 | 자연모수공간을 $A$로 표기 | hw의 $A(\eta)$ (log-partition)와 다른 뜻임을 기호 대응표에 명시 | hw1 기호와 충돌 | ★ (헷갈림) |
| p6 Thm 2.1 증명 | 적분과 미분의 교환을 언급하지 않음 | "자연모수공간의 내부에서 허용" (보충) | 정당화 | · |
| p8–9 | $\tilde\alpha_0=\alpha_0+\log n$, $e^{-\alpha_0}=\sum_ke^{\alpha^\top t(x_{(k)})}$ | 그대로 | $\log\mathbb E y_k=\log n-\log\sum_je^{\alpha^\top t(x_{(j)})}+\alpha^\top t(x_{(k)})$로 확인 | · |
| p2 표 2.1 | 감마는 척도 모수 $\sigma$ | 그대로 + ⚠ 비율 모수와 구별 | 평균 $\sigma\nu$ | · |
