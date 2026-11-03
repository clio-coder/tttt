# ch5.6 MLE의 점근이론 II (점근정규성과 정보부등식)

> **원본:** `수업자료/lecture_ch5.6_MLE Asymptotics 2.pdf` (9강, 7쪽). 출처 태그는 `[9강 p3]`.
> **수업:** 강의계획서상 10/8 "MLE의 점근이론 II" (강의자료는 10/16 판) → **중간고사 2 (11/16) 범위의 첫 부분.**
> **8강과의 관계:** 9강은 8강 ([ch5.3–5.5 노트](ch5.3-5.5%20최대가능도추정.md) §6–§7)의 후반부를 **다시 정리하고 넓힌 강의**다. 정리 번호가 바뀌었다: 8강 Thm 8.2 → 9강 **Thm 9.1** (점근정규성), 8강 Thm 9.1 → 9강 **Thm 9.2** (Cramér–Rao). 새로 생긴 것은 Cramér–Rao 증명의 세부 (지배조건, Cauchy–Schwarz에 $a=V^{-1}c$ 대입, $c=0$인 경우), 벡터 통계량 판, Ex 9.5 ( $p(1-p)$ 추정), Ex 9.7 (Beta 모형의 편향된 MLE)이다. 이 노트만 봐도 되도록 8강과 겹치는 내용도 다시 썼다.
> **관련 숙제:** hw3에는 9강만의 문제가 없다. hw3 #8 ( $aS^2$ 꼴의 MSE)이 Ex 9.6의 $\mathrm{Var}(S^2)$를 일반 분포로 넓힌 문제라서 [ch6 노트의 hw3 #8](ch6.1-6.3%20최소제곱·베이즈·UMVUE.md#hw3-8-as2-꼴-추정량의-mse와-첨도)에 풀었다.
> **범례:** 📝 hw에 나옴 · 🧩 강의노트에 증명 없음 → 보충 증명 · 📐 강의노트에 증명 있음 · 🔢 계산 · 💡 개념 · (보충) 강의노트에 없는 내용 · `[T…]` = [도구상자](00%20문제풀이%20도구상자.md) 카드
> **기호:** 한 관측치 로그밀도 $\ell(\theta;x)=\log f_\theta(x)$, 점수 $s(\theta;x)=\nabla_\theta\ell(\theta;x)$, 표본 점수 $S_n(\theta)=\sum_is(\theta;X_i)$, 한 관측치 Fisher 정보 $I(\theta)=I_1(\theta)$ (9강은 두 기호를 섞어 쓴다), 참값 $\theta_0$, " $\dot\sim$" = 큰 표본 근사분포.
> **더 연습하려면:** 수업 외 자료인 [튜터링2](튜터링/튜터링2%20최대가능도추정량의%20점근성질.md) (정보량·극한분포 8문제), [튜터링9](튜터링/튜터링9%20완비통계량과%20크래머라오%20부등식.md) (크래머–라오 부분).

---

## 0. 한눈에

### 0.1 범위

| 쪽 | 절 | 내용 | 증명 | 8강 대응 |
| --- | --- | --- | --- | --- |
| p1 | 9.1 | 점수와 Fisher 정보, 정칙조건 1–5, **Thm 9.1 점근정규성** | – | Thm 8.2 |
| p2 | 9.1 | Thm 9.1 증명 ( $d=1$ + 다변량 설명), Remark (경계·비정칙), Ex 9.1 베르누이 | 📐 | 같음 |
| p3 | 9.1 | Ex 9.2 지수 (율모수), **Ex 9.3 균등분포 반례** | 🔢 | Ex 8.3, 8.4 |
| p3–4 | 9.2 | 정칙조건 (R1)–(R5), Bartlett 항등식 (9.3) | 🧩 | (R0)–(R4) |
| p4–6 | 9.2 | **Thm 9.2 Cramér–Rao** 증명 (지배조건 → 공분산 = 기울기 → Cauchy–Schwarz), 벡터 통계량, $g(\theta)$의 불편추정 | 📐 | Thm 9.1 (증명이 훨씬 자세해짐) |
| p6–7 | 9.2 | Ex 9.4 포아송, **Ex 9.5 베르누이와 $p(1-p)$**, Ex 9.6 정규, **Ex 9.7 Beta $(\alpha,1)$** | 🔢 | 9.5·9.7은 새로 추가 |

### 0.2 시험 포인트 (출제 가능성 순)

근거: 시험은 hw처럼 유도·증명형 → 강의노트에 증명이 있는 정리 > 증명 없이 쓴 결과 > 예제. hw3에 9강 문제가 없어서 강의노트 구조로 판단했다. **9강은 8강과 같은 내용을 증명 중심으로 다시 썼다는 것 자체가 신호**다.

**★★★ 매우 유력**

1. 📐 **Thm 9.1 점근정규성 증명** — 점수 Taylor → 분모 LLN → 분자 CLT → Slutsky, 그리고 각 정칙조건이 어느 줄에 쓰이는지. → [2](#2-thm-91-점근정규성)
2. 📐 **Thm 9.2 Cramér–Rao 증명** — $\mathbb ES_n=0$ → $\mathrm{Cov}(S_n,U_n)=\nabla\mathbb EU_n$ → Cauchy–Schwarz에 $a=V^{-1}c$. 9강이 줄 단위로 새로 쓴 증명이다. → [5](#5-thm-92-cramérrao-정보부등식)
3. 🔢 **정보량 → 극한분포 → CR 하한 → 달성 여부** 한 세트 (Ex 9.4–9.7). 특히 함수형 모수 $\eta=g(\theta)$: 불변성으로 MLE, 델타 방법으로 극한분포, $g'(\theta)^2/(nI_1)$ 하한, 편향 보정. → [6](#6-예-정보량-하한-달성-여부)

**★★ 유력**

4. 🧩 **Bartlett 항등식 증명** (9강은 "성립한다"고만 썼다). → [4](#4-정칙조건과-bartlett-항등식)
5. 🔢 균등분포 반례: $\Pr\lbrace n(\theta-\hat\theta)\gt t\rbrace$ 계산, $\sqrt n(\hat\theta-\theta)\xrightarrow{p}0$, 왜 정칙조건이 깨지는지. → [3.3](#33-ex-93-균등분포-반례)
6. 🧩 정규모형의 정보행렬 계산, Beta 모형 MLE의 정확한 평균·분산 (역감마 적률). → [6.3](#63-ex-96-정규모형), [6.4](#64-ex-97-beta-모형의-편향된-mle)
7. 💡 다변량 판: 벡터 Taylor (행마다 다른 중간점), 벡터 통계량의 CR (야코비 행렬의 방향). → [2.3](#23-다변량-판), [5.3](#53-벡터-통계량과-함수형-모수)

**★ 가능성 있음**

8. 💡 미분·적분 교환의 충분조건 (지배함수), (보충) CR 등호 조건 → 지수족, $p=1/2$에서 하한이 0이 되는 현상.

### 0.3 이 장의 도구

| 도구 | 쓰는 곳 |
| --- | --- |
| [T27] **점수의 Taylor 전개 → LLN + CLT + Slutsky** | Thm 9.1, V3 |
| [T13] 정규화 항등식 $\int f=1$ 미분 | Bartlett 항등식, $\mathbb ES_n=0$ |
| [T29] (신규) **CR 틀:** 점수와의 공분산 = 평균의 기울기 → Cauchy–Schwarz | Thm 9.2, Ex 9.4–9.7, V4, V6 |
| [T30] (신규) **감마 역적률:** $G\sim\mathrm{Gamma}(n,1)$이면 $\mathbb EG^{-k}=\Gamma(n-k)/\Gamma(n)$ | Ex 9.7 |
| [T18] CMT·Slutsky, 델타 방법 | 함수형 모수, V1, V2, V5 |
| [T15] 편향–분산 분해 | Ex 9.5, 9.7 |

---

## 1. 점수, Fisher 정보, 정칙조건

💡 [9강 p1]

- **점수 (score)** $s(\theta;X)=\nabla_\theta\ell(\theta;X)$, 표본 점수 $S_n(\theta)=\sum_{i=1}^ns(\theta;X_i)$.
- 정칙조건 아래 $\mathbb E_{\theta_0}[s(\theta_0;X)]=0$, 그리고 한 관측치의 **Fisher 정보행렬**

$$I(\theta_0)=\mathbb E_{\theta_0}\big[s(\theta_0;X)s(\theta_0;X)^\top\big]=-\mathbb E_{\theta_0}\big[\nabla^2_\theta\ell(\theta_0;X)\big].$$

- 표본 전체의 Fisher 정보는 $nI(\theta_0)$ (독립이라 분산이 더해진다).
- 점수는 **표본에 따라 변하는 확률벡터**, Fisher 정보는 그 분포를 요약한 **모수의 함수**다.

**Thm 9.1의 정칙조건과 쓰이는 곳**

| 조건 | 내용 | 증명에서 쓰는 곳 ([2.2](#22-증명-d1)) |
| --- | --- | --- |
| 1 | 모수 차원 $d$는 $n$과 무관하게 고정, $\theta_0$는 내부점 | ① $S_n(\hat\theta_n)=0$ |
| 2 | $\ell(\theta;x)$가 $\theta_0$ 근방에서 (예: 3차까지) 매끄럽고 미분값에 적분가능한 지배함수 | ② Taylor, ③ 분모의 수렴 |
| 3 | 미분·적분 교환 → $\mathbb Es=0$, 정보 등식 | ④ CLT의 평균 0·분산 $I$ |
| 4 | 식별 가능, MLE 존재, $\hat\theta_n\xrightarrow{p}\theta_0$ | ①, ③ |
| 5 | $I(\theta_0)$ 양정치 | ⑤ 역행렬 |

---

## 2. Thm 9.1 점근정규성

### 2.1 진술

📐 [9강 p1]

**가정** 위 정칙조건 1–5.
**결론**

$$\sqrt n(\hat\theta_n-\theta_0)\xrightarrow{d}N_d\big(0,I(\theta_0)^{-1}\big),\qquad\text{즉}\quad\hat\theta_n\ \dot\sim\ N_d\Big(\theta_0,\frac{I(\theta_0)^{-1}}n\Big).$$

**핵심 한 수** 점수 방정식 $S_n(\hat\theta)=0$을 $\theta_0$ 중심으로 1차 전개 → $\sqrt n(\hat\theta-\theta_0)$ = (평균 헤시안)⁻¹ × (정규화 점수). [T27]

### 2.2 증명 d=1

📐 [9강 p2] (③의 지배조건 부분은 보충)

① **점수 = 0.** 일치성 ( $\hat\theta_n\xrightarrow{p}\theta_0$)과 $\theta_0$가 내부점이라는 조건 때문에, 확률이 1로 가는 사건에서 $\hat\theta_n$은 경계에 있지 않다. 내부 최대점에서는 도함수가 0이므로 $S_n(\hat\theta_n)=0$.

② **Taylor (평균값 정리).** $\theta_0$와 $\hat\theta_n$ 사이의 $\tilde\theta_n$이 있어서

$$0=S_n(\hat\theta_n)=S_n(\theta_0)+(\hat\theta_n-\theta_0)S_n'(\tilde\theta_n)\quad\Longrightarrow\quad\sqrt n(\hat\theta_n-\theta_0)=\Big\lbrace-\frac1nS_n'(\tilde\theta_n)\Big\rbrace^{-1}\frac1{\sqrt n}S_n(\theta_0).\qquad(9.2)$$

③ **분모 → $I(\theta_0)$.** $-\frac1nS_n'(\tilde\theta_n)=\frac1n\sum_i\lbrace-\ell''(\tilde\theta_n;X_i)\rbrace$. 이것을 두 조각으로 나눈다.
- $\frac1n\sum_i\lbrace-\ell''(\theta_0;X_i)\rbrace\xrightarrow{p}-\mathbb E\ell''(\theta_0;X)=I(\theta_0)$ (대수의 법칙 + 정보 등식).
- (보충) 나머지: $\theta_0$ 근방에서 $\lvert\ell'''(\theta;x)\rvert\le M(x)$, $\mathbb EM(X) < \infty$ (조건 2의 지배함수)이면 평균값 정리로 $\big\lvert\frac1n\sum\lbrace\ell''(\tilde\theta_n;X_i)-\ell''(\theta_0;X_i)\rbrace\big\rvert\le\frac1n\sum M(X_i)\cdot\lvert\tilde\theta_n-\theta_0\rvert$. 앞 인수는 LLN으로 $\mathbb EM$에 수렴 (유계), 뒤 인수는 $\lvert\tilde\theta_n-\theta_0\rvert\le\lvert\hat\theta_n-\theta_0\rvert\xrightarrow{p}0$. 곱은 $\xrightarrow{p}0$.
- 합치면 $-\frac1nS_n'(\tilde\theta_n)\xrightarrow{p}I(\theta_0)$.

④ **분자 → $N(0,I)$.** $s(\theta_0;X_i)$는 iid, 평균 0, 분산 $I(\theta_0)$ (조건 3) → CLT로 $\frac1{\sqrt n}S_n(\theta_0)\xrightarrow{d}N(0,I(\theta_0))$.

⑤ **Slutsky.** $I(\theta_0) > 0$ (조건 5)이라 $x\mapsto1/x$가 $I(\theta_0)$에서 연속 → 분모의 역수 $\xrightarrow{p}I(\theta_0)^{-1}$. Slutsky로

$$\sqrt n(\hat\theta_n-\theta_0)\xrightarrow{d}I(\theta_0)^{-1}N\big(0,I(\theta_0)\big)=N\big(0,I(\theta_0)^{-2}I(\theta_0)\big)=N\big(0,I(\theta_0)^{-1}\big).\qquad\blacksquare$$

- **점근 선형성:** (9.2)는 $\hat\theta_n-\theta_0\approx I(\theta_0)^{-1}\frac1n\sum s(\theta_0;X_i)$라는 뜻이다. MLE가 표본평균이 아니어도 **점수의 평균처럼** 행동한다 (9강 p2의 말).

### 2.3 다변량 판

💡 [9강 p2] + (보충) 세부

- 벡터 함수에는 한 점짜리 평균값 정리가 없다. **성분마다** 평균값 정리를 쓴다: $j$번째 성분 $\partial_j\ell_n$에 대해 $\theta_0$와 $\hat\theta_n$ 사이의 $\tilde\theta^{(j)}$가 있어 $0=\partial_j\ell_n(\theta_0)+\nabla\partial_j\ell_n(\tilde\theta^{(j)})^\top(\hat\theta_n-\theta_0)$.
- 행 $j$가 $\nabla\partial_j\ell_n(\tilde\theta^{(j)})^\top$인 행렬을 $\bar H_n$이라 하면 $0=S_n(\theta_0)+\bar H_n(\hat\theta_n-\theta_0)$.
- 모든 $\tilde\theta^{(j)}\xrightarrow{p}\theta_0$이므로 $-\frac1n\bar H_n\xrightarrow{p}I(\theta_0)$ (성분별로 ③과 같은 논증), $\frac1{\sqrt n}S_n(\theta_0)\xrightarrow{d}N_d(0,I(\theta_0))$ (다변량 CLT).
- Slutsky: $\sqrt n(\hat\theta_n-\theta_0)=\big(-\frac1n\bar H_n\big)^{-1}\frac1{\sqrt n}S_n\xrightarrow{d}N_d\big(0,I^{-1}II^{-1}\big)=N_d(0,I^{-1})$. (양정치라 역행렬이 존재하고 역행렬 함수가 연속)

### 2.4 가정이 빠지면

| 빠진 가정 | 무엇이 깨지나 | 예 |
| --- | --- | --- |
| $\theta_0$ 내부점 | ① 점수가 0일 필요가 없다 | (보충) $N(\theta,1)$, $\theta\ge0$, $\theta_0=0$: $\hat\theta=\max(\bar X,0)$이고 $\sqrt n\hat\theta\xrightarrow{d}\max(Z,0)$ — 정규가 아니다 |
| 지지집합이 $\theta$와 무관 | 미분 불가능, $\mathbb Es\ne0$ | Ex 9.3 균등분포 ( $n$ 속도, 지수 극한) |
| 일치성 ( $d$ 고정) | ①·③ | Neyman–Scott (모수 수가 $n$과 함께 증가, [ch5.3–5.5 5.3](ch5.3-5.5%20최대가능도추정.md#53-반례와-모형-오설정)) |
| $I(\theta_0)$ 양정치 | ⑤ 역행렬이 없다 | 식별 불가능한 모수 조합 |

**Remark (9강 p2)** 경계에서 최대가 되거나 지지집합이 모수에 따라 바뀌면 위 전개가 그대로 적용되지 않는다. **점근정규성은 MLE의 정의만으로 보장되는 성질이 아니다.**

**자주 하는 실수**
- $\sqrt n$ 스케일과 근사분포를 섞어 쓰기: $\sqrt n(\hat\theta-\theta)\to N(0,I^{-1})$이면 $\hat\theta\ \dot\sim\ N(\theta,I^{-1}/n)$. **정보는 한 관측치 $I_1$, 분산에는 $1/n$**.
- ①에서 "확률 → 1인 사건에서"를 빼먹기.
- 다변량에서 한 점 $\tilde\theta$로 벡터 Taylor를 쓰기 (2.3).
- " $\dot\sim N$"을 유한 표본에서 정확히 정규라는 뜻으로 쓰기.

**셀프 체크**
<details><summary>Q1. 분모가 I(θ₀)로 가는 데 필요한 세 가지는?</summary>

대수의 법칙 (평균 → 기댓값), $\tilde\theta_n\xrightarrow{p}\theta_0$ (일치성), 2계도함수의 연속성을 보장하는 지배조건 (3차 도함수를 $M(x)$로 누르기). 정보 등식으로 $-\mathbb E\ell''=I$를 쓴다.

</details>
<details><summary>Q2. 극한분산이 I⁻¹·I·I⁻¹에서 I⁻¹로 줄어드는 이유는?</summary>

"분모"의 극한 (헤시안의 음의 기댓값)과 "분자"의 분산 (점수의 분산)이 **같은 행렬** $I$이기 때문이다 (정보 등식). 모형이 틀리면 두 행렬이 달라져 샌드위치 $A^{-1}BA^{-1}$이 남는다.

</details>

---

## 3. 예와 반례

### 3.1 Ex 9.1 베르누이

🔢 [9강 p2]

$\hat p=\bar X_n$, CLT로 $\sqrt n(\hat p-p)\xrightarrow{d}N(0,p(1-p))$, 표준화하면 $\frac{\sqrt n(\hat p-p)}{\sqrt{p(1-p)}}\xrightarrow{d}N(0,1)$ ( $0 < p < 1$). $I_1(p)=\frac1{p(1-p)}$이므로 Thm 9.1과 일치한다.
- (보충) 분모의 $p$를 $\hat p$로 바꿔도 $\sqrt{\hat p(1-\hat p)}\xrightarrow{p}\sqrt{p(1-p)}$ (CMT)라 Slutsky로 극한이 같다 → 근사 신뢰구간 $\hat p\pm z_{\alpha/2}\sqrt{\hat p(1-\hat p)/n}$.

### 3.2 Ex 9.2 지수분포 (율모수)

🔢 [9강 p3]

$p_\theta(x)=\theta e^{-\theta x}$ ( $x\ge0$, $\theta > 0$). $\ell_n(\theta)=n\log\theta-\theta\sum X_i$, 점수 $\frac n\theta-\sum X_i=0$ → $\hat\theta=1/\bar X_n$ ( $\ell_n''=-n/\theta^2 < 0$이라 최대).
- 한 관측치 점수 $\frac1\theta-X$, $I(\theta)=\mathbb E(\frac1\theta-X)^2=\mathrm{Var}(X)=\frac1{\theta^2}$ (또는 $-\ell''=\frac1{\theta^2}$).
- 따라서 $\sqrt n(\hat\theta-\theta)\xrightarrow{d}N(0,\theta^2)$, $\hat\theta\ \dot\sim\ N(\theta,\theta^2/n)$.
- (보충) **델타 방법으로 검산:** $\sqrt n(\bar X-\frac1\theta)\xrightarrow{d}N(0,\frac1{\theta^2})$, $g(x)=1/x$, $g'(1/\theta)=-\theta^2$ → 분산 $\theta^4\cdot\frac1{\theta^2}=\theta^2$ ✓. 닫힌 꼴 MLE가 있으면 두 방법이 같은 답을 줘야 한다.

### 3.3 Ex 9.3 균등분포 반례

🔢 [9강 p3]

$X_i\overset{\text{iid}}{\sim}\mathrm{Uniform}(0,\theta)$: $L_n(\theta)=\theta^{-n}I(\theta\ge X_{(n)})$ → $\theta\ge X_{(n)}$에서 감소하므로 $\hat\theta=X_{(n)}$.

$0\le t\le n\theta$이면

$$\Pr\lbrace n(\theta-\hat\theta)\gt t\rbrace=\Pr\lbrace X_{(n)}\lt \theta-t/n\rbrace=\prod_{i=1}^n\Pr\lbrace X_i\lt \theta-t/n\rbrace=\Big(1-\frac t{n\theta}\Big)^n\longrightarrow e^{-t/\theta}.$$

- 줄마다: 최댓값이 작다 ⟺ 모두 작다 → 독립 → 균등 CDF → $(1+x/n)^n\to e^x$.
- 결론: $n(\theta-\hat\theta)\xrightarrow{d}\mathrm{Exp}(\text{평균 }\theta)$, 즉 $n(\hat\theta-\theta)$는 **지수분포의 음수**로 수렴한다.
- $\sqrt n(\hat\theta-\theta)=\frac1{\sqrt n}\cdot n(\hat\theta-\theta)$ = ( $\to0$) × (분포수렴, 유계 확률) → Slutsky로 $\xrightarrow{p}0$. **비퇴화 정규분포로 수렴하지 않는다.** 수렴 속도도 $\sqrt n$이 아니라 $n$이다.
- **왜 깨지나:** 로그가능도가 추정값에서 미분 불가능하고 (① 실패), 지지집합이 $\theta$에 의존한다. (보충) 실제로 $0 < x < \theta$에서 $s(\theta;x)=\partial_\theta\log\frac1\theta=-\frac1\theta$이므로 $\mathbb Es=-\frac1\theta\ne0$ — Bartlett 항등식부터 성립하지 않는다.

---

## 4. 정칙조건과 Bartlett 항등식

🧩 [9강 p3–4]

**(R1)** 식별 가능 · **(R2)** $f(x;\theta)$의 지지집합이 $\theta$와 무관 · **(R3)** $\Theta$는 $\mathbb R^d$의 열린 집합 · **(R4)** $\log f(x;\theta)$가 $\theta$에 대해 두 번 미분 가능 · **(R5)** 필요한 기댓값을 적분 아래에서 두 번 미분 가능.

편리한 충분조건 모음이지 모든 결과에 필요한 최소조건은 아니다. (9강 p4는 "(R1)과 (R4)가 Bartlett 항등식을 정당화한다"고 썼는데 9강 번호로는 **(R2)·(R5)**다 ✏️ — [확인 표](#강의노트-확인))

**Bartlett 항등식 (9.3)** (R2)–(R5) 아래 $s_1(\theta;X)=\nabla_\theta\log f(X;\theta)$에 대해

$$\mathbb E_\theta[s_1(\theta;X)]=0,\qquad I_1(\theta)=\mathbb E_\theta\big[s_1s_1^\top\big]=-\mathbb E_\theta\big[\nabla^2_\theta\log f(X;\theta)\big].$$

**🧩 보충 증명** [T13]

(a) **평균 0.** 모든 $\theta$에서 $\int_{\mathcal X}f(x;\theta)dx=1$. 지지집합 $\mathcal X$가 $\theta$와 무관하므로 (R2) 적분 구간이 고정되어 있고, (R5)로 미분을 적분 안에 넣는다: $0=\int\nabla_\theta f\thinspace dx$. $\mathcal X$ 위에서 $f > 0$이라 $\nabla f=f\nabla\log f=fs_1$ → $0=\int s_1f\thinspace dx=\mathbb E_\theta s_1$.

(b) **정보 등식.** (a)의 항등식 $\int s_1(\theta;x)f(x;\theta)dx=0$을 다시 $\theta^\top$로 미분하면 (R4)·(R5), 곱의 미분으로

$$0=\int\big\lbrace\nabla^2_\theta\log f\cdot f+s_1(\nabla_\theta f)^\top\big\rbrace dx=\int\big\lbrace\nabla^2_\theta\log f+s_1s_1^\top\big\rbrace f\thinspace dx .$$

따라서 $\mathbb E[s_1s_1^\top]=-\mathbb E[\nabla^2_\theta\log f]$.

(c) **정보 = 점수의 분산, 표본 정보 = $n$배.** (a)로 $\mathbb Es_1=0$이므로 $\mathbb E[s_1s_1^\top]=\mathrm{Var}(s_1)$. $S_n=\sum s_1(\theta;X_i)$는 독립 항의 합이라 $\mathrm{Var}(S_n)=nI_1(\theta)$. $\blacksquare$

- **검산 (포아송):** $s=-1+x/\lambda$ → $\mathbb Es=0$ ✓, $\mathrm{Var}(s)=\mathrm{Var}(X)/\lambda^2=1/\lambda$, $-\mathbb E\ell''=\mathbb EX/\lambda^2=1/\lambda$ ✓.
- **깨지는 예:** 균등 $(0,\theta)$는 (R2)가 깨져 $\mathbb Es=-1/\theta$ ([3.3](#33-ex-93-균등분포-반례)). 적분 구간 $(0,\theta)$가 $\theta$에 의존해서 미분할 때 경계항 $f(\theta;\theta)=1/\theta$가 생기기 때문이다 (라이프니츠 규칙).

---

## 5. Thm 9.2 Cramér–Rao 정보부등식

### 5.1 진술

📐 [9강 p4]

**가정** $I_1(\theta)$ 가역, $U_n$은 이차모멘트가 유한한 스칼라 통계량, 미분·적분 교환 가능.
**결론**

$$\mathrm{Var}_ \theta(U_n)\ \ge\ \lbrace\nabla_\theta\mathbb E_\theta U_n\rbrace^\top\big(nI_1(\theta)\big)^{-1}\lbrace\nabla_\theta\mathbb E_\theta U_n\rbrace .\qquad(9.4)$$

- $\mathbb E_\theta U_n=g(\theta)$이면 우변은 $\nabla g(\theta)^\top(nI_1(\theta))^{-1}\nabla g(\theta)$.
- 1차원, $\theta$의 불편추정량이면 $\mathrm{Var}\ge\frac1{nI_1(\theta)}$. 편향 $b(\theta)$가 있으면 $\mathrm{Var}\ge\frac{(1+b'(\theta))^2}{nI_1(\theta)}$.
- 9강은 스칼라에도 $\succeq$ 기호를 쓴다 (스칼라에서는 $\ge$와 같다).

**핵심 한 수** "점수와의 공분산 = 평균의 기울기"를 만든 뒤 Cauchy–Schwarz. [T29]

### 5.2 증명

📐 [9강 p4–6]

① **결합밀도.** $f^{(n)}_ \theta(x)=\prod_{i=1}^nf_\theta(x_i)$, $\mathbb E_\theta U_n=\int U_n(x)f^{(n)}_ \theta(x)dx$.

② **미분을 적분 안으로.** 충분조건 (지배수렴정리): 모수의 근방 $V$와 함수 $G_j$가 있어

$$\sup_{\vartheta\in V}\Big\lvert U_n(x)\frac{\partial}{\partial\vartheta_j}f^{(n)}_ \vartheta(x)\Big\rvert\le G_j(x),\qquad\int G_j(x)dx\lt \infty .$$

그러면 $\nabla_\theta\mathbb E_\theta U_n=\int U_n\nabla_\theta f^{(n)}_ \theta dx=\int U_nf^{(n)}_ \theta\nabla_\theta\log f^{(n)}_ \theta dx=\mathbb E_\theta[U_nS_n]$. ( $\nabla\log f^{(n)}=\sum_i\nabla\log f_\theta(x_i)=S_n$)

③ **점수 평균 0.** 같은 교환을 $\int f^{(n)}_ \theta dx=1$에 쓰면 $\mathbb E_\theta S_n=\nabla_\theta1=0$. (② 에서 $U_n\equiv1$인 경우)

④ **공분산 = 기울기.** ③ 때문에

$$c:=\mathrm{Cov}_ \theta(S_n,U_n)=\mathbb E_\theta\big[S_n\lbrace U_n-\mathbb E_\theta U_n\rbrace\big]=\mathbb E_\theta[S_nU_n]=\nabla_\theta\mathbb E_\theta U_n .$$

( $U_n$이 $\theta$에 명시적으로 의존하지 않는 **통계량**이라서 ②의 미분이 밀도에만 걸린다.)

⑤ **Cauchy–Schwarz.** $Y=U_n-\mathbb E_\theta U_n$, $V=\mathrm{Var}_ \theta(S_n)=nI_1(\theta)$ (양정치, [4](#4-정칙조건과-bartlett-항등식) (c)). 임의의 $a\in\mathbb R^d$에 대해 $a^\top S_n$은 스칼라이므로

$$\big\lbrace\mathbb E_\theta[Y(a^\top S_n)]\big\rbrace^2\le\mathbb E_\theta[Y^2]\thinspace\mathbb E_\theta[(a^\top S_n)^2].$$

$\mathbb ES_n=0$이라 $\mathbb E[Y(a^\top S_n)]=a^\top c$, $\mathbb EY^2=\mathrm{Var}(U_n)$, $\mathbb E(a^\top S_n)^2=a^\top Va$. 따라서 $(a^\top c)^2\le\mathrm{Var}(U_n)\thinspace a^\top Va$.

⑥ **$a=V^{-1}c$ 대입.** $a^\top c=c^\top V^{-1}c$, $a^\top Va=c^\top V^{-1}VV^{-1}c=c^\top V^{-1}c$ → $(c^\top V^{-1}c)^2\le\mathrm{Var}(U_n)\thinspace c^\top V^{-1}c$.

⑦ **나누기.** $c\ne0$이면 $V^{-1}$이 양정치라 $c^\top V^{-1}c > 0$ → 양변을 나눠 $\mathrm{Var}(U_n)\ge c^\top V^{-1}c$. $c=0$이면 우변이 0이라 바로 성립.

⑧ $V=nI_1(\theta)$, $c=\nabla_\theta\mathbb E_\theta U_n$을 넣으면 (9.4). $\blacksquare$

- **왜 $a=V^{-1}c$?** $(a^\top c)^2/(a^\top Va)$를 최대로 만드는 방향 = $Y$를 $S_n$으로 회귀한 계수. 같은 증명을 "회귀 잔차의 분산 ≥ 0"으로 쓰면 $0\le\mathrm{Var}(U_n-c^\top V^{-1}S_n)=\mathrm{Var}(U_n)-c^\top V^{-1}c$ (8강 노트의 방법).

### 5.3 벡터 통계량과 함수형 모수

📐 [9강 p6]

- $U_n\in\mathbb R^m$, $g(\theta)=\mathbb E_\theta U_n$, $\nabla g$ = $m\times d$ **야코비 행렬**. 임의의 $b\in\mathbb R^m$에 대해 스칼라 $b^\top U_n$에 (9.4)를 쓰면 $b^\top\mathrm{Var}(U_n)b\ge b^\top\nabla g(nI_1)^{-1}\nabla g^\top b$ → $\mathrm{Var}_ \theta(U_n)\succeq\nabla g(\theta)(nI_1(\theta))^{-1}\nabla g(\theta)^\top$.
- **전치 위치 주의:** 스칼라 판의 $\nabla g$는 $d\times1$ 열벡터라 $\nabla g^\top(nI_1)^{-1}\nabla g$, 벡터 판의 $\nabla g$는 $m\times d$라 $\nabla g(nI_1)^{-1}\nabla g^\top$.
- $g(\theta)$의 불편추정량 $\hat\eta$: $\mathrm{Var}_ \theta(\hat\eta)\ge\nabla g^\top(nI_1)^{-1}\nabla g$. **이 하한은 모든 모형·추정량에서 달성되지는 않는다.**
- 2모수 이상에서 한 성분 $\theta_k$의 하한은 $[I_1^{-1}]_ {kk}/n$이지 $1/(n[I_1]_ {kk})$가 아니다 (부수모수를 모를 때의 정보 손실). 정규모형은 $I_1$이 대각이라 둘이 같다.

### 5.4 등호 조건

(보충, 9강 p6 "달성되지 않는다"의 이유)

- Cauchy–Schwarz의 등호 ⟺ $Y$가 $a^\top S_n$의 상수배 (a.s.). $a=V^{-1}c$에서는 계수가 1이라 $U_n-g(\theta)=\nabla g^\top(nI_1)^{-1}S_n$.
- 1차원, $\theta$의 불편추정량이면 $\partial_\theta\log f^{(n)}_ \theta(x)=nI_1(\theta)\lbrace U_n(x)-\theta\rbrace$. 이것을 $\theta$로 적분하면 $\log f^{(n)}_ \theta=A(\theta)U_n(x)-B(\theta)+h(x)$ — **$U_n$을 충분통계량으로 하는 지수족**.
- 그래서 하한은 지수족에서 **평균모수의 1차식**을 추정할 때만 정확히 달성된다. Ex 9.5의 $p(1-p)$, Ex 9.6의 $\sigma^2$ ( $\mu$ 모를 때), V4의 $e^{-\lambda}$가 달성하지 못하는 이유다.

### 5.5 가정이 빠지면

- **지지집합이 $\theta$에 의존:** 균등 $(0,\theta)$에서 형식적으로 $I_1=\mathbb Es^2=1/\theta^2$을 넣은 "하한" $\theta^2/n$보다 불편추정량 $\frac{n+1}nX_{(n)}$의 분산 $\frac{\theta^2}{n(n+2)}$이 **더 작다** (V7, [ch6 Ex 12.7](ch6.1-6.3%20최소제곱·베이즈·UMVUE.md#c4-umvue를-찾는-두-방법과-예)). 미분·적분 교환이 깨졌기 때문이다.
- **$U_n$의 분산이 무한:** 부등식이 의미 없다 (자명하게 참).

**셀프 체크**
<details><summary>Q1. 증명에서 미분·적분 교환을 두 번 쓰는 곳은?</summary>

② $\nabla\mathbb EU_n=\mathbb E[U_nS_n]$ (밀도에 $U_n$을 곱한 적분)과 ③ $\mathbb ES_n=0$ (밀도만의 적분). 둘 다 지배함수 조건으로 정당화한다.

</details>
<details><summary>Q2. ③이 없으면 어디가 무너지나?</summary>

④에서 $\mathbb E[S_nU_n]$을 공분산으로 바꿀 수 없고, ⑤에서 $\mathbb E(a^\top S_n)^2$이 분산 $a^\top Va$가 아니게 된다.

</details>

---

## 6. 예 정보량 하한 달성 여부

### 6.1 Ex 9.4 포아송

🔢 [9강 p6]

$\ell_1(\lambda;x)=-\lambda+x\log\lambda-\log x!$, $\ell_1'=-1+\frac x\lambda$, $\ell_1''=-\frac x{\lambda^2}$ → $I_1(\lambda)=\frac{\mathbb EX}{\lambda^2}=\frac1\lambda$. 모든 불편추정량에 대해 $\mathrm{Var}_ \lambda(U_n)\ge\frac\lambda n$.
- MLE $\hat\lambda=\bar X_n$은 불편이고 $\mathrm{Var}(\bar X_n)=\lambda/n$ → **하한 달성** → UMVUE. (근거: 어떤 불편추정량의 분산도 하한 $=\mathrm{Var}(\bar X)$ 이상이므로)
- 하한을 달성하지 못하면 이 논증은 쓸 수 없고 Lehmann–Scheffé ([ch6 C3](ch6.1-6.3%20최소제곱·베이즈·UMVUE.md#c3-umvue-완비성-lehmannscheffé))로 간다.

### 6.2 Ex 9.5 베르누이와 분산 함수

🔢 [9강 p6–7] (🧩 기댓값 계산은 보충)

**$p$ 자체.** $\ell_1'(p;X)=\frac Xp-\frac{1-X}{1-p}$, $-\ell_1''=\frac X{p^2}+\frac{1-X}{(1-p)^2}$ → $I_1(p)=\frac1p+\frac1{1-p}=\frac1{p(1-p)}$. 하한 $\frac{p(1-p)}n$을 $\bar X_n$이 달성 → UMVUE.

**$\eta=p(1-p)$.** $g'(p)=1-2p$ → 불편추정량의 하한

$$\mathrm{Var}_ p(\hat\eta)\ge\frac{(1-2p)^2p(1-p)}n .$$

- **MLE (불변성):** $\hat\eta_{\mathrm{MLE}}=\bar X_n(1-\bar X_n)$.
- **🧩 편향:** $\mathbb E\bar X^2=\mathrm{Var}(\bar X)+p^2=\frac{p(1-p)}n+p^2$ → $\mathbb E\hat\eta_{\mathrm{MLE}}=p-p^2-\frac{p(1-p)}n=\frac{n-1}np(1-p)$. 유한 표본에서 편향되어 있고 $\tilde\eta=\frac n{n-1}\bar X_n(1-\bar X_n)$은 불편 (= [ch6 Ex 12.4의 UMVUE](ch6.1-6.3%20최소제곱·베이즈·UMVUE.md#c4-umvue를-찾는-두-방법과-예)).
- **분산 (9강 p7, 기호 계산으로 확인):**

$$\mathrm{Var}_ p(\hat\eta_{\mathrm{MLE}})=\frac{p(1-p)(1-2p)^2}n-\frac{2p(1-p)(1-2p)^2}{n^2}+\frac{2p^2(1-p)^2}{n^2}+\frac{p(1-p)\lbrace1-6p(1-p)\rbrace}{n^3},$$

$\mathrm{Var}(\tilde\eta)=\big(\frac n{n-1}\big)^2\mathrm{Var}(\hat\eta_{\mathrm{MLE}})$.
- **해석:** 첫 항이 정확히 하한이다. $\tilde\eta$의 분산은 하한보다 크지만 $p\ne\frac12$이면 비가 1로 간다 (점근적으로 효율적). 델타 방법으로도 $\sqrt n(\hat\eta-\eta)\xrightarrow{d}N(0,(1-2p)^2p(1-p))$.
- **$p=\frac12$:** 하한이 0이고 1차 근사가 사라진다. (보충) $\hat\eta-\frac14=-(\bar X-\frac12)^2$이고 $\sqrt n(\bar X-\frac12)\xrightarrow{d}N(0,\frac14)$이므로 $n(\hat\eta-\frac14)\xrightarrow{d}-\frac14\chi^2_1$ — 속도가 $n$이고 정규가 아니다. 1차 근사만으로는 효율을 판단할 수 없다.

### 6.3 Ex 9.6 정규모형

🔢 [9강 p7] (🧩 정보행렬 계산은 보충)

$\theta=(\mu,v)$, $v=\sigma^2$, $\ell=-\frac12\log(2\pi v)-\frac{(x-\mu)^2}{2v}$.
- 1계: $\partial_\mu\ell=\frac{x-\mu}v$, $\partial_v\ell=-\frac1{2v}+\frac{(x-\mu)^2}{2v^2}$.
- 2계: $\partial_\mu^2\ell=-\frac1v$, $\partial_\mu\partial_v\ell=-\frac{x-\mu}{v^2}$, $\partial_v^2\ell=\frac1{2v^2}-\frac{(x-\mu)^2}{v^3}$.
- 음의 기댓값: $\frac1v$, $0$, $-\frac1{2v^2}+\frac v{v^3}=\frac1{2v^2}$ → $I_1(\theta)=\mathrm{diag}\big(\frac1{\sigma^2},\frac1{2\sigma^4}\big)$.

하한: $\mu$는 $\frac{\sigma^2}n$ ( $\bar X$가 달성), $\sigma^2$은 $\frac{2\sigma^4}n$. 불편 $S^2=\frac1{n-1}\sum(X_i-\bar X)^2$은 $(n-1)S^2/\sigma^2\sim\chi^2_{n-1}$ (분산 $2(n-1){}$)이라 $\mathrm{Var}(S^2)=\frac{2\sigma^4}{n-1} > \frac{2\sigma^4}n$ — **달성하지 못한다**.
- 그런데 $S^2$은 UMVUE다 ( $(\bar X,S^2)$이 완비충분, ch6). 즉 **CR 하한 ≠ 최선의 불편추정량 분산**일 수 있다.
- (보충) $\mu$를 알면 $\frac1n\sum(X_i-\mu)^2$의 분산이 $\frac{2\sigma^4}n$으로 하한을 달성한다 ( $\sum(X_i-\mu)^2/\sigma^2\sim\chi^2_n$).

### 6.4 Ex 9.7 Beta 모형의 편향된 MLE

🔢 [9강 p7] (🧩 분포·적률 계산은 보충)

$f(x;\alpha)=\alpha x^{\alpha-1}$ ( $0 < x < 1$). $\ell_1'=\frac1\alpha+\log x$, $-\ell_1''=\frac1{\alpha^2}$ → $I_1=\frac1{\alpha^2}$, 불편추정량의 하한 $\frac{\alpha^2}n$.

- **MLE:** $\ell_n=n\log\alpha+(\alpha-1)\sum\log X_i$ → $\hat\alpha=-\frac n{\sum\log X_i}$.
- **🧩 분포:** $Y_i=-\alpha\log X_i$이면 $\Pr(Y_i > y)=\Pr(X_i\lt e^{-y/\alpha})=(e^{-y/\alpha})^\alpha=e^{-y}$ → $Y_i\overset{\text{iid}}{\sim}\mathrm{Exp}(1)$, $G=\sum Y_i\sim\mathrm{Gamma}(n,1)$, $\hat\alpha=n\alpha/G$.
- **🧩 역감마 적률 [T30]:** $\mathbb EG^{-k}=\frac1{\Gamma(n)}\int_0^\infty y^{n-k-1}e^{-y}dy=\frac{\Gamma(n-k)}{\Gamma(n)}$ ( $n > k$) → $\mathbb EG^{-1}=\frac1{n-1}$, $\mathbb EG^{-2}=\frac1{(n-1)(n-2)}$.
- 따라서 ( $n > 2$)

$$\mathbb E_\alpha\hat\alpha=\frac n{n-1}\alpha,\qquad\mathrm{Var}_ \alpha(\hat\alpha)=n^2\alpha^2\Big\lbrace\frac1{(n-1)(n-2)}-\frac1{(n-1)^2}\Big\rbrace=\frac{n^2\alpha^2}{(n-1)^2(n-2)} .$$

- 불편 $\frac{n-1}n\hat\alpha$의 분산은 $\frac{\alpha^2}{n-2}$ — 하한 $\frac{\alpha^2}n$보다 크고 비 $\frac{n-2}n\to1$. 점근적으로는 Thm 9.1대로 $\sqrt n(\hat\alpha-\alpha)\xrightarrow{d}N(0,\alpha^2)$.
- (보충) $\sum\log X_i$는 지수족의 완비충분통계량이라 $\frac{n-1}n\hat\alpha$은 UMVUE다 (ch6). Ex 9.6과 같은 교훈: UMVUE여도 하한을 달성하지 못할 수 있다.

### 6.5 정리표

| 모형 | $I_1$ | 하한 ( $n$ 관측) | MLE | 하한 달성? |
| --- | --- | --- | --- | --- |
| 포아송 $\lambda$ | $1/\lambda$ | $\lambda/n$ | $\bar X$ (불편) | ✅ → UMVUE |
| 베르누이 $p$ | $\frac1{p(1-p)}$ | $\frac{p(1-p)}n$ | $\bar X$ | ✅ |
| 베르누이 $p(1-p)$ | 〃 | $\frac{(1-2p)^2p(1-p)}n$ | $\bar X(1-\bar X)$ (편향) | ❌ (비 → 1, $p\ne\frac12$) |
| 정규 $\mu$, $\sigma^2$ | $\mathrm{diag}(\frac1{\sigma^2},\frac1{2\sigma^4})$ | $\frac{\sigma^2}n$, $\frac{2\sigma^4}n$ | $\bar X$, $\frac{n-1}nS^2$ | $\mu$ ✅, $S^2$ ❌ |
| Beta $(\alpha,1)$ | $1/\alpha^2$ | $\alpha^2/n$ | $-n/\sum\log X_i$ (편향) | ❌ (비 → 1) |
| 지수 율 $\theta$ | $1/\theta^2$ | $\theta^2/n$ | $1/\bar X$ (편향) | 1차 근사에서만 |
| 균등 $(0,\theta)$ | 정칙 아님 | 적용 불가 | $X_{(n)}$ | 속도 $n$ |

**큰 그림 (9강의 결론)** 정칙 모형에서 MLE는 점수의 CLT 덕분에 점근정규이고, 그 점근분산 $I_1^{-1}$은 Cramér–Rao 하한의 $n$배다 → **점근적으로 하한을 달성** (점근 효율). 유한 표본에서는 편향이 있거나 하한을 넘을 수 있고, 지지집합이 모수에 의존하는 모형에서는 둘 다 깨진다. (효율·ARE·한 단계 추정량은 8강 내용 → [ch5.3–5.5 §8](ch5.3-5.5%20최대가능도추정.md#8-점근효율과-한-단계-추정량))

---

## 변형 문제

난이도: `●` 예제의 분포만 바꿈 · `●●` 도구 두세 개 조합 · `●●●` 장을 넘나듦. V1–V3은 24-2 숙제 1 (#8–#10)의 설정을 빌렸다 (같은 과목의 이전 학기 숙제). 힌트부터 열고, 써 본 뒤 풀이를 연다.

### V1 Beta 1 베타의 적률추정량과 MLE

`●` $X_i\overset{\text{iid}}{\sim}\mathrm{Beta}(1,\beta)$, $f(x)=\beta(1-x)^{\beta-1}$ ( $0 < x < 1$). (a) 적률추정량과 그 극한분포 (b) MLE와 그 극한분포 (c) ARE(적률 : MLE).

<details><summary>힌트</summary>

$\mathbb EX=\frac1{1+\beta}$, $\mathrm{Var}X=\frac\beta{(1+\beta)^2(2+\beta)}$, 델타 방법. MLE는 Ex 9.7과 같은 구조 ( $1-X\sim\mathrm{Beta}(\beta,1){}$).

</details>
<details><summary>풀이</summary>

(a) $\bar X=\frac1{1+\tilde\beta}$ → $\tilde\beta=\frac1{\bar X}-1$. $g(m)=\frac1m-1$, $g'(m)=-\frac1{m^2}$, $m=\frac1{1+\beta}$에서 $g'=-(1+\beta)^2$. 델타 방법: $\sqrt n(\tilde\beta-\beta)\xrightarrow{d}N\big(0,(1+\beta)^4\frac\beta{(1+\beta)^2(2+\beta)}\big)=N\big(0,\frac{\beta(1+\beta)^2}{2+\beta}\big)$.

(b) $\ell_n=n\log\beta+(\beta-1)\sum\log(1-X_i)$ → $\hat\beta=-\frac n{\sum\log(1-X_i)}$ ( $\ell_n''=-n/\beta^2 < 0$). $I_1=\frac1{\beta^2}$ → $\sqrt n(\hat\beta-\beta)\xrightarrow{d}N(0,\beta^2)$.

(c) $\mathrm{ARE}=\frac{\beta^2}{\beta(1+\beta)^2/(2+\beta)}=\frac{\beta(\beta+2)}{(\beta+1)^2}=1-\frac1{(\beta+1)^2} < 1$. MLE가 항상 낫고, $\beta$가 크면 차이가 줄어든다.

</details>

### V2 0인지 아닌지만 본 포아송

`●●` $X_i\overset{\text{iid}}{\sim}\mathrm{Poisson}(\lambda)$인데 $Y_i=I(X_i > 0)$만 관측한다. (a) $\lambda$의 MLE와 존재 조건 (b) 극한분포 (c) $X_i$를 다 볼 때와의 효율 비교.

<details><summary>힌트</summary>

$Y_i\sim\mathrm{Bernoulli}(p)$, $p=1-e^{-\lambda}$. $p$의 MLE → 불변성 → 델타 방법.

</details>
<details><summary>풀이</summary>

(a) $\hat p=\bar Y$, 불변성으로 $\hat\lambda=-\log(1-\bar Y)$. $\bar Y=1$이면 가능도가 $\lambda\to\infty$로 증가만 해서 유한 MLE가 없다 (확률 $p^n\to0$).

(b) $\sqrt n(\bar Y-p)\xrightarrow{d}N(0,p(1-p))$, $g(p)=-\log(1-p)$, $g'=\frac1{1-p}$ → 점근분산 $\frac{p(1-p)}{(1-p)^2}=\frac p{1-p}=e^\lambda-1$. 검산: $Y$의 $\lambda$에 대한 정보 $I_Y=\frac{(dp/d\lambda)^2}{p(1-p)}=\frac{e^{-2\lambda}}{(1-e^{-\lambda})e^{-\lambda}}=\frac1{e^\lambda-1}$ ✓.

(c) 완전자료 MLE $\bar X$의 점근분산은 $\lambda$. 효율 $\frac\lambda{e^\lambda-1} < 1$ ( $e^\lambda-1 > \lambda$). $\lambda$가 크면 거의 모두 $Y=1$이라 정보가 급격히 준다.

</details>

### V3 위치가 있는 지수분포

`●●` $f(x;\alpha,\beta)=\frac1\beta e^{-(x-\alpha)/\beta}$ ( $x\ge\alpha$). (a) MLE (b) 각 MLE의 극한분포. 어느 모수가 정칙인가?

<details><summary>힌트</summary>

$\alpha$는 지지집합을 정한다 → 미분하지 말고 우도 모양 ( $X_{(1)}$). $X_{(1)}-\alpha$의 정확한 분포를 구하고, $\hat\beta$는 Slutsky로.

</details>
<details><summary>풀이</summary>

(a) $L=\beta^{-n}\exp\lbrace-\sum(x_i-\alpha)/\beta\rbrace I(\alpha\le x_{(1)})$은 $\alpha\le x_{(1)}$에서 $\alpha$에 대해 증가 → $\hat\alpha=X_{(1)}$. 대입하면 $-n\log\beta-\sum(x_i-x_{(1)})/\beta$ → $\hat\beta=\bar X-X_{(1)}$.

(b) $\Pr(X_{(1)}-\alpha > t)=e^{-nt/\beta}$ → $n(\hat\alpha-\alpha)\sim\mathrm{Exp}(\text{평균 }\beta)$ **정확히** (속도 $n$, 정규 아님 — Ex 9.3과 같은 현상). $\sqrt n(\hat\beta-\beta)=\sqrt n(\bar X-\alpha-\beta)-\frac1{\sqrt n}n(X_{(1)}-\alpha)$. 첫 항 → $N(0,\beta^2)$ (CLT, $\mathrm{Var}X=\beta^2$), 둘째 항 → 0 (확률). Slutsky로 $\sqrt n(\hat\beta-\beta)\xrightarrow{d}N(0,\beta^2)$. **$\beta$는 정칙, $\alpha$는 비정칙.**

</details>

### V4 포아송의 영 확률과 하한

`●●●` $X_i\overset{\text{iid}}{\sim}\mathrm{Poisson}(\lambda)$, $\eta=e^{-\lambda}=\Pr(X=0)$. (a) 불편추정량의 CR 하한 (b) $T=\sum X_i$일 때 $(1-\frac1n)^T$이 불편임을 보이고 분산을 구해 하한과 비교 (c) MLE $e^{-\bar X}$의 편향과 극한분포.

<details><summary>힌트</summary>

(b) 포아송 PGF $\mathbb Es^T=e^{n\lambda(s-1)}$에 $s=1-\frac1n$, 분산은 $s=(1-\frac1n)^2$. $e^x-1 > x$.

</details>
<details><summary>풀이</summary>

(a) $g'(\lambda)=-e^{-\lambda}$, $I_1=1/\lambda$ → 하한 $\frac{\lambda e^{-2\lambda}}n$.

(b) $\mathbb E(1-\frac1n)^T=e^{n\lambda(-1/n)}=e^{-\lambda}$ ✓. $\mathbb E(1-\frac1n)^{2T}=e^{n\lambda\lbrace(1-1/n)^2-1\rbrace}=e^{-2\lambda+\lambda/n}$ → $\mathrm{Var}=e^{-2\lambda}(e^{\lambda/n}-1) > e^{-2\lambda}\frac\lambda n$. 하한을 달성하지 못한다 ( $e^{-\lambda}$는 평균모수 $\lambda$의 1차식이 아니다, [5.4](#54-등호-조건)). 그래도 비는 1로 간다. 이 추정량은 ch6의 UMVUE다 ([ch6 V5](ch6.1-6.3%20최소제곱·베이즈·UMVUE.md#v5-포아송에서-k의-확률)).

(c) $\mathbb Ee^{-\bar X}=\mathbb E(e^{-1/n})^T=e^{n\lambda(e^{-1/n}-1)}\ne e^{-\lambda}$ → 편향. 델타 방법: $\sqrt n(e^{-\bar X}-e^{-\lambda})\xrightarrow{d}N(0,\lambda e^{-2\lambda})$ — 점근분산이 하한의 $n$배와 같다.

</details>

### V5 정규모형의 변동계수

`●●` $X_i\overset{\text{iid}}{\sim}N(\mu,\sigma^2)$, $\mu > 0$. MLE $(\hat\mu,\hat\sigma^2)$의 결합 극한분포를 쓰고, 변동계수 $\gamma=\sigma/\mu$의 MLE의 극한분포를 구하라.

<details><summary>힌트</summary>

Thm 9.1 + Ex 9.6의 $I_1^{-1}$, 다변량 델타 방법 ( $v=\sigma^2$를 변수로).

</details>
<details><summary>풀이</summary>

$\sqrt n\big((\hat\mu,\hat\sigma^2)-(\mu,\sigma^2)\big)\xrightarrow{d}N_2\big(0,\mathrm{diag}(\sigma^2,2\sigma^4)\big)$. $\gamma=\sqrt v/\mu$, $\partial_\mu\gamma=-\frac\sigma{\mu^2}$, $\partial_v\gamma=\frac1{2\sigma\mu}$. 불변성으로 $\hat\gamma=\hat\sigma/\hat\mu$이고

$$\sqrt n(\hat\gamma-\gamma)\xrightarrow{d}N\Big(0,\ \sigma^2\frac{\sigma^2}{\mu^4}+2\sigma^4\frac1{4\sigma^2\mu^2}\Big)=N\Big(0,\ \gamma^4+\frac{\gamma^2}2\Big).$$

</details>

### V6 하한을 달성하면 지수족

`●●●` (a) 1차원 모수에서 $\theta$의 불편추정량 $U_n$이 모든 $\theta$에서 CR 하한을 달성하면 $f^{(n)}_ \theta$가 지수족 꼴임을 보여라. (b) 평균 $\theta$인 지수분포에서 $\bar X$가 하한을 달성함을 확인하고 $S_n=nI_1(\theta)(\bar X-\theta)$를 검산하라.

<details><summary>힌트</summary>

[5.4](#54-등호-조건): Cauchy–Schwarz 등호 ⟺ 선형관계. 그 식을 $\theta$로 적분.

</details>
<details><summary>풀이</summary>

(a) 등호이면 $U_n-\theta=k(\theta)S_n$ (a.s.)이고 양변과 $S_n$의 공분산을 비교하면 $1=k(\theta)nI_1(\theta)$. 따라서 $\partial_\theta\log f^{(n)}_ \theta=nI_1(\theta)(U_n-\theta)$. $\theta$로 적분: $\log f^{(n)}_ \theta(x)=A(\theta)U_n(x)-B(\theta)+h(x)$, $A'=nI_1$, $B'=\theta nI_1$. 지수족이고 $U_n$이 충분통계량이다.

(b) $\ell_1=-\log\theta-x/\theta$, $s=-\frac1\theta+\frac x{\theta^2}$, $-\ell_1''=-\frac1{\theta^2}+\frac{2x}{\theta^3}$ → $I_1=\frac1{\theta^2}$, 하한 $\frac{\theta^2}n=\mathrm{Var}(\bar X)$ ✓. $S_n=\sum(\frac{X_i}{\theta^2}-\frac1\theta)=\frac n{\theta^2}(\bar X-\theta)=nI_1(\bar X-\theta)$ ✓. (율모수 $1/\theta$로 쓰면 $1/\bar X$는 편향이라 달성하지 못한다 — 같은 모형이라도 **어느 모수의 1차식인가**가 중요하다)

</details>

### V7 균등분포에서 CR 하한이 깨진다

`●` $X_i\overset{\text{iid}}{\sim}\mathrm{Uniform}(0,\theta)$. (a) $\mathbb Es$와 형식적 정보 $\mathbb Es^2$를 구하라. (b) $\frac{n+1}nX_{(n)}$의 분산을 구해 형식적 "하한"과 비교하고, 무엇이 잘못되었는지 설명하라.

<details><summary>힌트</summary>

$X_{(n)}/\theta\sim\mathrm{Beta}(n,1)$. 미분·적분 교환에서 경계항.

</details>
<details><summary>풀이</summary>

(a) $s=-\frac1\theta$ (상수) → $\mathbb Es=-\frac1\theta\ne0$, $\mathbb Es^2=\frac1{\theta^2}$ → "하한" $\frac{\theta^2}n$.

(b) $\mathbb EX_{(n)}=\frac n{n+1}\theta$, $\mathbb EX_{(n)}^2=\frac n{n+2}\theta^2$ → $\mathrm{Var}X_{(n)}=\frac{n\theta^2}{(n+1)^2(n+2)}$, $\mathrm{Var}\big(\frac{n+1}nX_{(n)}\big)=\frac{\theta^2}{n(n+2)} < \frac{\theta^2}n$ ( $n\ge1$). 지지집합 $(0,\theta)$가 $\theta$에 의존해서 $\frac d{d\theta}\int_0^\theta=\int_0^\theta\frac{\partial}{\partial\theta}+(\text{경계항})$이 되고, ②·③의 교환이 성립하지 않는다.

</details>

---

## 셀프 테스트

### 백지 증명 체크리스트

- [ ] **Thm 9.1 ( $d=1$):** ① 확률 → 1 사건에서 $S_n(\hat\theta)=0$ ② 평균값 정리 (9.2) ③ 분모: LLN + 지배조건 ④ 분자: CLT ⑤ Slutsky. 각 줄에 쓰인 정칙조건 번호까지.
- [ ] 다변량 판: 성분별 중간점 $\tilde\theta^{(j)}$, $I^{-1}II^{-1}$.
- [ ] **Bartlett 항등식** (a)(b)(c): $\int f=1$ 한 번·두 번 미분, $\nabla f=fs$.
- [ ] **Thm 9.2:** 지배조건 → $\nabla\mathbb EU=\mathbb E[US_n]$, $\mathbb ES_n=0$ → $c=\mathrm{Cov}$ → Cauchy–Schwarz → $a=V^{-1}c$ → $c=0$ 경우.
- [ ] 벡터 통계량 판 ( $b^\top U_n$), 함수형 모수, 다모수 성분 하한 $[I_1^{-1}]_ {kk}/n$.
- [ ] (보충) 등호 조건 → 지수족.
- [ ] 균등분포 반례: $(1-\frac t{n\theta})^n\to e^{-t/\theta}$, $\sqrt n$ 스케일은 0으로.
- [ ] 정보 계산 다섯 개 (포아송, 베르누이, 지수 율, 정규 2모수, Beta $(\alpha,1){}$)를 2분 안에.

### 짧은 문답

<details><summary>Q1. 표본 전체의 정보가 nI₁인 이유는?</summary>

$S_n$이 독립인 점수 $n$개의 합이고 각 점수의 분산이 $I_1$이기 때문이다 (Bartlett (c)).

</details>
<details><summary>Q2. CR 하한을 달성하는 불편추정량이 있으면 UMVUE인가?</summary>

그렇다. 모든 불편추정량의 분산이 하한 이상이고 그 추정량의 분산이 하한과 같으니 모든 θ에서 최소다. 역은 거짓: 정규의 $S^2$, Beta의 $\frac{n-1}n\hat\alpha$은 UMVUE지만 하한보다 분산이 크다.

</details>
<details><summary>Q3. Ex 9.5에서 MLE는 편향인데 왜 "효율적"이라고 하나?</summary>

편향이 $O(1/n)$이라 표준오차 $O(1/\sqrt n)$보다 작고, 분산의 첫 항이 정확히 하한이라 점근적으로 하한을 달성하기 때문이다 ( $p\ne\frac12$).

</details>
<details><summary>Q4. 균등분포 MLE의 수렴이 정규보다 "빠르다"는 말의 뜻은?</summary>

오차가 $1/\sqrt n$이 아니라 $1/n$ 크기다: $n(\theta-\hat\theta)$가 비퇴화 극한 (지수분포)을 가진다. 그래서 $\sqrt n$으로 곱하면 0으로 간다.

</details>
<details><summary>Q5. 9강 CR 증명에서 Cauchy–Schwarz를 쓰기 전에 꼭 확인해야 하는 사실 두 개는?</summary>

$\mathbb ES_n=0$ (그래야 곱의 기댓값이 공분산·분산이 된다)과 $V=nI_1$의 양정치성 (그래야 $V^{-1}$이 있고 $c^\top V^{-1}c > 0$으로 나눌 수 있다).

</details>
<details><summary>Q6. 정규모형에서 μ를 모를 때 σ²의 하한이 2σ⁴/n인 이유를 정보행렬로 말하면?</summary>

하한은 $[I_1^{-1}]_ {22}/n$인데 $I_1$이 대각이라 $[I_1^{-1}]_ {22}=1/[I_1]_ {22}=2\sigma^4$이다. 비대각 성분이 있으면 부수모수를 모르는 대가로 하한이 커진다.

</details>
<details><summary>Q7. Ex 9.7에서 Y = −α log X가 Exp(1)인 것을 한 줄로?</summary>

$\Pr(Y > y)=\Pr(X\lt e^{-y/\alpha})=F_X(e^{-y/\alpha})=(e^{-y/\alpha})^\alpha=e^{-y}$.

</details>

---

## 강의노트 확인

| 위치 | 강의노트 | 노트에 반영한 것 |
| --- | --- | --- |
| p4 | "특히 (R1)과 (R4)는 … Bartlett 항등식을 정당화한다" | 9강 번호로는 지지집합 고정 **(R2)**와 적분 아래 미분 **(R5)**. 8강 번호 ((R1) 지지집합, (R4) 미분)를 옮기다 생긴 것으로 보인다 ✏️ |
| p1, p4 | $I(\theta)$와 $I_1(\theta)$를 섞어 씀 | 같은 것 (한 관측치 정보) |
| p2 | 분모 수렴을 "대수의 법칙과 $\tilde\theta_n\to\theta_0$에서 따른다"로 한 줄 | 3차 도함수 지배조건으로 나머지를 누르는 단계를 보충 (2.2 ③) |
| p2 | 다변량은 "같은 전개를 벡터와 행렬로" | 벡터 평균값 정리는 한 점으로 안 된다 → 행마다 다른 중간점 (2.3) |
| p4 (9.4) | 스칼라 부등식에 $\succeq$ | 스칼라에서는 $\ge$와 같음 |
| p6 | 벡터 판 $\nabla g(nI_1)^{-1}\nabla g^\top$ vs 스칼라 판 $\nabla g^\top(nI_1)^{-1}\nabla g$ | 기울기의 모양 ( $d\times1$ vs $m\times d$)이 달라서 전치 위치가 다름 (5.3) |
| p6 Ex 9.4 | "따라서 UMVUE이기도 하다" | 하한 달성 ⇒ UMVUE 논증 한 줄 보충 (6.1) |
| p7 Ex 9.5 | MLE 분산 식 | 기호 계산으로 확인 ✓ |
| p7 Ex 9.6 | "두 번째 하한보다 크다" | 2모수에서 하한은 $[I_1^{-1}]_ {22}/n$ (대각이라 $1/[I_1]_ {22}$와 같음) 보충 |
| p7 Ex 9.7 | 평균·분산 식 | 역감마 적률로 유도, 시뮬레이션으로 확인 ✓ |
