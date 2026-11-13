# 1 MLE 점근이론과 정보부등식

> **범위:** 8강 후반 (일치성), 9강 (점근정규성, Cramér–Rao), 부록 (정칙조건 모음), 13강 앞부분 (CR 등호 조건). 필기 ✍p1–7, p22.
> **자세한 판:** [ch5.3–5.5 §5–§8](../ch5.3-5.5%20최대가능도추정.md#5-mle의-일치성), [ch5.6](../ch5.6%20MLE의%20점근이론%20II.md). 이 파일은 **수업에서 쓴 순서와 표현**으로 다시 묶었다.
> **표시:** ✍ 수업 필기 · 📄 강의자료만 · 📝 연습시험·hw · (보충) 수업·자료에 없음. 목차는 [README](README.md).

---

## 1. 큰 그림

수업 (10/16, ✍p3)은 MLE 이야기를 이렇게 정리했다: **존재·유일성 → 일치성 ( $\hat\theta\to\theta$?) → 극한분포 ( $\hat\theta-\theta$는 어떻게 퍼지나?) → 하한 (그보다 잘할 수 있나?)**.

| 단계 | 정리 | 핵심 조건 | 핵심 한 수 |
| --- | --- | --- | --- |
| 존재 | 부록 Thm 1.1 (6강 Thm 6.1) | $\Theta$ 열린 집합, $\ell_n$ 연속, 콤팩트 밖으로 나가면 $\ell_n\to-\infty$ | 최대값 정리 |
| 유일 | 부록 Thm 1.2 | $\Theta$ 볼록, $\ell_n$ 순오목 ( $\nabla^2\ell_n\prec0$) | 점수 방정식의 근 = 유일한 MLE |
| 지수족 | 부록 Thm 2.1 (7강 Thm 7.1) | $\mathrm{Var}_ \alpha t(X)\succ0$ | 유한 MLE 존재 ⟺ $\bar t\in\nabla\psi(A)$ |
| 일치성 | 8강 Thm 8.1 | (C1) 강한 식별성 (C2) 균등 LLN (C3) 최대점 존재 | $0\le R(\hat\theta)\le\Delta_n$ |
| 극한분포 | 9강 Thm 9.1 | (A1)–(A5) | 점수 Taylor → LLN + CLT + Slutsky |
| 하한 | 9강 Thm 9.2 | (R1)–(R5), 미분·적분 교환 | $\mathrm{Cov}(S_n,U_n)=\nabla\mathbb EU_n$ → Cauchy–Schwarz |

---

## 2. 점수와 Fisher 정보

✍p3, p5

- 점수 $s_1(\theta;x)=\nabla_\theta\log f_\theta(x)$, 표본 점수 $S_n(\theta)=\sum_is_1(\theta;X_i)=\nabla_\theta\sum_i\log f_\theta(X_i)$.
- 정칙조건 아래 $\mathbb E_\theta s_1=0$, $I_1(\theta)=\mathbb E_\theta[s_1s_1^\top]=-\mathbb E_\theta[\nabla^2_\theta\log f_\theta(X)]$, $\mathrm{Var}_ \theta(S_n)=nI_1(\theta)$.

**✍ 정보 항등식 증명 (수업 판서 그대로)**
- (a) $\mathbb E_\theta s_1=\int\nabla_\theta f_\theta(x)dx=\nabla_\theta\int f_\theta(x)dx=\nabla_\theta1=0$. 여기서 $\nabla_\theta f_\theta=f_\theta s_1$ (로그 미분), 미분·적분 교환과 **공통 지지집합**이 필요하다.
- (b) 항등식 $0=\int f_\theta(x)s_1(\theta;x)dx$를 한 번 더 미분: $0=\int\nabla_\theta\lbrace f_\theta s_1^\top\rbrace dx=\int f_\theta s_1s_1^\top dx+\int f_\theta H(\theta;x)dx=\mathbb E[s_1s_1^\top]+\mathbb E[H]$ ( $H=\nabla^2_\theta\log f_\theta$).
- (c) 따라서 $I_1(\theta)=\mathrm{Var}_ \theta(s_1)=-\mathbb E_\theta H$, 독립이라 $\mathrm{Var}_ \theta(S_n)=nI_1(\theta)$. $\blacksquare$

---

## 3. 일치성

✍p3–4 · 📄 부록 Thm 3.1

**설정** $R_n(\theta)=\frac1n\sum_i\log\frac{f_{\theta_0}(X_i)}{f_\theta(X_i)}$ ( $R_n(\theta_0)=0$), $R(\theta)=\mathbb E_{\theta_0}R_n(\theta)=\mathrm{KL}(f_{\theta_0}\Vert f_\theta)\ge0$. MLE는 $R_n$의 최소점.

**가정** (공통 지지집합 전제)
1. 강한 식별성: $\forall\epsilon > 0$, $\inf_{\Vert\theta-\theta_0\Vert\ge\epsilon}R(\theta)\ge\eta_\epsilon > 0$.
2. 균등 LLN: $\Delta_n=\sup_{\theta\in\Theta}\lvert R_n(\theta)-R(\theta)\rvert\xrightarrow{p}0$.
3. 각 $n$에서 $\hat\theta_n\in\mathop{\mathrm{argmax}}\ell_n$를 고를 수 있다.

**결론** $\hat\theta_n\xrightarrow{p}\theta_0$.

**✍ 증명 (판서)**
- (a) MLE는 $R_n$의 최소점: $R_n(\hat\theta_n)\le R_n(\theta_0)=0$.
- (b) $0\le R(\hat\theta_n)\le R_n(\hat\theta_n)+\Delta_n\le\Delta_n\xrightarrow{p}0$.
- (c) $\Vert\hat\theta_n-\theta_0\Vert\ge\epsilon$이면 1로 $R(\hat\theta_n)\ge\eta_\epsilon$, 그러면 (b)로 $\Delta_n\ge\eta_\epsilon$. 따라서 $\Pr(\Vert\hat\theta_n-\theta_0\Vert\ge\epsilon)\le\Pr(\Delta_n\ge\eta_\epsilon)\to0$. $\blacksquare$

- 식별 가능성만으로는 1이 보장되지 않는다. $\Theta$ 콤팩트 + $R$ 연속 + $\theta_0$가 유일한 최소점이면 1이 따른다. 각 $\theta$에서의 LLN은 2 (균등)를 대신하지 못한다. 모수 차원이 $n$과 함께 커지면 (Neyman–Scott) 깨진다.

---

## 4. 점근정규성

✍p4 · 📄 9강 Thm 9.1, 부록 Thm 4.1

**정칙조건 (A1)–(A5)** (✍ 판서 번호 그대로)

| | 조건 | 증명에서 쓰는 곳 |
| --- | --- | --- |
| (A1) | 차원 $d$ 고정, $\theta_0$는 **내부점** | 점수 $S_n(\hat\theta)=0$ |
| (A2) | $\ell(\theta;x)$가 $\theta_0$ 근방에서 3차까지 매끄럽고 지배조건 (예: $\sup_{U}\Vert H(\theta;X)\Vert\le G(X)$, $\mathbb EG < \infty$) | Taylor, $J_n\to I$ |
| (A3) | 미분·적분 교환 → $\mathbb Es=0$, $I=\mathbb E[ss^\top]=-\mathbb EH$ | CLT, $J_n$의 극한 |
| (A4) | 식별, MLE 존재, $\hat\theta_n\xrightarrow{p}\theta_0$ | 내부점, 중간점 수렴 |
| (A5) | $I(\theta_0)$ 유한·양정치 | 역행렬 |

**결론** $\sqrt n(\hat\theta_n-\theta_0)\xrightarrow{d}N_d(0,I(\theta_0)^{-1})$.

**✍ 증명 (적분형 Taylor, 판서·부록 판)**
- (a) (A1)·(A4): 확률 → 1인 사건에서 $\hat\theta_n$은 내부 최대점 → $S_n(\hat\theta_n)=0$.
- (b) 적분형 Taylor: $0=S_n(\hat\theta_n)=S_n(\theta_0)-nJ_n(\hat\theta_n-\theta_0)$, 여기서

$$J_n=-\int_0^1\frac1n\sum_{i=1}^nH\big(\theta_0+u(\hat\theta_n-\theta_0);X_i\big)du .$$

  (벡터에서도 한 점 평균값 정리 대신 적분형을 쓰면 깔끔하다)
- (c) (A2)·(A3)·(A4) → $J_n\xrightarrow{p}I(\theta_0)$. (A3)·(A5) + CLT → $\frac1{\sqrt n}S_n(\theta_0)\xrightarrow{d}N_d(0,I(\theta_0))$.
- (d) (A5)로 $J_n$은 확률 → 1로 가역 → Slutsky:

$$\sqrt n(\hat\theta_n-\theta_0)=J_n^{-1}\frac{S_n(\theta_0)}{\sqrt n}\xrightarrow{d}I^{-1}N_d(0,I)=N_d(0,I^{-1}).\qquad\blacksquare$$

**✍ 반례: $U(0,\theta)$** — 공통 지지집합이 없어 정칙조건이 깨진다. $L(\theta)=\theta^{-n}I(\theta\ge X_{(n)})$ → $\hat\theta=X_{(n)}$, $\Pr\lbrace n(\theta-\hat\theta)\gt t\rbrace=(1-\frac t{n\theta})^n\to e^{-t/\theta}$ → $n(\theta-\hat\theta)\xrightarrow{d}\mathrm{Exp}(\text{평균 }\theta)$, $\sqrt n(\hat\theta-\theta)\xrightarrow{p}0$. (필기 메모: "MLE의 존재·일치성은 있지만 점근정규성은 없다")
- 11/12 수업: 모수가 **경계**에 있으면 내부점 조건이 깨져 극한이 정규가 아니게 된다 (예: 경계에서는 $\chi^2$가 섞인 극한).

---

## 5. 수업 예제 지수분포 율모수 풀세트

✍p6–7 (10/16) — **MLE 문제 하나를 처음부터 끝까지 푸는 모범 순서.** 시험에서 "MLE의 존재·유일성·일치성·극한분포"를 물으면 이 틀로 쓴다.

$X_i\overset{\text{iid}}{\sim}f_\theta(x)=\theta e^{-\theta x}I(x\ge0)$, $\theta > 0$, $\mathbb EX=\frac1\theta$.

1. **로그가능도·점수:** $\ell_n(\theta)=n\log\theta-\theta\sum X_i$, $S_n(\theta)=\frac n\theta-\sum X_i$, $\ell_n''(\theta)=-\frac n{\theta^2} < 0$.
2. **존재:** $\sum X_i > 0$ (확률 1)이면 $\theta\to0$이나 $\theta\to\infty$에서 $\ell_n\to-\infty$ → 최대점 존재. **유일:** 순오목 → 점수 방정식의 근 $\hat\theta=\frac n{\sum X_i}=\frac1{\bar X_n}$이 유일한 MLE.
3. **일치성:** LLN $\bar X_n\xrightarrow{p}\frac1{\theta_0}$, $x\mapsto1/x$가 $\frac1{\theta_0}$에서 연속 → CMT로 $\hat\theta_n\xrightarrow{p}\theta_0$.
4. **정칙조건 확인:** 공통 지지집합 $[0,\infty)$, $\mathrm{Var}_ {\theta_0}X=\frac1{\theta_0^2} < \infty$, 헤시안 $-\frac1nS_n'(\hat\theta)=\frac1{\hat\theta^2}\to\frac1{\theta_0^2}$.
5. **정보·극한분포:** $\mathbb E(\frac1\theta-X)=0$, $I_1(\theta)=\mathrm{Var}(X)=\frac1{\theta^2}$ → $\sqrt n(\hat\theta-\theta_0)\xrightarrow{d}N(0,\theta_0^2)$, 즉 $\hat\theta\ \dot\sim\ N(\theta_0,\frac{\theta_0^2}n)$.
6. **델타 방법으로 검산:** CLT $\sqrt n(\bar X-\frac1{\theta_0})\to N(0,\frac1{\theta_0^2})$, $h(x)=\frac1x$, $h'(\frac1{\theta_0})=-\theta_0^2$ → 분산 $\theta_0^4\cdot\frac1{\theta_0^2}=\theta_0^2$ ✓.
7. **정확한 평균·분산 (감마 역적률)** ✍p7: $T=\sum X_i\sim\mathrm{Gamma}(n,\text{rate }\theta)$, $\mathbb E_\theta T^{-k}=\theta^k\frac{\Gamma(n-k)}{\Gamma(n)}$ ( $n > k$). 따라서

$$\mathbb E\hat\theta=\frac n{n-1}\theta\ (n\gt 1),\qquad\mathrm{Var}\hat\theta=n^2\Big(\frac{\theta^2}{(n-1)(n-2)}-\frac{\theta^2}{(n-1)^2}\Big)=\frac{n^2\theta^2}{(n-1)^2(n-2)}\ (n\gt 2),$$

   $n\mathrm{Var}(\hat\theta)\to\theta^2$ (극한분포의 분산과 일치).
8. **CR 적용** ✍p7: $g(\theta)=\theta$, $I_1=\frac1{\theta^2}$ → 불편추정량의 분산 $\ge\frac{\theta^2}n$.
   - 불편 $\tilde\theta=\frac{n-1}T$: $\mathrm{Var}=\frac{\theta^2}{n-2} > \frac{\theta^2}n$ (비 $\frac n{n-2}\to1$) — 하한 미달성.
   - **편향된 MLE에 CR:** $m(\theta)=\mathbb E\hat\theta=\frac n{n-1}\theta$ → $\mathrm{Var}(\hat\theta)\ge\frac{m'(\theta)^2}{nI_1}=\frac{n\theta^2}{(n-1)^2}$. (실제 $\frac{n^2\theta^2}{(n-1)^2(n-2)}$이 더 큼 ✓) — **불편이 아니면 $g'$ 대신 $m'$**.

---

## 6. Cramér–Rao 정보부등식

✍p1, p5–6, p22 · 📄 9강 Thm 9.2, 13강 Thm 13.1

**진술** $\mathbb E_\theta U_n^2 < \infty$, $m(\theta)=\mathbb E_\theta U_n$이면

$$\mathrm{Var}_ \theta(U_n)\ge\nabla m(\theta)^\top\lbrace nI_1(\theta)\rbrace^{-1}\nabla m(\theta).$$

$U_n$이 $g(\theta)$의 불편추정량이면 $m=g$. 1차원: $\mathrm{Var}(T)\ge\frac{g'(\theta)^2}{I_n(\theta)}$, $I_n=nI_1$.

**✍ 증명 (판서)**
- (a) $f^{(n)}_ \theta(x)=\prod_if_\theta(x_i)$, $m(\theta)=\int U_n(x)f^{(n)}_ \theta(x)dx$. 미분·적분 교환 (지배함수) → $\nabla m=\int U_n\nabla f^{(n)}_ \theta=\int U_nf^{(n)}_ \theta S_n=\mathbb E[S_nU_n]$.
- (b) $\mathbb ES_n=0$이라 $\mathbb E[S_nU_n]=\mathbb E[S_nU_n]-\mathbb EU_n\mathbb ES_n=\mathrm{Cov}(S_n,U_n)$ → **$\mathrm{Cov}(S_n,U_n)=\nabla m(\theta)=:c$** ( $d\times1$).
- (c) $Y=U_n-\mathbb EU_n$, $V=\mathrm{Var}(S_n)=nI_1$. 스칼라 $a^\top S_n$과 Cauchy–Schwarz: $\lbrace\mathbb E[Y(a^\top S_n)]\rbrace^2\le\mathbb E[Y^2]\mathbb E[(a^\top S_n)^2]$, 즉 $(a^\top c)^2\le\mathrm{Var}(U_n)\thinspace a^\top Va$.
- (d) $a=V^{-1}c$: $a^\top c=a^\top Va=c^\top V^{-1}c$ → $(c^\top V^{-1}c)^2\le\mathrm{Var}(U_n)c^\top V^{-1}c$ → $\mathrm{Var}(U_n)\ge c^\top V^{-1}c$ ( $c=0$이면 자명). $\blacksquare$
- ✍ **다른 길:** $0\le\mathrm{Var}(U_n-\mathbb EU_n-c^\top V^{-1}S_n)=\mathrm{Var}(U_n)-c^\top V^{-1}c$. **등호 ⟺ $U_n-\mathbb EU_n=c^\top V^{-1}S_n$** (a.s.).

**✍ 1차원 등호 조건 (13강, ✍p22)** $\mathrm{Var}(T)\ge\frac{\mathrm{Cov}^2(T,U_n)}{\mathrm{Var}(U_n)}$ ⟸ $\mathrm{Corr}^2(T,U_n)\le1$, 그리고 $g'(\theta)=\mathrm{Cov}(T,U_n)=\mathbb E[TU_n]$. 등호 ⟺ $\mathrm{Corr}=\pm1$ ⟺

$$\Pr_\theta\Big(T-g(\theta)=\frac{g'(\theta)}{I_n(\theta)}U_n(\theta)\Big)=1,$$

이때 $\mathrm{Var}(T)=\frac{g'^2}{I_n^2}I_n=\frac{g'^2}{I_n}$. 즉 **점수가 추정 오차에 정확히 비례**할 때만 달성 → 지수족에서 평균모수의 1차식 ([ch5.6 5.4](../ch5.6%20MLE의%20점근이론%20II.md#54-등호-조건)).

**주의 (13강 p1)** 점근분포의 분산은 유한 표본 분산과 일반적으로 다르다. MLE가 불편이라는 사실만으로 하한을 달성하지는 않는다. 다모수 성분의 하한은 $[I_1^{-1}]_ {kk}/n$.

---

## 7. 하한 예제

✍p1–2, p22

| 모형 | $I_1$ | MLE | 하한 | 달성? |
| --- | --- | --- | --- | --- |
| 포아송 $\lambda$ ✍ | $\frac1\lambda$ ( $\ell_1''=-x/\lambda^2$) | $\bar X$ | $\frac\lambda n$ | ✅ = UMVUE, $\sqrt n(\bar X-\lambda)\to N(0,\lambda)$ |
| 베르누이 $p$ ✍ | $\frac1{p(1-p)}$ | $\bar X$ | $\frac{p(1-p)}n$ | ✅ |
| 베르누이 $\eta=p(1-p)$ ✍ | – | $\bar X(1-\bar X)$ | $\frac{(1-2p)^2p(1-p)}n$ | ❌ (MLE 편향 $\frac{n-1}np(1-p){}$) |
| 정규 $(\mu,\sigma^2)$ ✍ | $\mathrm{diag}(\frac1{\sigma^2},\frac1{2\sigma^4})$ | $\bar X$, $\frac1n\sum(X_i-\bar X)^2$ | $\frac{\sigma^2}n$, $\frac{2\sigma^4}n$ | $\mu$ ✅, $S^2$은 $\frac{2\sigma^4}{n-1}$ ❌ |
| Beta $(\theta,1)$ ✍ | $\frac1{\theta^2}$ | $-\frac n{\sum\log X_i}$ | $\frac{\theta^2}n$ | ❌ (불편판 $\frac{\theta^2}{n-2}$) |
| 지수 율 $\theta$ ✍ | $\frac1{\theta^2}$ | $\frac1{\bar X}$ | $\frac{\theta^2}n$ | ❌ (5절) |

**✍ $\eta=p(1-p)$의 MLE 편향과 분산 (판서 방법)**
- $\mathbb E\hat\eta=\mathbb E\bar X-\mathbb E\bar X^2=p-\lbrace\frac{p(1-p)}n+p^2\rbrace=\frac{n-1}np(1-p)$ → 편향. 불편판 $\tilde\eta=\frac n{n-1}\hat\eta$, $\mathrm{Var}\tilde\eta=(\frac n{n-1})^2\mathrm{Var}\hat\eta$.
- 분산: $Y=\bar X-p$로 두면 $\hat\eta=(p+Y)(1-p-Y)=p(1-p)+(1-2p)Y-Y^2$이므로

$$\mathrm{Var}(\hat\eta)=(1-2p)^2\mathrm{Var}(Y)+\lbrace\mathbb EY^4-(\mathbb EY^2)^2\rbrace-2(1-2p)\mathbb EY^3,$$

  주항은 $(1-2p)^2\frac{p(1-p)}n$ = 하한 (나머지는 $O(n^{-2}){}$). 필기 메모: **"MLE가 unbiased는 아니지만 asymptotic하게는 Fisher info로 수렴"**. $p=\frac12$에서는 1차항이 사라진다 ( $n(\hat\eta-\frac14)\to-\frac14\chi^2_1$).

**✍ 정규 정보행렬 (판서)** $\ell_1=-\frac12\log\sigma^2-\frac{(x-\mu)^2}{2\sigma^2}+C$, $\partial_\mu\ell_1=\frac{x-\mu}{\sigma^2}$, $\partial^2_\mu\ell_1=-\frac1{\sigma^2}$, $\partial_{\sigma^2}\ell_1=-\frac1{2\sigma^2}+\frac{(x-\mu)^2}{2\sigma^4}$, $\partial^2_{\sigma^2}\ell_1=\frac1{2\sigma^4}-\frac{(x-\mu)^2}{\sigma^6}$ → $-\mathbb E=\frac1{2\sigma^4}$. MLE $\hat\sigma^2$은 $\sqrt n(\hat\sigma^2-\sigma^2)\to N(0,2\sigma^4)$, $S^2$은 $\frac{(n-1)S^2}{\sigma^2}\sim\chi^2_{n-1}$이라 $\mathrm{Var}S^2=\frac{2\sigma^4}{n-1}$.

**✍ Beta $(\theta,1)$ (판서, ✍p22)** $f=\theta x^{\theta-1}$, $\sum\log f=n\log\theta+(\theta-1)\sum\log X_i$ → $\hat\theta=-\frac n{\sum\log X_i}$. $Y_i=-\log X_i$: $\Pr(Y\le y)=\Pr(X\ge e^{-y})=1-e^{-\theta y}$ → $Y_i\sim\mathrm{Exp}(\text{rate }\theta)$, $Z=\sum Y_i\sim\mathrm{Gamma}(n,\text{rate }\theta)$ → $\mathbb E\frac nZ=\frac n{n-1}\theta$. 불편 $\frac{n-1}n\hat\theta=-\frac{n-1}{\sum\log X_i}$, 분산 $\frac{\theta^2}{n-2} > \frac{\theta^2}n$. $n\to\infty$: 편향 → 0, MSE → 0.

---

## 8. 자주 하는 실수

- $\sqrt n(\hat\theta-\theta)\to N(0,I_1^{-1})$와 $\hat\theta\ \dot\sim\ N(\theta,I_1^{-1}/n)$를 섞어 쓰기 ( $I_1$ vs $I_n=nI_1$).
- 점근정규성 증명에서 "확률 → 1인 사건에서 $S_n(\hat\theta)=0$"을 빼먹기.
- 지지집합이 모수에 의존하는 모형 ( $U(0,\theta)$, 이동 지수)에 정보 공식·CR을 그대로 쓰기.
- 편향된 추정량에 $g'$을 쓰기 ( $m'(\theta)$를 써야 함).
- CR 하한 미달성 = UMVUE 아님이라고 결론 내리기 (Beta, 정규 $S^2$은 UMVUE지만 미달성).

**셀프 체크**
<details><summary>Q1. 일치성 증명에서 KL이 쓰이는 곳은?</summary>

$R(\theta)=\mathrm{KL}(f_{\theta_0}\Vert f_\theta)\ge0$이라 (b)의 왼쪽 0이 나오고, 강한 식별성이 "θ₀에서 ε 떨어지면 KL이 η 이상"이라는 분리 조건이 된다.

</details>
<details><summary>Q2. CR 증명에서 a를 V⁻¹c로 고르는 이유는?</summary>

$(a^\top c)^2/(a^\top Va)$를 최대로 만드는 방향이라서 가장 좋은 하한이 나온다. $Y$를 $S_n$으로 회귀한 계수와 같다.

</details>
<details><summary>Q3. 지수분포 율모수에서 불편추정량 (n−1)/T가 하한을 못 달성하는 이유를 등호 조건으로?</summary>

등호이려면 $T'-\theta$가 $S_n=\frac n\theta-T$의 상수배여야 하는데 $\frac{n-1}T-\theta$는 $T$의 1차식이 아니다. 율모수 $\theta$는 지수족 $e^{-\theta x}$의 평균모수 ( $\mathbb EX=1/\theta$)의 1차식이 아니기 때문이다.

</details>
