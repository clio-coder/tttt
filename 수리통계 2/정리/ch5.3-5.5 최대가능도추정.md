# ch5.3–5.5 최대가능도추정 (MLE)

> **원본:** `수업자료/lecture_ch5.3_MLE.pdf` (6강, 5쪽) · `수업자료/lecture_ch5.4_MLE Algorithm.pdf` (7강, 8쪽) · `수업자료/lecture_ch5.5_MLE Asymptotics.pdf` (8강, 11쪽) — 세 강의를 한 노트로 합쳤다. 출처 태그는 `[6강 p2]`, `[7강 p4]`, `[8강 p7]`.
> **수업:** 9/22 최대가능도추정법 · 9/29 MLE 알고리즘 (녹화) · 10/6 MLE의 점근이론 → **중간고사 1 (10/12) 범위의 마지막 부분**
> **관련 숙제:** hw2 전체 (#1 support 모수, #2 이산 모수, #3 경쟁위험, #4 Satterthwaite (적률법), #5 EM) — 이 노트 [hw2 풀이](#hw2-풀이)에 모았다. #1(a)는 ch4, #1(c)·#4는 ch5.2 적률법 내용이지만 ch5.1–5.2 노트가 아직 없어 여기 둔다.
> **범례:** 📝 hw에 나옴 · 🧩 강의노트에 증명 없음 → 보충 증명 · 📐 강의노트에 증명 있음 · 🔢 계산 · 💡 개념 · (보충) 강의노트에 없는 내용 · `[T…]` = [도구상자](00%20문제풀이%20도구상자.md) 카드
> **기호:** $\ell(\theta)=\log L(\theta)$, $U(\theta)=\nabla\ell$ (score), $H(\theta)=\nabla^2\ell$, 한 관측치 점수 $s(\theta;X)$, 표본 점수 $S_n(\theta)=\sum s(\theta;X_i)$, 한 관측치 Fisher 정보 $I_1(\theta)$ (8강은 $I(\theta)$로도 씀). 지수족은 7강 꼴 $f_\alpha(x)=\exp\lbrace\alpha^\top t(x)-\psi(\alpha)\rbrace h(x)$ (우리 ch2와 같다).
> **더 연습하려면:** 수업 외 자료인 [튜터링1](튜터링/튜터링1%20적률추정량과%20최대가능도추정량.md) (MLE 계산 13문제), [튜터링2](튜터링/튜터링2%20최대가능도추정량의%20점근성질.md) (정보량·극한분포 8문제).

---

## 0. 한눈에

### 0.1 범위

| 강의 | 쪽 | 절 | 내용 | 증명 | hw2 |
| --- | --- | --- | --- | --- | --- |
| 6강 | p1–3 | 6.1 | MLE의 정의, 가능도방정식과 경계, 베르누이, **존재·유일성 (Thm 6.1)** | 📐 | #1, #2, #3 |
| 6강 | p3–5 | 6.2 | 프로파일 가능도, 불변성, 로짓, 지수분포 평균 (재매개화로 오목하게) | 📐 | #3 |
| 7강 | p1–3 | 7.1 | **지수족의 MLE (Thm 7.1)**, 기하분포, 이중지수 → 중앙값 | 📐 | – |
| 7강 | p3–5 | 7.2–7.5 | Newton–Raphson (로지스틱 = IRLS), gradient descent, coordinate descent | – | – |
| 7강 | p5–8 | 7.6 | **EM 알고리즘**, 단조성 (Thm 7.2), ABO 혈액형, 알고리즘 비교 | 🧩 | #5 |
| 8강 | p1–4 | 8.1–8.2 | KL과 경험위험, **일치성 (Thm 8.1)**, Neyman–Scott 반례, 모형 오설정 | 📐 | – |
| 8강 | p4–6 | 8.3–8.7 | 점수·Fisher 정보, **점근정규성 (Thm 8.2)**, 베르누이·지수 예, 균등분포 반례 | 📐 | – |
| 8강 | p6–8 | 9 | 정칙조건 (R0)–(R4), Bartlett 항등식, **Cramér–Rao (Thm 9.1)**, 예 | 📐 | – |
| 8강 | p8–11 | 10–13 | 점근효율, Hodges, 위치모형 ARE 표, 로지스틱, 한 단계 추정량 | – | – |

### 0.2 시험 포인트 (출제 가능성 순)

근거: hw2 (시험 직전 숙제, 마감 10/12 = 시험 당일) > 강의노트 증명 > 예제.

**★★★ 매우 유력**

1. 📝 **MLE 계산 세 갈래** — ① 미분 가능·오목 → 가능도방정식 ② support가 모수에 의존 → 우도 모양 (hw2 #1: $X_{(1)}$) ③ 이산 모수공간 → 직접 비교 (hw2 #2). 그리고 **경계·존재 여부** 확인 (hw2 #3: 한쪽 원인이 0번이면 유한 MLE 없음). → [1](#1-mle의-정의와-존재유일성), [hw2](#hw2-풀이)
2. 📐 **존재·유일성 (Thm 6.1) + 재매개화로 오목하게** — "경계에서 $-\infty$ → 존재, 헤시안 음정치 → 유일". 지수분포 평균은 $\theta$로 오목이 아니라 $\lambda=1/\theta$로 바꾼다. → [1.3](#13-존재와-유일성), [2.3](#23-재매개화로-오목하게-만들기)
3. 📐 **점근정규성 증명 (Thm 8.2)** — 점수 Taylor 전개 → 분모 LLN → 분자 CLT → Slutsky. 그리고 **정보량 계산 → 극한분포 쓰기**. → [6](#6-점근정규성)
4. 📝🧩 **EM 알고리즘** — E-step (잠재변수의 조건부 기댓값), M-step (완전자료 MLE에 대입), 단조성. hw2 #5가 통째로 이것. → [4.4](#44-em-알고리즘), [hw2 #5](#hw2-5-결측값이-있는-포아송과-em)
5. 📐 **Cramér–Rao 부등식과 증명**, 하한이 달성되는 예 (포아송·베르누이)와 안 되는 예 ($S^2$). → [7](#7-정칙조건과-cramérrao-하한)

**★★ 유력**

6. 📐 지수족의 MLE (Thm 7.1): $\nabla\psi(\hat\alpha)=\bar t$, 유일성 (헤시안 = $-n\mathop{\mathrm{Var}}\nolimits(t){}$). → [3](#3-지수족의-mle)
7. 📐 일치성 (Thm 8.1) 증명 — 강한 식별성 + 균등 대수의 법칙. Neyman–Scott 반례 ($\sigma^2/2$). → [5](#5-mle의-일치성)
8. 📐 프로파일 가능도와 불변성 (일대일이 아닐 때도). → [2](#2-프로파일-가능도와-불변성)
9. 🔢 균등분포 반례: $n(\hat\theta-\theta)$의 극한이 지수분포 → 정칙조건이 깨지면 정규 극한 없음. → [6.3](#63-예와-반례)
10. 💡 Newton–Raphson 반복식, 로지스틱 회귀의 $U$, $H$ (IRLS). → [4.1](#41-newtonraphson)

**★ 가능성 있음**

11. 💡 gradient·coordinate descent의 성질, 알고리즘 비교표, 실전 점검 목록
12. 🔢 위치모형 ARE 표 (정규·로지스틱·이중지수), 한 단계 추정량
13. 💡 모형 오설정 → KL 투영 $\theta^\ast$, Hodges 추정량

### 0.3 이 장의 도구

| 도구 | 쓰는 곳 |
| --- | --- |
| [T25] (신규) **존재·유일성 틀:** 경계에서 $\ell\to-\infty$ + 헤시안 음정치 | Thm 6.1, 지수분포, 로지스틱 |
| [T26] (신규) **재매개화로 오목하게** + 불변성으로 되돌리기 | 지수분포 평균, hw2 #3 |
| [T22] 분해정리 + 지시함수, support 모수는 미분하지 않기 | hw2 #1 |
| [T27] (신규) **점수의 Taylor 전개 → LLN + CLT + Slutsky** | Thm 8.2, 한 단계 추정량 |
| [T18] CMT·Slutsky, 델타 방법 | 점근분포, 효율 |
| [T28] (신규) **EM: 완전자료 MLE에 조건부 기댓값 대입** | hw2 #5, ABO |
| [T13] 지수족: $\nabla\psi=\mathbb Et$, $\nabla^2\psi=\mathop{\mathrm{Var}}\nolimits(t)$ | Thm 7.1 |

---

## 1. MLE의 정의와 존재·유일성

### 1.1 정의와 주의점

💡 [6강 p1]

- **MLE** $\hat\theta$ = 가능도 $L(\theta)=\prod_{i=1}^nf(X_i\mid\theta)$를 최대화하는 값. 계산은 로그가능도 $\ell(\theta)=\log L(\theta)$로.
- 적률추정량은 모수공간 밖의 값을 낼 수 있지만 MLE는 **모수공간 안에서** 최대화한다. 단 모수공간이 열린 집합이고 최대가 경계에서만 달성되면 MLE가 **존재하지 않을 수 있다**.
- 직관: 관측된 자료에 가장 큰 가능도를 주는 모수값.
- **어려움 세 가지:** 계산 (전역 최댓값이 있는가, 찾은 값이 전역인가), 수치적 안정성 (Gaussian mixture, $\text{Uniform}(0,\theta)$처럼 매끄럽지 않은 경우), 강건성 (표본이 조금 바뀌면 크게 달라지는가).
- **가능도방정식** $\partial\ell/\partial\theta_j=0$ ($j=1,\dots,k$)은 **내부점만** 다룬다 → 경계에서 최대가 될 가능성은 따로 확인한다.

### 1.2 베르누이와 경계

🔢 [6강 p2 Example 6.1]

$X_i\overset{\text{iid}}{\sim}\text{Bernoulli}(p)$, $p\in[0,1]$, $S=\sum X_i$. $\ell(p)=S\log p+(n-S)\log(1-p)$, $\ell'(p)=\frac Sp-\frac{n-S}{1-p}$.
- $0 < S < n$: $\hat p=S/n=\bar X$.
- $S=0$: $\ell=n\log(1-p)$ 감소 → $\hat p=0$. $S=n$: $\ell=n\log p$ 증가 → $\hat p=1$. 경계에서 최대이고 여전히 $\hat p=S/n$.
- **Remark:** $p\in(0,1)$ (열린 공간)이면 $S=0$ 또는 $n$일 때 MLE가 **존재하지 않는다**. 로짓 $\psi=\log\frac p{1-p}$로 보면 $\hat\psi=\pm\infty$ (Example 6.3).

### 1.3 존재와 유일성

📐 [6강 p2–3 Thm 6.1]

**가정 (존재)** $\Theta\subset\mathbb R^d$는 공집합이 아닌 열린 집합, $\ell$은 연속, $\Theta$ 안의 모든 콤팩트 집합을 벗어나는 수열 $(\theta_m)$에 대해 $\ell(\theta_m)\to-\infty$.
**가정 (유일성)** 추가로 $\Theta$ 볼록, $\ell\in C^2$, 모든 $\theta$에서 $\nabla^2\ell(\theta)$ 음정치.
**결론** MLE가 존재하고, 유일성 가정 아래에서는 유일하며 $\nabla\ell(\theta)=0$의 유일한 해다.

**핵심 한 수** "멀리 가면 $-\infty$" → 콤팩트 집합 위의 최대 문제로 → 최대값 정리. 유일성은 최대점 중심 Taylor.

**증명**
1. **콤팩트로 가두기:** $\theta_0\in\Theta$를 고정. $\theta\notin K$이면 $\ell(\theta) < \ell(\theta_0)$인 콤팩트 $K\ni\theta_0$가 있다. (없다면 콤팩트 집합을 키워 가며 그 밖에서 $\ell\ge\ell(\theta_0)$인 점들을 고를 수 있고, 그 수열은 모든 콤팩트를 벗어나는데 $\ell\not\to-\infty$ — 가정에 모순)
2. **최대값 정리:** 그러면 $\sup_\Theta\ell=\max_K\ell$이고, $\ell$이 $K$에서 연속이라 어떤 $\hat\theta\in K$에서 최댓값이 달성된다 → 존재.
3. **유일성:** $\Theta$가 열려 있으니 $\hat\theta$는 내부점, $\nabla\ell(\hat\theta)=0$. 임의의 $\theta$에 대해 선분 $[\hat\theta,\theta]\subset\Theta$ (볼록)이고, 그 위의 $\theta^\ast$로 Taylor:

$$\ell(\theta)=\ell(\hat\theta)+\tfrac12(\theta-\hat\theta)^\top\nabla^2\ell(\theta^\ast)(\theta-\hat\theta)\le\ell(\hat\theta),$$

   등호는 $\theta=\hat\theta$일 때뿐 (∵ 음정치). ∎

**답안 틀 (1차원):** "① $\ell''(\theta) < 0$ ∀θ ② 모수공간의 양 끝에서 $\ell\to-\infty$ (또는 $\ell'$의 부호가 $+$에서 $-$로) ③ 따라서 $\ell'=0$의 유일한 근이 MLE." (튜터링1 Thm 4와 같은 내용)

**셀프 체크**
<details><summary>Q1. 1단계에서 왜 콤팩트 집합 K가 존재하는가?</summary>

없다고 하면 커지는 콤팩트 집합 $K_m$ 밖에서 $\ell(\theta_m)\ge\ell(\theta_0)$인 $\theta_m$을 고를 수 있다. 이 수열은 모든 콤팩트를 벗어나므로 가정에 의해 $\ell(\theta_m)\to-\infty$여야 하는데 모순.

</details>
<details><summary>Q2. 유일성에 볼록성이 필요한 이유는?</summary>

Taylor 전개를 쓸 선분 $[\hat\theta,\theta]$가 모수공간 안에 있어야 한다.

</details>

---

## 2. 프로파일 가능도와 불변성

### 2.1 프로파일 가능도

📐 [6강 p3–4]

$\theta=(\eta,\xi)$에서 관심 모수 $\eta$의 **프로파일 가능도** $L_p(\eta)=\sup_{\xi:(\eta,\xi)\in\Theta}L(\eta,\xi)$.

**계산 순서:** ① $\eta$ 고정 ② $\hat\xi(\eta)=\arg\max_\xi L(\eta,\xi)$ ③ $L_p(\eta)=L(\eta,\hat\xi(\eta))$ ④ $L_p$를 $\eta$에 대해 최대화.

**Example 6.2 (정규분포의 평균)** $\ell(\mu,\sigma^2)=-\frac n2\log\sigma^2-\frac1{2\sigma^2}\sum(X_i-\mu)^2+C$. $\mu$ 고정 시 $\hat\sigma^2(\mu)=\frac1n\sum(X_i-\mu)^2$ → $\ell_p(\mu)=-\frac n2\log\big\lbrace\frac1n\sum(X_i-\mu)^2\big\rbrace+C$ → 제곱합 최소화 → $\hat\mu=\bar X$.

**프로파일 MLE = 전체 MLE의 성분.** 전체 MLE $(\hat\eta,\hat\xi)$에 대해 모든 $\eta$에서

$$L_p(\eta)=\sup_\xi L(\eta,\xi)\le L(\hat\eta,\hat\xi)=\sup_\xi L(\hat\eta,\xi)=L_p(\hat\eta).$$

(부등호: 전체 MLE의 정의. 가운데 등호: $\hat\xi$는 $\hat\eta$를 고정했을 때도 최대점.) ∎

### 2.2 불변성

📐 [6강 p4]

**주장** $\eta=g(\theta)$이면 $\hat\eta=g(\hat\theta)$.
- **$g$가 일대일:** $L^\ast(\eta)=L(g^{-1}(\eta))$이면 $L^\ast(g(\hat\theta))=L(\hat\theta)\ge L(\theta)=L^\ast(g(\theta))$ → $g(\hat\theta)$가 $L^\ast$의 최대점.
- **일대일이 아니어도 성립:** $L^\ast(\eta)=\sup_{\theta:g(\theta)=\eta}L(\theta)$ (유도 가능도)로 정의하면 이것은 프로파일 가능도이고, 2.1에 의해 최대점이 $g(\hat\theta)$. → 프로파일 가능도는 "일대일이 아닌 변환에도 불변성을 적용하는 방법".

**Example 6.3 (로짓)** $0 < S < n$이면 $\hat\psi=\log\frac{\hat p}{1-\hat p}$. 성공만·실패만 관측되면 $\mathbb R$ 안에 유한 MLE가 없다 (확장 공간에서 $\pm\infty$).

### 2.3 재매개화로 오목하게 만들기

📐 [6강 p5 Example 6.4] · [T26]

$X_i\overset{\text{iid}}{\sim}\text{Exp}(\theta)$ (평균 $\theta$). $\ell(\theta)=-n\log\theta-\frac{n\bar x}\theta$, $\ell''(\theta)=\frac n{\theta^2}-\frac{2n\bar x}{\theta^3}$ — **부호가 $\theta$에 따라 바뀌어** Thm 6.1을 바로 쓸 수 없다.

$\lambda=1/\theta$로 바꾸면 $\ell(\lambda)=n\log\lambda-n\lambda\bar x$:
- $\ell''(\lambda)=-n/\lambda^2 < 0$ (순오목),
- $\lim_{\lambda\to0^+}\ell'(\lambda)=+\infty$, $\lim_{\lambda\to\infty}\ell'(\lambda)=-n\bar x < 0$ → $\ell'=0$의 근 $\hat\lambda=1/\bar X$가 내부에 유일하게 존재 → 유일한 전역 최대.
- 불변성: $\hat\theta=1/\hat\lambda=\bar X$.

**교훈:** 모수를 적절히 바꾸면 로그가능도가 순오목이 되어 존재·유일성이 쉬워진다. 지수족은 **자연모수**로 쓰면 항상 오목하다 (→ 3).

---

## 3. 지수족의 MLE

### 3.1 Thm 7.1

📐 [7강 p1–2 Thm 7.1]

**가정** 공통 지지집합 $\mathcal X_0=\lbrace x:f_\alpha(x) > 0\rbrace$ ($\alpha$와 무관)을 갖는 $p$차원 정규 지수족 $f_\alpha(x)=\exp\lbrace\alpha^\top t(x)-\psi(\alpha)\rbrace h(x)$, 자연모수공간 $A$는 공집합이 아닌 **열린 볼록집합**, 모든 $\alpha\in A$에서 $\mathop{\mathrm{Var}}\nolimits_ \alpha\lbrace t(X)\rbrace$ 양정치. $\bar t=\frac1n\sum t(x_i)$.
**결론** 가능도방정식 $\nabla\psi(\alpha)=\bar t$의 해 $\hat\alpha\in A$가 존재하면 그것이 **유일한** MLE.

**증명**
1. **CGF:** $\alpha+s\in A$이면 $K_\alpha(s)=\log\mathbb E_\alpha e^{s^\top t(X)}=\psi(\alpha+s)-\psi(\alpha)$. $A$가 열려 있어 $s=0$ 근방에서 미분 → $\nabla\psi(\alpha)=\mathbb E_\alpha t(X)$, $\nabla^2\psi(\alpha)=\mathop{\mathrm{Var}}\nolimits_ \alpha t(X)$ (양정치).
2. $\ell(\alpha)=n\lbrace\alpha^\top\bar t-\psi(\alpha)\rbrace+C(x)$ → $\nabla\ell=n(\bar t-\nabla\psi(\alpha))$, $\nabla^2\ell=-n\nabla^2\psi(\alpha)$ 음정치.
3. $\ell$이 $A$ 위에서 순오목이므로 가능도방정식의 해는 유일한 전역 최대점. ∎

- ⚠ **해가 존재할 때**의 결론이다. $\bar t$가 평균모수공간의 경계에 놓이면 유한 MLE가 없다 (베르누이에서 $S=0$).
- **원래 모수로:** $\alpha=g(\theta)$ 일대일이면 $\hat\theta=g^{-1}(\hat\alpha)$ (불변성). 지수분포 평균은 $\alpha=-1/\theta$.
- 우리 [ch2 5.3](ch2%20모수모형과%20지수족.md#53-보충-theorem-21에서-나오는-것들)의 "MLE = 적률 맞추기"가 이 정리다.

### 3.2 예

🔢 [7강 p2–3]

**Example 7.1 (기하분포)** $f(x;p)=(1-p)^{x-1}p$, $x=1,2,\dots$ $\alpha=\log(1-p)$로 두면 $f_\alpha(x)=\exp\lbrace\alpha x+\log(e^{-\alpha}-1)\rbrace$ ($\alpha < 0$), $\psi(\alpha)=-\log(e^{-\alpha}-1)$, $\dot\psi(\alpha)=\frac1{1-e^\alpha}=\frac1p$. 가능도방정식 $\frac1p=\bar X$ → $\hat p=1/\bar X$.

**Example 7.2 (이중지수 → 중앙값)** $f(x;\theta)=\frac12e^{-\lvert x-\theta\rvert}$이면 MLE는 $S(\theta)=\sum\lvert X_{(i)}-\theta\rvert$의 최소화. 순서통계량 사이의 구간 $I_{j+1}=[X_{(j)},X_{(j+1)}]$에서 $S$의 기울기는 $j-(n-j)$ (아래 개수 − 위 개수):
- $j < n-j$이면 감소, $j > n-j$이면 증가, $j=n-j$이면 평평.
- $n=2m+1$이면 유일한 MLE $X_{(m+1)}$, $n=2m$이면 $[X_{(m)},X_{(m+1)}]$의 모든 값이 MLE → **표본중앙값은 MLE이지만 짝수 표본에서는 유일하지 않다**. (지수족이 아니고 미분 불가)

---

## 4. 최적화 알고리즘

💡 [7강 p3–8] — MLE가 닫힌 꼴로 없을 때 반복값 $\theta^{(0)},\theta^{(1)},\dots$으로 근사. 양수 제약은 $\sigma=e^\eta$ 같은 재매개화나 제약 최적화로.

### 4.1 Newton–Raphson

[7강 p3–4] · ◆ 반복식은 외울 것

내부의 매끄러운 최대점에서 $U(\hat\theta)=0$, $H(\hat\theta)$ 음정치. 현재값 근처에서 점수를 1차 Taylor 전개: $0=U(\hat\theta)\approx U(\theta^{(t)})+H(\theta^{(t)})(\hat\theta-\theta^{(t)})$ →

$$\theta^{(t+1)}=\theta^{(t)}-H(\theta^{(t)})^{-1}U(\theta^{(t)}).\qquad(7.1)$$

- 종료: $\lVert U(\theta^{(t)})\rVert$ 또는 $\lvert\ell(\theta^{(t+1)})-\ell(\theta^{(t)})\rvert$가 충분히 작을 때.
- 장점: 해 근처에서 **2차 수렴** (매우 빠름). 단점: 초기값이 나쁘면 발산, $H$가 비가역일 수 있음, 우도가 감소하는 방향으로 갈 수도 있음.
- ⚠ 점수방정식을 푸는 것과 최대화는 경계해, 여러 국소 최대, 특이 헤시안 때문에 항상 같지는 않다.

**Example 7.3 (로지스틱 회귀)** $\log\frac{p_i}{1-p_i}=x_i^\top\beta$이면 $U(\beta)=X^\top(y-p)$, $H(\beta)=-X^\top WX$, $W=\mathrm{diag}\lbrace p_i(1-p_i)\rbrace$:

$$\beta^{(t+1)}=\beta^{(t)}+(X^\top WX)^{-1}X^\top(y-p)\quad(\text{IRLS로도 해석}).$$

### 4.2 Gradient descent

[7강 p4–5]

$F(\theta)=-\ell(\theta)$를 최소화: $\theta^{(t+1)}=\theta^{(t)}-\gamma_t\nabla F(\theta^{(t)})=\theta^{(t)}+\gamma_t\nabla\ell(\theta^{(t)})$ (7.2), $\gamma_t$ = 보폭.
- **왜 내려가나:** $F(\theta+\varepsilon d)\approx F(\theta)+\varepsilon\nabla F(\theta)^\top d$. $d=-\nabla F$이면 변화량 $-\varepsilon\lVert\nabla F\rVert^2\le0$ → 보폭이 충분히 작으면 감소.
- 보폭이 너무 작으면 느리고, 너무 크면 증가하거나 진동한다 (그림 1). 헤시안이 필요 없어 고차원·대규모에 단순.

### 4.3 Coordinate descent

[7강 p5]

한 번에 한 좌표만: $\theta_j^{(t+1)}=\arg\min_aF(\theta_1^{(t+1)},\dots,\theta_{j-1}^{(t+1)},a,\theta_{j+1}^{(t)},\dots,\theta_d^{(t)})$. 각 갱신이 $F$를 증가시키지 않으므로 $F(\theta^{(0)})\ge F(\theta^{(1)})\ge\cdots$. 좌표별 최적화가 쉽고 볼록일 때 유용, 좌표 간 상관이 강하면 느리다.

### 4.4 EM 알고리즘

📝🧩 [7강 p5–7] · hw2 #5 · [T28]

관측자료 $Y$, 잠재자료 $Z$. 관측자료 로그가능도 $\ell(\theta;Y)=\log\sum_zf(Y,z\mid\theta)$는 합의 로그라 직접 최대화가 어렵다. 현재값 $\theta^{(t)}$에서

$$Q(\theta\mid\theta^{(t)})=\mathbb E_{Z\mid Y,\theta^{(t)}}\big[\log f(Y,Z\mid\theta)\big].$$

- **E-step:** $f(Z\mid Y;\theta^{(t)})$로 $Q(\theta\mid\theta^{(t)})$를 계산 (완전자료 로그가능도가 $Z$에 대해 1차식이면 = $Z$를 조건부 기댓값으로 바꾸기).
- **M-step:** $\theta^{(t+1)}=\arg\max_\theta Q(\theta\mid\theta^{(t)})$ (= 완전자료 MLE 공식에 대입).

**Theorem 7.2 (단조성)** $\ell(\theta^{(t+1)};Y)\ge\ell(\theta^{(t)};Y)$.

**(보충 증명)** 강의노트에 증명이 없다.
1. $f(Y,Z\mid\theta)=f(Z\mid Y,\theta)f(Y\mid\theta)$이므로 $\ell(\theta;Y)=\log f(Y,Z\mid\theta)-\log f(Z\mid Y,\theta)$. 양변을 $Z\sim f(\cdot\mid Y,\theta^{(t)})$로 기댓값:

$$\ell(\theta;Y)=Q(\theta\mid\theta^{(t)})-V(\theta\mid\theta^{(t)}),\qquad V(\theta\mid\theta^{(t)})=\mathbb E_{Z\mid Y,\theta^{(t)}}\big[\log f(Z\mid Y,\theta)\big].$$

2. $V(\theta\mid\theta^{(t)})\le V(\theta^{(t)}\mid\theta^{(t)})$: 차이 $=-\mathrm{KL}\big(f(\cdot\mid Y,\theta^{(t)})\Vert f(\cdot\mid Y,\theta)\big)\le0$ (Jensen / KL ≥ 0).
3. M-step으로 $Q(\theta^{(t+1)}\mid\theta^{(t)})\ge Q(\theta^{(t)}\mid\theta^{(t)})$. 따라서

$$\ell(\theta^{(t+1)};Y)-\ell(\theta^{(t)};Y)=\big[Q(\theta^{(t+1)})-Q(\theta^{(t)})\big]-\big[V(\theta^{(t+1)})-V(\theta^{(t)})\big]\ge0 .\qquad\blacksquare$$

- 장점: 단조성. 단점: 국소 최대점에 수렴할 수 있고, 최적점 근방에서 Newton보다 느린 경우가 많다.

**Example 7.4 (ABO 혈액형)** 대립유전자 빈도 $p=(p_A,p_B,p_O)$, Hardy–Weinberg. 표현형 도수 $(n_A,n_B,n_{AB},n_O)$만 관측 (A형 = AA 또는 AO, B형 = BB 또는 BO).
- 관측자료 가능도 $\propto(p_A^2+2p_Ap_O)^{n_A}(p_B^2+2p_Bp_O)^{n_B}(2p_Ap_B)^{n_{AB}}(p_O^2)^{n_O}$ — $\log(p_A^2+2p_Ap_O)$ 때문에 직접 풀기 어렵다.
- 유전형 도수까지 보면 $L_C\propto p_A^{2n_{AA}+n_{AO}+n_{AB}}p_B^{2n_{BB}+n_{BO}+n_{AB}}p_O^{n_{AO}+n_{BO}+2n_{OO}}$ → 완전자료 MLE $\hat p_A=\frac{2n_{AA}+n_{AO}+n_{AB}}{2n}$ 등 (유전자를 세는 것).
- **E-step:** $w_A^{(t)}=P(AA\mid A;p^{(t)})=\frac{p_A^{(t)}}{p_A^{(t)}+2p_O^{(t)}}$, $\tilde n_{AA}=n_Aw_A^{(t)}$, $\tilde n_{AO}=n_A(1-w_A^{(t)})$ (B도 같게).
- **M-step:** 기대 도수를 완전자료 MLE 공식에 대입. EM은 목적함수를 바꾸는 것이 아니라, 매 단계 쉬운 $L_C$ 최대화만 하면서 $L_I$의 MLE로 간다 ($L_I(p^{(t+1)})\ge L_I(p^{(t)}){}$).

### 4.5 알고리즘 비교와 실전 점검

[7강 p7–8]

| | Newton–Raphson | Gradient descent | Coordinate descent | EM |
| --- | --- | --- | --- | --- |
| 쓰는 정보 | gradient + Hessian | gradient만 | 좌표별 조건부 목적함수 | 조건부 기댓값 + 완전자료 최적화 |
| 한 반복 비용 | 큼 | 작음 | 좌표별 1변수 최적화 | E/M-step에 따라 |
| 강점 | 해 근처에서 빠름 | 고차원·대규모에 단순 | 좌표별 해가 쉬운 문제 | 잠재변수·결측자료 |
| 주의 | 초기값, Hessian, 발산 | 보폭, 지그재그 | 좌표 상관이 강하면 느림 | 국소해, 느린 수렴, 초기값 |

**점검 목록:** 여러 초기값으로 돌리고 $\ell(\theta^{(t)})$ 기록 · 제약 만족·score norm·수치 안정성 확인 · 종료 기준을 여러 개 (상대 우도 변화, 갱신량, score norm, 최대 반복) · 수렴 ≠ 전역 MLE (특히 혼합모형) · 추정 후 observed information, bootstrap, 프로파일 가능도로 불확실성 평가. 실제로는 섞어 쓴다 (EM 후 Newton 보정).

---

## 5. MLE의 일치성

### 5.1 KL과 경험위험

💡 [8강 p1–2]

목표 두 가지: **일치성** $\Pr(\lvert\hat\theta-\theta_0\rvert > \epsilon)\to0$, **점근분포** $\sqrt n(\hat\theta-\theta_0)\xrightarrow{d}N(0,v^2(\theta_0))$. 지금까지는 분산 → 0 + Chebyshev, LLN, CLT + Slutsky로 보였지만, 여기서는 **MLE 일반**에 대해 어떤 조건이면 되는지 본다.

- $\mathrm{KL}(f_{\theta_0}\Vert f_\theta)=\mathbb E_{\theta_0}\log\frac{f_{\theta_0}(X)}{f_\theta(X)}\ge0$, $=0\iff\theta=\theta_0$ (공통 지지집합 + 식별가능성, $-\log t\ge-t+1$로).
- **경험위험** $R_n(\theta)=\frac1n\sum\log\frac{f_{\theta_0}(X_i)}{f_\theta(X_i)}$ — 분자가 $\theta$와 무관해서 $R_n$ 최소화 = 가능도 최대화.
- **모집단위험** $R(\theta)=\mathbb E_{\theta_0}\log\frac{f_{\theta_0}}{f_\theta}=\mathrm{KL}(f_{\theta_0}\Vert f_\theta)$ — 식별 가능하면 $\theta_0$에서 유일하게 최소.
- $\hat\theta=\arg\min R_n$, $\theta_0=\arg\min R$. 각 $\theta$에서 LLN으로 $R_n(\theta)\xrightarrow{p}R(\theta)$이지만, **이 점별 수렴은 자료를 보고 고른 $\hat\theta_n$에 바로 쓸 수 없다** → 균등 수렴이 필요.

### 5.2 Thm 8.1 일치성

📐 [8강 p2–3]

**정칙조건** ① **강한 식별성:** 모든 $\epsilon > 0$에 $\eta_\epsilon > 0$이 있어 $\inf_{\lVert\theta-\theta_0\rVert\ge\epsilon}\mathrm{KL}(f_{\theta_0}\Vert f_\theta)\ge\eta_\epsilon$ ② **균등 대수의 법칙:** $\sup_\theta\lvert R_n(\theta)-R(\theta)\rvert\xrightarrow{p}0$ ③ MLE $\hat\theta_n$이 존재 (여럿이면 아무거나).
**결론** $\hat\theta_n\xrightarrow{p}\theta_0$.

**증명**
1. $\hat\theta_n$이 $R_n$의 최소점이라 $R_n(\hat\theta_n)\le R_n(\theta_0)=0$. 또 $R(\theta_0)=0$.
2. $R(\hat\theta_n)=R(\hat\theta_n)-R_n(\hat\theta_n)+R_n(\hat\theta_n)\le\lvert R(\hat\theta_n)-R_n(\hat\theta_n)\rvert\le\sup_\theta\lvert R(\theta)-R_n(\theta)\rvert$.
3. $\lVert\hat\theta_n-\theta_0\rVert\ge\epsilon$이면 강한 식별성으로 $R(\hat\theta_n)\ge\eta_\epsilon$. 따라서

$$\Pr(\lVert\hat\theta_n-\theta_0\rVert\ge\epsilon)\le\Pr\Big(\sup_\theta\lvert R_n(\theta)-R(\theta)\rvert\ge\eta_\epsilon\Big)\to0 .\qquad\blacksquare$$

- **Remark:** 강한 식별성은 모수공간이 콤팩트하고 $R$이 연속이며 $\theta_0$가 유일한 최소점이면 따라 나온다. 모수 차원이 $n$과 함께 커지거나 모수공간이 너무 복잡하면 균등 수렴이 깨질 수 있다 → 다음 반례.

### 5.3 반례와 모형 오설정

🔢💡 [8강 p3–4]

**Example 8.1 (Neyman–Scott, 쌍별 측정)** $Y_{i1},Y_{i2}\overset{\text{ind}}{\sim}N(\mu_i,\sigma^2)$, $i=1,\dots,n$, 관심은 $\sigma^2$. MLE $\hat\mu_i=\frac{Y_{i1}+Y_{i2}}2$,

$$\hat\sigma^2_{\mathrm{MLE}}=\frac1{2n}\sum_i\sum_j(Y_{ij}-\hat\mu_i)^2=\frac1{4n}\sum_i(Y_{i1}-Y_{i2})^2,\qquad\mathbb E\hat\sigma^2_{\mathrm{MLE}}=\frac{\sigma^2}2$$

($Y_{i1}-Y_{i2}$의 분산이 $2\sigma^2$). SLLN으로 $\hat\sigma^2\xrightarrow{\text{a.s.}}\sigma^2/2$ — **불일치**. 부수모수 $\mu_i$의 개수가 $n$과 함께 커져서 고정 차원의 논리가 깨진다.

**Remark (오설정)** 참분포 $q$가 모형 $\lbrace p_\theta\rbrace$ 밖에 있으면, 가능도 추정량은

$$\theta^\ast=\arg\max_\theta\mathbb E_q\log p_\theta(X)=\arg\min_\theta\mathrm{KL}(q\Vert p_\theta)$$

(모형 안에서 $q$에 가장 가까운 **KL 투영**)로 수렴한다. ($\mathrm{KL}(q\Vert p_\theta)=\mathbb E_q\log q-\mathbb E_q\log p_\theta$의 첫 항이 $\theta$와 무관)

---

## 6. 점근정규성

### 6.1 점수와 Fisher 정보

💡 [8강 p4]

- 한 관측치 로그가능도 $\ell(\theta;X)=\log f_\theta(X)$, **점수** $s(\theta;X)=\nabla_\theta\ell(\theta;X)$, 표본 점수 $S_n(\theta)=\sum_is(\theta;X_i)$.
- 정칙조건 아래 (Bartlett 항등식, 증명은 [7.1](#71-정칙조건과-bartlett-항등식))

$$\mathbb E_{\theta_0}[s(\theta_0;X)]=0,\qquad I(\theta_0)=\mathbb E_{\theta_0}\big[s\thinspace s^\top\big]=-\mathbb E_{\theta_0}\big[\nabla^2_\theta\ell(\theta_0;X)\big].$$

- 표본 전체의 Fisher 정보는 $nI(\theta_0)$. 점수는 자료에 따라 변하는 확률벡터, Fisher 정보는 그 분포를 요약한 모수의 함수.

### 6.2 Thm 8.2와 증명

📐 [8강 p4–5]

**정칙조건** ① 모수 차원 $d$는 $n$과 무관, $\theta_0$는 **내부점** ② $\ell(\theta;x)$가 $\theta_0$ 근방에서 (예: 3차까지) 매끄럽고 미분값에 적분가능한 지배함수 ③ 미분·적분 교환 → 점수 평균 0, 정보 등식 ④ 식별 가능, MLE 존재, $\hat\theta_n\xrightarrow{p}\theta_0$ ⑤ $I(\theta_0)$ 양정치.
**결론** $\sqrt n(\hat\theta_n-\theta_0)\xrightarrow{d}N_d\big(0,I(\theta_0)^{-1}\big)$.

**핵심 한 수** 점수를 $\theta_0$ 중심으로 1차 전개 → 분모는 LLN으로 $I$, 분자는 CLT로 $N(0,I)$ → Slutsky. [T27]

**증명 ($d=1$)**
1. 일치성 + 내부점 → 확률 → 1인 사건에서 $\hat\theta_n$은 경계가 아니고 **내부 최대점이라 $S_n(\hat\theta_n)=0$**.
2. $\theta_0$와 $\hat\theta_n$ 사이의 $\tilde\theta_n$으로 Taylor (평균값 정리):

$$0=S_n(\theta_0)+(\hat\theta_n-\theta_0)S_n'(\tilde\theta_n)\ \Rightarrow\ \sqrt n(\hat\theta_n-\theta_0)=\Big\lbrace-\frac1nS_n'(\tilde\theta_n)\Big\rbrace^{-1}\frac1{\sqrt n}S_n(\theta_0).\qquad(8.5)$$

3. **분모:** $-\frac1nS_n'(\tilde\theta_n)=\frac1n\sum-\ell''(\tilde\theta_n;X_i)\xrightarrow{p}I(\theta_0)$ (LLN, $\tilde\theta_n\xrightarrow{p}\theta_0$, 매끄러움).
4. **분자:** 점수는 평균 0, 분산 $I(\theta_0)$ → CLT로 $\frac1{\sqrt n}S_n(\theta_0)\xrightarrow{d}N(0,I(\theta_0))$.
5. **Slutsky:** $\sqrt n(\hat\theta_n-\theta_0)\xrightarrow{d}I(\theta_0)^{-1}N(0,I(\theta_0))=N(0,I(\theta_0)^{-1})$. ∎

- 다변량도 같은 전개를 벡터·행렬로 (중간 헤시안의 음수 → $I(\theta_0)$, 정규화 점수 → $N_d(0,I(\theta_0)){}$).
- **각 조건이 쓰이는 곳:** 일치성·내부점 (1단계 $S_n(\hat\theta)=0$, 3단계 $\tilde\theta\to\theta_0$), 매끄러움 (2·3단계), 정보 양정치 (5단계 역행렬).
- **Remark (경계·비정칙):** 경계에서 최대이면 점수가 0일 필요가 없어 전개가 무너진다. 지지집합이 모수에 따라 바뀌어도 깨진다 → 점근정규성은 MLE의 정의만으로 보장되는 성질이 아니다.

**셀프 체크**
<details><summary>Q1. 증명의 1단계에서 왜 확률 → 1인 사건에서만 말하나?</summary>

$S_n(\hat\theta_n)=0$은 $\hat\theta_n$이 내부점일 때만 성립하는데, 그것은 일치성과 "θ₀가 내부점"으로부터 확률 → 1로만 보장된다.

</details>
<details><summary>Q2. 분모가 I(θ₀)로 가는 데 쓰는 세 가지는?</summary>

대수의 법칙, $\tilde\theta_n\xrightarrow{p}\theta_0$ (일치성), 로그가능도의 매끄러움 (2계도함수의 연속성·지배조건).

</details>

### 6.3 예와 반례

🔢 [8강 p5–6]

| 모형 | MLE | $I_1$ | 극한분포 |
| --- | --- | --- | --- |
| Bernoulli $(p)$ (Ex 8.2) | $\bar X_n$ | $\frac1{p(1-p)}$ | $\sqrt n(\hat p-p)\xrightarrow{d}N(0,p(1-p))$, $0 < p < 1$ |
| 지수, 율모수 $\theta e^{-\theta x}$ (Ex 8.3) | $1/\bar X_n$ | $\frac1{\theta^2}$ | $N(0,\theta^2)$, 즉 $\hat\theta\ \dot\sim\ N(\theta,\theta^2/n)$ |
| Poisson $(\lambda)$ (Ex 11.1) | $\bar X_n$ | $\frac1\lambda$ | $N(0,\lambda)$ |
| Beta $(\theta,1)$ (Ex 11.1) | $-n/\sum\log X_i$ | $\frac1{\theta^2}$ | $N(0,\theta^2)$ (유한 표본에서는 편향 $O(n^{-1}){}$) |
| 로지스틱 위치 (Ex 11.2) | 닫힌 꼴 없음 | $\frac13$ | $N(0,3)$ — MLE를 못 풀어도 점수·정보로 극한분포를 안다 |

- 지수분포의 점수 계산: $\ell_n=n\log\theta-\theta\sum X_i$, 한 관측치 점수 $\frac1\theta-X$, $I=\mathbb E(\frac1\theta-X)^2=\mathop{\mathrm{Var}}\nolimits(X)=\frac1{\theta^2}$.
- " $\dot\sim$"는 큰 표본의 근사분포라는 뜻이지 유한 표본에서 정확히 정규라는 뜻이 아니다.

**Example 8.4 (균등분포 반례)** $X_i\overset{\text{iid}}{\sim}\text{Uniform}(0,\theta)$: $L_n(\theta)=\theta^{-n}I(\theta\ge X_{(n)})$ → $\hat\theta=X_{(n)}$. $0\le t\le n\theta$에서

$$\Pr\lbrace n(\theta-\hat\theta)\gt t\rbrace=\Pr\lbrace X_i\lt \theta-t/n\ \forall i\rbrace=\Big(1-\frac t{n\theta}\Big)^n\to e^{-t/\theta}.$$

$n(\hat\theta-\theta)$는 평균 $\theta$인 지수분포의 음수로 수렴하고 $\sqrt n(\hat\theta-\theta)\xrightarrow{p}0$ — **비퇴화 정규 극한이 아니다**. 로그가능도가 추정값에서 미분 불가능하고 지지집합이 $\theta$에 의존해서 정칙조건이 깨졌다. (ch3 Ex 3.5와 같은 계산)

---

## 7. 정칙조건과 Cramér–Rao 하한

### 7.1 정칙조건과 Bartlett 항등식

📐 [8강 p6–7]

**(R0)** 식별 가능 · **(R1)** $f(x;\theta)$의 지지집합이 $\theta$와 무관 · **(R2)** $\Theta$는 $\mathbb R^d$의 열린 집합 · **(R3)** $\log f$가 $\theta$에 대해 두 번 미분 가능 · **(R4)** 필요한 기댓값을 적분 아래에서 두 번 미분 가능. (편리한 충분조건 묶음이지 최소조건이 아니다. (R1)·(R4)가 Bartlett 항등식을 정당화한다)

**Bartlett 항등식 (9.1)** $\mathbb E_\theta[s_1(\theta;X)]=0$, $I_1(\theta)=\mathbb E_\theta[s_1s_1^\top]=-\mathbb E_\theta[\nabla^2_\theta\log f(X;\theta)]$.

**(보충 증명)** $1=\int f(x;\theta)dx$를 미분 ((R4), 지지집합 고정 (R1)): $0=\int\nabla f=\int s_1f=\mathbb E s_1$. 한 번 더: $0=\int(\nabla^2\log f+s_1s_1^\top)f$ → $\mathbb E[s_1s_1^\top]=-\mathbb E[\nabla^2\log f]$. ∎ (튜터링2 Thm 4와 같다)

### 7.2 Thm 9.1 Cramér–Rao 정보부등식

📐 [8강 p7–8]

**가정** $I_1(\theta)$ 가역, $U_n$은 이차모멘트가 유한한 스칼라 통계량, 미분·적분 교환 가능.
**결론**

$$\mathop{\mathrm{Var}}\nolimits_ \theta(U_n)\ \ge\ \lbrace\nabla_\theta\mathbb E_\theta U_n\rbrace^\top\big(nI_1(\theta)\big)^{-1}\lbrace\nabla_\theta\mathbb E_\theta U_n\rbrace .\qquad(9.2)$$

$\mathbb E_\theta U_n=g(\theta)$이면 우변은 $\nabla g^\top(nI_1)^{-1}\nabla g$. 1차원 불편추정량이면 $\mathop{\mathrm{Var}}\nolimits\ge\frac1{nI_1(\theta)}$.

**증명**
1. **공분산:** 점수 평균이 0이라 $\mathrm{Cov}_ \theta(S_n,U_n)=\mathbb E_\theta[S_nU_n]=\int U_n\nabla_\theta\prod f=\nabla_\theta\mathbb E_\theta U_n$ (미분·적분 교환).
2. **회귀 잔차의 분산 ≥ 0** (또는 Cauchy–Schwarz): $\mathop{\mathrm{Var}}\nolimits(U_n)\ge\mathrm{Cov}(U_n,S_n)\mathop{\mathrm{Var}}\nolimits(S_n)^{-1}\mathrm{Cov}(S_n,U_n)$.
3. **독립:** $\mathop{\mathrm{Var}}\nolimits_ \theta(S_n)=nI_1(\theta)$. 대입하면 (9.2). ∎

- 벡터 $U_n\in\mathbb R^m$: 임의의 선형결합 $a^\top U_n$에 스칼라 결과를 쓰면 $\mathop{\mathrm{Var}}\nolimits(U_n)\succeq\nabla g(nI_1)^{-1}\nabla g^\top$ ($\nabla g$는 $m\times d$ 야코비 행렬).
- **하한은 항상 달성되지 않는다.**

### 7.3 예

🔢 [8강 p8]

- **Poisson:** $I_1=1/\lambda$ → 불편추정량의 분산 $\ge\lambda/n$. $\bar X$의 분산이 정확히 $\lambda/n$ → **달성**.
- **Bernoulli:** $I_1=\frac1{p(1-p)}$ → $\bar X$가 하한 $\frac{p(1-p)}n$ 달성 ($0 < p < 1$).
- **정규 (Ex 9.2) — 달성 안 됨:** $I_1(\mu,\sigma^2)=\mathrm{diag}\big(\frac1{\sigma^2},\frac1{2\sigma^4}\big)$ → 하한 $\frac{\sigma^2}n$, $\frac{2\sigma^4}n$. $\bar X$는 첫 하한 달성, 불편 $S^2$은 분산 $\frac{2\sigma^4}{n-1}$ > $\frac{2\sigma^4}n$. 하한이 최선의 불편추정량 분산을 정확히 나타내지 않을 수 있다.

---

## 8. 점근효율과 한 단계 추정량

### 8.1 점근효율과 Fisher의 추측

💡 [8강 p8–9]

- **델타 방법:** $\sqrt n\lbrace g(\hat\theta_n)-g(\theta)\rbrace\xrightarrow{d}N\big(0,\nabla g^\top I_1^{-1}\nabla g\big)$ — Cramér–Rao 하한의 $n$배. 정칙 MLE는 하한을 1차 근사에서 **달성** (점근적으로 효율적).
- **효율** $\mathrm{eff}(\hat\eta)=\lim_n\frac{\nabla g^\top(nI_1)^{-1}\nabla g}{\mathop{\mathrm{Var}}\nolimits_ \theta(\hat\eta)}$ (10.1). 작은 편향이 있어도 편향이 표준오차보다 작으면 비슷하게 정의한다.
- **Fisher의 추측** "모든 점근정규 추정량의 분산 ≥ 정보 하한"은 **추정량 전체에 대해서는 거짓** — Hodges 추정량 같은 비정칙 추정량은 특정 모수점에서 더 작은 점근분산을 보인다. 효율 비교는 정칙 추정량의 부류 안에서 한다.

### 8.2 위치모형의 ARE

🔢 [8강 p9]

$f(x;\theta)=f_0(x-\theta)$, $\mathrm{ARE}(\text{비교},\text{MLE})=\frac{\text{MLE의 점근분산}}{\text{비교 추정량의 점근분산}}$.

| $f_0$ | 표본평균 대 MLE | 표본중앙값 대 MLE |
| --- | --- | --- |
| $N(0,1)$ | 1 | $2/\pi$ |
| 표준 로지스틱 | $9/\pi^2$ | $3/4$ |
| 표준 이중지수 | $1/2$ | 1 |

(보충, 계산) 점근분산: 중앙값 $\frac1{4f_0(0)^2}$, 평균 $\mathop{\mathrm{Var}}\nolimits_ {f_0}(X)$, MLE $I_1^{-1}$. 로지스틱이면 $f_0(0)=\frac14$ → 중앙값 4, 평균 $\frac{\pi^2}3$, MLE 3 → $\frac3{\pi^2/3}=\frac9{\pi^2}$, $\frac34$ ✓. 평균의 효율은 꼬리 모양에 따라 달라지고, 이중지수에서는 중앙값이 곧 MLE다.

### 8.3 한 단계 추정량

📐 [8강 p10]

초기값 $\hat\theta^{[0]}$에서 Newton 한 번:

$$\hat\theta^{[1]}=\hat\theta^{[0]}-\lbrace\nabla^2\ell_n(\hat\theta^{[0]})\rbrace^{-1}\nabla\ell_n(\hat\theta^{[0]}).\qquad(12.1)$$

$\hat\theta^{[0]}-\theta_0=O_P(n^{-1/2})$ ($\sqrt n$-일치)이고 국소 정칙조건이 성립하면 $\sqrt n(\hat\theta^{[1]}-\theta_0)\xrightarrow{d}N(0,I_1(\theta_0)^{-1})$ — **MLE와 같은 1차 점근분포**. 증명은 초기값 주변에서 점수를 전개하고, 초기 오차의 제곱항이 $o_P(n^{-1/2})$임을 쓴다 (튜터링2 3.3에 뼈대).

**Example 12.1 (로지스틱 위치)** 초기값 $\bar X_n$ ($\sqrt n$-일치). $\ell_n'(\theta)=n-2\sum\frac{e^{-(X_i-\theta)}}{1+e^{-(X_i-\theta)}}$, $\ell_n''(\theta)=-2\sum\frac{e^{-(X_i-\theta)}}{(1+e^{-(X_i-\theta)})^2}$을 (12.1)에 넣으면 한 번의 계산으로 $N(0,3)$ 점근분포.

### 8.4 정리

[8강 p10–11]

점근정규성은 **점수의 CLT**와 **헤시안의 수렴**에서 나온다. 정칙 모형에서 MLE는 Fisher 정보가 정하는 점근분산을 가지며 Cramér–Rao 하한을 (정칙 추정량의 범위에서) 점근적으로 달성한다. 비정칙 모형에서는 깨질 수 있고, 비선형 점수방정식도 $\sqrt n$-일치 초기값에서 Newton 한 번이면 같은 1차 근사를 얻는다.

---

## hw2 풀이

📝 [hw2, 마감 10/12 = 중간고사 1 당일] · 제출 풀이는 아직 없어서 "내 풀이 점검" 칸은 비워 둔다. hw2는 "각 문제의 안내를 따라 단계별로" 쓰라고 한다 — hw1과 같은 유도형이다.

### hw2 1 x의 마이너스 2제곱 밀도

$f(x\mid\theta)=\theta x^{-2}$, $0 < \theta\le x < \infty$. (a) 충분통계량 (인수분해) (b) MLE (c) 적률이용추정량 (적률이 존재하는지 먼저).

**전략** support가 $\theta$에서 시작 → 지시함수 + $\min$. MLE는 미분하지 말고 우도 모양. 적률은 $\mathbb EX$부터 확인.

**모범 답안**

(a) $f(x_1,\dots,x_n\mid\theta)=\theta^n\prod x_i^{-2}\prod I(x_i\ge\theta)=\underbrace{\prod x_i^{-2}}_ {h(x)}\cdot\underbrace{\theta^nI(x_{(1)}\ge\theta)}_ {g_\theta(x_{(1)})}$ (∵ $\prod I(x_i\ge\theta)=I(\min x_i\ge\theta){}$). 인수분해정리로 $X_{(1)}$이 충분통계량.

(b) $L(\theta)=\theta^n\prod x_i^{-2}$ ($\theta\le x_{(1)}$), $0$ ($\theta > x_{(1)}$). $(0,x_{(1)}]$에서 $\theta$에 대해 증가하므로 최대점은 경계 $\hat\theta=X_{(1)}$. (가능도방정식 $n/\theta=0$은 근이 없다)

(c) $\mathbb EX=\int_\theta^\infty x\cdot\theta x^{-2}dx=\theta\int_\theta^\infty\frac{dx}x=\infty$ → **1차 적률이 존재하지 않아 보통의 적률법 ($\bar X=\mathbb EX$)은 쓸 수 없다.** 대신 존재하는 적률을 쓴다: $\mathbb E[X^{-1}]=\int_\theta^\infty\theta x^{-3}dx=\frac1{2\theta}$ → $\frac1n\sum X_i^{-1}=\frac1{2\theta}$를 풀어

$$\tilde\theta=\frac{1}{\frac2n\sum_{i=1}^nX_i^{-1}}=\frac n{2\sum X_i^{-1}} .$$

(일반적으로 $r < 1$이면 $\mathbb EX^r=\frac{\theta^r}{1-r}$이 존재한다)

**채점 포인트** 지시함수를 쓰고 $\min$으로 바꾸기 · MLE에서 "증가함수 → 경계" 논증 · (c)에서 $\mathbb EX=\infty$를 **계산으로** 보이고 대안 적률을 제시.

### hw2 2 두 값 중 하나인 모수

$\theta\in\lbrace0,1\rbrace$: $\theta=0$이면 $\text{Uniform}(0,1)$, $\theta=1$이면 $f(x)=\frac1{2\sqrt x}$ ($0 < x < 1$). $\theta$의 MLE와 가능도가 같은 경우.

**모범 답안** 모든 $x_i\in(0,1)$일 때 $L(0)=1$, $L(1)=\prod\frac1{2\sqrt{x_i}}=2^{-n}\big(\prod x_i\big)^{-1/2}$.

$$L(1)\gt L(0)\iff\prod_{i=1}^nx_i\lt 4^{-n}\iff\Big(\prod x_i\Big)^{1/n}\lt \frac14 .$$

따라서 기하평균 $G=(\prod x_i)^{1/n}$이 $G < \frac14$이면 $\hat\theta=1$, $G > \frac14$이면 $\hat\theta=0$. 정확히 $G=\frac14$**이면** $L(0)=L(1)$이라 두 값 모두 MLE (연속분포라 확률 0).

**채점 포인트** 모수공간이 이산 → **미분하지 말고 두 가능도 비교** · 비를 표본의 함수 (기하평균)로 정리 · 동률 명시. (2021 중간 #1과 같은 유형)

### hw2 3 경쟁위험의 최솟값 관측

$X\sim$ 평균 $\lambda$ 지수, $Y\sim$ 평균 $\mu$ 지수, 독립. $Z=\min(X,Y)$, $W=I(X < Y)$만 관측, $(Z_i,W_i)$ iid. (a) $(Z,W)$의 혼합밀도 (b) 가능도와 MLE, 한 원인이 0번 관측될 때.

**모범 답안**

(a) $w=1$: $X=z$이고 $Y > z$ → $\frac1\lambda e^{-z/\lambda}\cdot e^{-z/\mu}$. $w=0$: $\frac1\mu e^{-z/\mu}\cdot e^{-z/\lambda}$. 합치면

$$f(z,w)=\lambda^{-w}\mu^{-(1-w)}\exp\Big\lbrace-z\Big(\frac1\lambda+\frac1\mu\Big)\Big\rbrace,\qquad z\gt 0,\ w\in\lbrace0,1\rbrace .$$

(b) $k=\sum w_i$ ($X$가 먼저 일어난 횟수), $S=\sum z_i$:

$$\ell(\lambda,\mu)=-k\log\lambda-(n-k)\log\mu-S\Big(\frac1\lambda+\frac1\mu\Big).$$

두 모수가 분리되어 있다. $\lambda$에 대해: 비율 $a=1/\lambda$로 바꾸면 $k\log a-aS$로 순오목 ([T26]), $\partial_\lambda\ell=-\frac k\lambda+\frac S{\lambda^2}=0$ → $\hat\lambda=S/k$. 같은 방법으로 $\hat\mu=S/(n-k)$.
- **$k=0$이면:** $\partial_\lambda\ell=S/\lambda^2 > 0$ — $\lambda$에 대해 증가만 하므로 $\hat\lambda=\infty$, **유한한 MLE가 없다**. $k=n$이면 $\mu$에 대해 같은 현상.
- 해석: $\hat\lambda$ = (총 관측 시간) / (원인 $X$가 관측된 횟수) — 중도절단 자료의 표준 공식. (튜터링8 문제5가 같은 설정)

### hw2 4 Satterthwaite 근사

$X_i\sim\chi^2_{\nu_i}$ 독립, $Q=\sum a_iX_i$ ($a_i > 0$), $\mu=\mathbb EQ=\sum a_i\nu_i$, $W=Q/\mu$를 $\chi^2_\nu/\nu$로 근사.

**모범 답안**

(a) $\mathbb EX_i=\nu_i$, $\mathop{\mathrm{Var}}\nolimits X_i=2\nu_i$ → $\mathbb EQ=\sum a_i\nu_i=\mu$, $\mathop{\mathrm{Var}}\nolimits Q=2\sum a_i^2\nu_i$ (독립). 따라서 $\mathbb EW=1$, $\mathop{\mathrm{Var}}\nolimits W=\frac{2\sum a_i^2\nu_i}{\mu^2}$. 그리고 $\mathbb E[\chi^2_\nu/\nu]=1$, $\mathop{\mathrm{Var}}\nolimits[\chi^2_\nu/\nu]=\frac{2\nu}{\nu^2}=\frac2\nu$.

(b) 1차 적률은 자동으로 같다. 2차를 맞추면 $\frac2\nu=\frac{2\sum a_i^2\nu_i}{(\sum a_i\nu_i)^2}$:

$$\nu=\frac{\big(\sum_ia_i\nu_i\big)^2}{\sum_ia_i^2\nu_i} .$$

(c) $a_i=a$이면 $Q=a\sum X_i\sim a\chi^2_{\sum\nu_i}$ ($\chi^2$의 가법성), $W=\chi^2_{\sum\nu_i}/\sum\nu_i$로 **정확히** 그 꼴이고 공식도 $\nu=\frac{a^2(\sum\nu_i)^2}{a^2\sum\nu_i}=\sum\nu_i$. 일반적인 $a_i$에서는 서로 다른 배수를 곱한 $\chi^2$의 합이 $\chi^2$의 배수가 아니므로, 이 방법은 **처음 두 적률만 맞춘 근사**다 (적률법의 사고방식: 모르는 분포를 적률이 같은 다루기 쉬운 분포로 대체).

### hw2 5 결측값이 있는 포아송과 EM

$X_i\sim\text{Poi}(\tau_i)$, $Y_i\sim\text{Poi}(\beta\tau_i)$ 독립, $X_1$만 결측. (a) 완전자료 MLE (b) 결측값의 조건부 기댓값 (c) EM 갱신식.

**전략** [4.4](#44-em-알고리즘)의 틀 그대로: 완전자료 로그가능도가 $x_1$에 대해 1차식 → E-step = $x_1$을 조건부 기댓값으로 바꾸기, M-step = (a)의 공식에 대입.

**모범 답안**

(a) 상수 ($-\log x_i!$ 등)를 빼면

$$\ell_C=\sum_{i=1}^n\big[x_i\log\tau_i-\tau_i+y_i\log\beta+y_i\log\tau_i-\beta\tau_i\big].$$

$\partial_{\tau_i}$: $\frac{x_i+y_i}{\tau_i}-(1+\beta)=0$ → $\tau_i=\frac{x_i+y_i}{1+\beta}$. $\partial_\beta$: $\frac{\sum y_i}\beta-\sum\tau_i=0$. 첫 식을 더하면 $\sum\tau_i=\frac{\sum x_i+\sum y_i}{1+\beta}$이고 대입하면 $\beta\sum x_i=\sum y_i$:

$$\hat\beta=\frac{\sum_iy_i}{\sum_ix_i},\qquad\hat\tau_i=\frac{x_i+y_i}{1+\hat\beta}.$$

(b) $X_1$은 다른 모든 관측과 독립이므로 $\mathbb E[X_1\mid\text{관측};\beta^{(r)},\tau^{(r)}]=\mathbb E[X_1;\tau_1^{(r)}]=\tau_1^{(r)}$.

(c) $\ell_C$가 $x_1$에 대해 1차식이라 $Q$ = $x_1$ 자리에 $\tau_1^{(r)}$를 넣은 $\ell_C$. 따라서

$$\beta^{(r+1)}=\frac{\sum_{i=1}^ny_i}{\tau_1^{(r)}+\sum_{i=2}^nx_i},\qquad\tau_1^{(r+1)}=\frac{\tau_1^{(r)}+y_1}{1+\beta^{(r+1)}},\qquad\tau_i^{(r+1)}=\frac{x_i+y_i}{1+\beta^{(r+1)}}\ (i\ge2).$$

**(보충) 수렴점 확인:** 고정점에서 $\tau_1=\frac{\tau_1+y_1}{1+\beta}$ → $\tau_1=y_1/\beta$, 그리고 $\beta(\tau_1+\sum_{i\ge2}x_i)=\sum y_i$ → $\beta=\frac{\sum_{i\ge2}y_i}{\sum_{i\ge2}x_i}$. 이것이 관측자료 MLE다 ($Y_1$만으로는 $\beta\tau_1$만 알 수 있어 $\beta$는 나머지 쌍에서 정해진다). (수치 시뮬레이션으로 수렴 확인)

**채점 포인트** (a) 두 방정식을 연립해서 닫힌 꼴 · (b) 독립이라 조건부 기댓값 = 현재 모수의 평균 · (c) **왜 대입이 정당한지** ($\ell_C$가 $x_1$에 대해 1차식) 한 줄.

---

## 변형 문제

난이도: `●` hw·예제의 분포만 바꿈 · `●●` 도구 두세 개 조합 · `●●●` 장을 넘나듦. 힌트부터 열고, 스스로 써 본 뒤 풀이를 연다. (MLE 계산 연습은 [튜터링1 추천 문제](튜터링/튜터링1%20적률추정량과%20최대가능도추정량.md#6-추천-문제-풀이)에 13개 더 있다)

### V1 분산의 MLE를 정밀도로

`●●` $X_i\overset{\text{iid}}{\sim}N(0,\sigma^2)$. (a) $\ell(\sigma^2)$가 $\sigma^2$에 대해 오목이 아님을 보여라. (b) $\tau=1/\sigma^2$로 바꿔 존재·유일성을 보이고 $\hat\sigma^2$을 구하라.

<details><summary>풀이</summary>

$T=\sum x_i^2 > 0$. (a) $\ell(\sigma^2)=-\frac n2\log\sigma^2-\frac T{2\sigma^2}$, $\ell''=\frac n{2\sigma^4}-\frac T{\sigma^6}$ — $\sigma^2 > 2T/n$이면 양수. (b) $\ell(\tau)=\frac n2\log\tau-\frac{T\tau}2$, $\ell''=-\frac n{2\tau^2} < 0$, $\ell'(\tau)\to+\infty$ ($\tau\to0$), $\to-T/2 < 0$ ($\tau\to\infty$) → 유일한 근 $\hat\tau=n/T$, 불변성으로 $\hat\sigma^2=\frac1n\sum x_i^2$.

</details>

### V2 Neyman–Scott, k번 측정

`●●` $Y_{ij}\overset{\text{ind}}{\sim}N(\mu_i,\sigma^2)$, $j=1,\dots,k$. $\hat\sigma^2_{\mathrm{MLE}}$의 극한은? 어떻게 고치나?

<details><summary>풀이</summary>

$\hat\mu_i=\bar Y_{i\cdot}$, $\hat\sigma^2=\frac1{nk}\sum_i\sum_j(Y_{ij}-\bar Y_{i\cdot})^2$. 각 $i$의 제곱합 기댓값이 $(k-1)\sigma^2$이라 SLLN으로 $\hat\sigma^2\to\frac{k-1}k\sigma^2$. $k$가 고정이면 불일치 ($k=2$이면 $\sigma^2/2$). $\frac k{k-1}\hat\sigma^2$ (합동 표본분산)은 일치.

</details>

### V3 혼합비율의 EM

`●●●` $f(x)=\pi f_1(x)+(1-\pi)f_2(x)$, $f_1,f_2$는 알려진 밀도, $\pi\in(0,1)$ 미지. 잠재변수 $Z_i=1$ (성분 1)을 도입해 EM 갱신식을 구하라.

<details><summary>전략 힌트</summary>

완전자료 $\ell_C=\sum[z_i\log\pi+(1-z_i)\log(1-\pi)]+\text{const}$ — $z_i$에 대해 1차. 완전자료 MLE는 $\bar z$.

</details>
<details><summary>풀이</summary>

E-step: $w_i^{(t)}=P(Z_i=1\mid x_i;\pi^{(t)})=\frac{\pi^{(t)}f_1(x_i)}{\pi^{(t)}f_1(x_i)+(1-\pi^{(t)})f_2(x_i)}$. M-step: $\pi^{(t+1)}=\frac1n\sum w_i^{(t)}$. Thm 7.2로 관측자료 우도가 매 단계 증가한다.

</details>

### V4 Cramér–Rao 하한을 달성하지 못하는 불편추정량

`●●` $X_i\overset{\text{iid}}{\sim}$ 율 $\theta$의 지수분포. (a) $\theta$의 불편추정량 $\tilde\theta=\frac{n-1}{\sum X_i}$을 확인하라. (b) 분산과 Cramér–Rao 하한을 비교하라.

<details><summary>풀이</summary>

$T=\sum X_i\sim\text{Gamma}(n,\text{rate }\theta)$: $\mathbb E[1/T]=\frac\theta{n-1}$, $\mathbb E[1/T^2]=\frac{\theta^2}{(n-1)(n-2)}$. (a) $\mathbb E\tilde\theta=\theta$. (b) $\mathop{\mathrm{Var}}\nolimits\tilde\theta=\frac{\theta^2}{n-2}$ > 하한 $\frac{\theta^2}n$ ($I_1=1/\theta^2$). 효율 $\frac{n-2}n\to1$. ($T$가 완비충분이라 이것이 UMVUE — 그래도 하한에 못 미친다. 시뮬레이션 확인)

</details>

### V5 지지집합이 모수에 의존하면 하한이 깨진다

`●●` $\text{Uniform}(0,\theta)$에서 $\tilde\theta=\frac{n+1}nX_{(n)}$은 불편이다. 분산을 구하고, (형식적으로 계산한) $I_1=1/\theta^2$의 "하한" $\theta^2/n$과 비교하라.

<details><summary>풀이</summary>

$\mathop{\mathrm{Var}}\nolimits X_{(n)}=\frac{n\theta^2}{(n+1)^2(n+2)}$ → $\mathop{\mathrm{Var}}\nolimits\tilde\theta=\frac{\theta^2}{n(n+2)}$ — "하한"보다 **작다**. 지지집합이 $\theta$에 의존해 (R1)·(R4)가 깨지므로 Cramér–Rao가 적용되지 않는다 (속도도 $\sqrt n$이 아니라 $n$).

</details>

### V6 델타 방법으로 P(X=0)

`●` $X_i\overset{\text{iid}}{\sim}\text{Poi}(\lambda)$, $\eta=e^{-\lambda}$. MLE와 극한분포, 효율을 구하라.

<details><summary>풀이</summary>

불변성 $\hat\eta=e^{-\bar X}$. 델타 방법: $\sqrt n(\hat\eta-\eta)\xrightarrow{d}N(0,\lambda e^{-2\lambda})$ ($g'(\lambda)^2/I_1=e^{-2\lambda}\lambda$) — Cramér–Rao 하한의 $n$배와 같아 점근적으로 효율적. (UMVUE $(1-1/n)^{\sum X_i}$의 분산 $e^{-2\lambda}(e^{\lambda/n}-1)\approx\lambda e^{-2\lambda}/n$과도 1차까지 같다)

</details>

---

## 셀프 테스트

### 백지 증명 체크리스트

| ☐ | 증명 | 뼈대 | 위치 |
| --- | --- | --- | --- |
| ☐ | **존재·유일성 (Thm 6.1)** | 콤팩트 $K$로 가두기 (귀류) → 최대값 정리 → 내부점, Taylor + 음정치 | [1.3](#13-존재와-유일성) |
| ☐ | 프로파일 MLE = 전체 MLE | $L_p(\eta)\le L(\hat\eta,\hat\xi)=L_p(\hat\eta)$ | [2.1](#21-프로파일-가능도) |
| ☐ | 불변성 (일대일 / 아닐 때) | $L^\ast(g(\hat\theta))=L(\hat\theta)\ge L(\theta)$ / 유도 가능도 = 프로파일 | [2.2](#22-불변성) |
| ☐ | **지수족 MLE (Thm 7.1)** | CGF → $\nabla^2\ell=-n\mathop{\mathrm{Var}}\nolimits(t)$ → 순오목 | [3.1](#31-thm-71) |
| ☐ | 중앙값이 MLE (이중지수) | 구간별 기울기 $j-(n-j)$ | [3.2](#32-예) |
| ☐ | Newton 반복식 | 점수의 1차 Taylor | [4.1](#41-newtonraphson) |
| ☐ | **EM 단조성 (보충)** | $\ell=Q-V$, $V$는 KL로 감소 안 함, $Q$는 M-step으로 증가 | [4.4](#44-em-알고리즘) |
| ☐ | **일치성 (Thm 8.1)** | $R_n(\hat\theta)\le0$ → $R(\hat\theta)\le\sup\lvert R-R_n\rvert$ → 강한 식별성 | [5.2](#52-thm-81-일치성) |
| ☐ | **점근정규성 (Thm 8.2)** | $S_n(\hat\theta)=0$ → 평균값 정리 → LLN, CLT → Slutsky | [6.2](#62-thm-82와-증명) |
| ☐ | 균등분포 반례 | $(1-t/(n\theta))^n\to e^{-t/\theta}$ | [6.3](#63-예와-반례) |
| ☐ | Bartlett 항등식 | $\int f=1$을 두 번 미분 | [7.1](#71-정칙조건과-bartlett-항등식) |
| ☐ | **Cramér–Rao (Thm 9.1)** | $\mathrm{Cov}(S_n,U_n)=\nabla\mathbb EU_n$ → 회귀 잔차 분산 ≥ 0 → $\mathop{\mathrm{Var}}\nolimits S_n=nI_1$ | [7.2](#72-thm-91-cramérrao-정보부등식) |

### 짧은 문답

<details><summary>1. MLE가 존재하지 않는 전형적 상황은?</summary>

최대가 열린 모수공간의 경계에서만 달성될 때 (베르누이 $S=0$, hw2 #3의 $k=0$).

</details>
<details><summary>2. 지수분포 평균 θ의 MLE에서 왜 λ = 1/θ로 바꾸나?</summary>

$\ell(\theta)$는 오목이 아니어서 유일성 정리를 바로 못 쓰지만 $\ell(\lambda)$는 순오목. 불변성으로 되돌린다.

</details>
<details><summary>3. EM의 E-step이 "결측값을 조건부 기댓값으로 바꾸기"가 되는 조건은?</summary>

완전자료 로그가능도가 결측값 (또는 그 충분통계량)에 대해 1차식일 때.

</details>
<details><summary>4. Neyman–Scott에서 MLE가 불일치인 이유는?</summary>

부수모수 $\mu_i$의 개수가 $n$과 함께 커져 고정 차원 일치성 논리 (균등 수렴)가 깨진다.

</details>
<details><summary>5. 점근정규성 증명에서 "θ₀가 내부점"은 어디에 쓰이나?</summary>

$S_n(\hat\theta_n)=0$ (내부 최대점)을 쓰기 위해. 경계면 점수가 0이 아니다.

</details>
<details><summary>6. Cramér–Rao 하한이 달성되는 예와 안 되는 예는?</summary>

달성: 포아송·베르누이의 $\bar X$. 안 됨: 정규의 $S^2$ ($\frac{2\sigma^4}{n-1}$ vs $\frac{2\sigma^4}n$), 지수 율모수의 불편추정량 (V4).

</details>
<details><summary>7. 로지스틱 위치모형에서 평균·중앙값·MLE의 점근분산은?</summary>

$\pi^2/3$, $4$, $3$.

</details>

---

## 강의노트 확인

강의노트 세 개를 텍스트로 전부 읽었다 (수식이 유니코드로 잘 추출됨). 수식 오류는 찾지 못했다. 철자 오타 (condtions)는 조용히 고쳤다.

| 쪽 | 강의노트 원문 | 노트에 반영한 것 | 근거 | 영향 |
| --- | --- | --- | --- | --- |
| 6강 p5 끝 | "이를 다음 정리로 정리한다."에서 문서가 끝남 | 그 정리는 7강 Thm 7.1 | 두 강의의 연결 | · |
| 7강 p6 Thm 7.2 | EM 단조성의 증명 없음 | 보충 증명 ($\ell=Q-V$, KL ≥ 0) | 무증명 정리 = 출제 후보 | ★ |
| 8강 제목 | "MLE의 점근이론 I" | 일치성·점근정규성·Cramér–Rao·효율까지 모두 포함 | 본문 | · |
| 8강 | 정보를 $I(\theta)$와 $I_1(\theta)$로 섞어 씀 | 둘 다 한 관측치 정보 | 9절에서 $I_1$로 다시 정의 | · |
