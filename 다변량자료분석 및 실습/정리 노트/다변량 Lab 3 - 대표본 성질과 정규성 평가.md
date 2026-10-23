# 다변량자료분석 및 실습 — Lab 3: 대표본 성질과 정규성 평가 (4.5·4.6)

> **자료:** `Lab_Session_3.pdf` (조교 송의종, 10/21 실습, 16쪽) · `Lab_Session_3_preliminaries_ver2.pdf` (증명에 쓰는 정의·보조정리, 16쪽) · **10/21 필기 3쪽**. 출처는 슬라이드 **(L p.쪽)**, preliminaries **(Pre 번호)** 로 표시한다.
> 슬라이드에서 Result 4.11\~4.13의 [Proof] 칸은 비어 있다. **증명은 10/21 필기(📝)를 뼈대로 하고, 필기에서 건너뛴 단계는 preliminaries의 해당 번호로 채웠다.**
> 강의 PDF 4장과의 관계: [2번 파일 4.7 MLE와 대표본 성질](다변량%201-7장%20학습노트%202%20-%203-4장.md#47-mle와-대표본-성질-p151-173177)의 Result 4.11·4.12를 엄밀하게 증명하고, Discussion으로만 있던 4.6 정규성 평가를 채운다. 기호·✏️ 표시는 [1번 파일](다변량%201-7장%20학습노트%201%20-%20요약·1-2장.md) 머리말과 같다.

---

## 0. 한눈에 보기

| 결과 | 가정 | 결론 | 증명의 핵심 도구 |
| --- | --- | --- | --- |
| **4.11** MLE | i.i.d. $N_p(\mu,\Sigma)$, $S_0\succ0$ | $\hat\mu=\bar x$, $\hat\Sigma=S_0=\frac{n-1}nS$ | 프로파일링 → $\Omega=\Sigma^{-1}$로 변수변환 → 볼록성 + 1계 조건 |
| **4.12** 대수의 법칙 | i.i.d., 평균 $\mu$, **유한한** $\Sigma$ (정규 불필요) | $\bar X_n\overset p\to\mu$, $S\overset p\to\Sigma$, $S_0\overset p\to\Sigma$ | 마코프 부등식, WLLN, CMT |
| **4.13** 다변량 CLT | i.i.d., $\Sigma\succ0$ (정규 불필요) | $\sqrt n(\bar X_n-\mu)\Rightarrow N_p(0,\Sigma)$, $T_n\Rightarrow\chi^2_p$ | Cramér–Wold, $r(\cdot)$의 연속성, 슬러츠키, CMT |
| **4.6** 정규성 평가 | — | 주변 정규? 쌍별 타원? 전체 적합·이상치? | 비율, Q–Q plot, $r_Q$, 50% 타원, 카이제곱 plot |

**시험 포인트 (중요도 순)**
1. **4.6의 판정 계산**: 비율 검사의 경계 $1.396/\sqrt n$, $0.628/\sqrt n$, $r_Q$를 표와 비교, 50% 타원 안의 개수, 카이제곱 plot 해석. 숫자만 바꿔 내기 쉽다. → [5절](#5-46-정규성-가정의-평가-l-p1016)
2. **4.13 (ii)의 의미**: 정규가 아니어도 $n(\bar X-\mu)^\top S^{-1}(\bar X-\mu)\approx\chi^2_p$ → 5장 대표본 검정의 근거. 증명 흐름(Cramér–Wold → $r(S)\overset p\to\Sigma^{-1}$ → 슬러츠키 → CMT).
3. **4.11의 증명 흐름**: 왜 $\Sigma$가 아니라 $\Omega=\Sigma^{-1}$로 바꾸는가(볼록성), 2계·1계 조건.
4. 4.12 (b)의 분해식 $(n-1)S=\sum(X_j-\mu)(X_j-\mu)^\top-n(\bar X-\mu)(\bar X-\mu)^\top$.

---

## 1. 표기와 증명 도구 (L p.3\~6, Pre)

**표기** (L p.3): $X_1,\dots,X_n\in\mathbb R^p$ i.i.d., $E(X_j)=\mu$, $\mathop{\mathrm{Cov}}\nolimits(X_j)=\Sigma$ 유한. **따로 말하지 않으면 정규성은 가정하지 않는다.**

$$\bar X_n=\frac1n\sum_{j=1}^nX_j,\qquad S=\frac1{n-1}\sum_{j=1}^n(X_j-\bar X_n)(X_j-\bar X_n)^\top,\qquad S_0=\frac{n-1}nS$$

$\Rightarrow$ = 분포수렴, $\overset p\to$ = 확률수렴. 소문자 $x_j$, $\bar x$는 관측값.

증명에서 실제로 쓰는 도구만 모았다. 번호는 preliminaries의 번호다.

| 분류 | 도구 | 내용 |
| --- | --- | --- |
| 노름 | Frobenius 노름 (Pre 1.2, 1.3) | $\lVert A\rVert_F=(\sum_{i,j}a_{ij}^2)^{1/2}=\lbrace\mathop{\mathrm{tr}}\nolimits(A^\top A)\rbrace^{1/2}$ = 원소를 벡터로 늘어놓은 길이 |
| 연속성 | 원소별 연속 (Pre 1.6) | 행렬값 함수의 **모든 원소가 연속이면** 함수가 연속. 원소들의 다항식(분모 $\ne0$인 분수 포함)은 연속 |
| | 행렬식·역행렬 (Pre 1.1 (v)(vi)) | $\lvert A\rvert$는 원소의 다항식(라이프니츠 공식) → 연속, $\lbrace A:\lvert A\rvert\ne0\rbrace$는 **열린집합**. $A^{-1}=\mathop{\mathrm{adj}}\nolimits(A)/\lvert A\rvert$ → 그 위에서 연속 |
| 고유값 | 최소 고유값 (Pre 2.5) | $\lambda_p(A)=\min_{\lVert x\rVert=1}x^\top Ax$, $\lvert\lambda_p(A)-\lambda_p(B)\rvert\le\lVert A-B\rVert_F$ → **PD 행렬 집합은 볼록이고 열린집합** |
| | 제곱근 (Pre 2.6, 2.7) | $A^{\pm1/2}=U\Lambda^{\pm1/2}U^\top$, $\lambda_i(A^{-1})=1/\lambda_{p+1-i}(A)$ |
| 부등식 | Von Neumann (Pre 3.2) | $\sum_i\lambda_{p+1-i}(A)\lambda_i(B)\le\mathop{\mathrm{tr}}\nolimits(AB)\le\sum_i\lambda_i(A)\lambda_i(B)$ (대칭 $A,B$) |
| | Pre 3.3 | $x-\log x\ge1$ ($x>0$), 등호 $\iff x=1$ |
| 행렬미분 | 방향미분 (Pre 5.5) | $\Omega\succ0$, $H$ 대칭: $D\log\lvert\Omega\rvert[H]=\mathop{\mathrm{tr}}\nolimits(\Omega^{-1}H)$, $D^2\log\lvert\Omega\rvert[H]=-\mathop{\mathrm{tr}}\nolimits(\Omega^{-1}H\Omega^{-1}H)$, $D^2\mathop{\mathrm{tr}}\nolimits(\Omega B)[H]=0$ |
| | 행렬 기울기 (Pre 5.6, 5.7) | $\partial\log\lvert\Omega\rvert/\partial\Omega=\Omega^{-1}$, $\partial\mathop{\mathrm{tr}}\nolimits(\Omega B)/\partial\Omega=B^\top$. 기울기 $G$가 대칭이면 "모든 대칭 $H$에 $Df[H]=0$" $\iff G=0$ |
| 최적화 | 볼록성 (Pre 6.1) | 열린 볼록집합에서 모든 $H\ne0$에 $D^2f[H]>0$이면 **임계점 = 유일한 최소점** |
| | argmin 규칙 (Pre 6.2) | 양수배·상수 덧셈·부호 반전($\max\leftrightarrow\min$)·로그·**전단사 변수변환**은 최적점을 바꾸지 않음 |
| | 프로파일링 (Pre 6.3) | 각 $\eta$마다 $\hat\theta(\eta)$로 최소화하고, 그 결과 $\ell_p(\eta)$를 최소화하면 결합 최소점 |
| 확률 | S1, Pre 8.3 | 마코프: $P(\lVert Y\rVert>\epsilon)\le E\lVert Y\rVert^2/\epsilon^2$ |
| | S2 | $E\lVert Y-EY\rVert^2=\mathop{\mathrm{tr}}\nolimits\mathop{\mathrm{Cov}}\nolimits(Y)$, 독립합의 공분산 = 공분산의 합 |
| | S3, S4 | 일변량 WLLN ( $E\lvert Y\rvert<\infty$), 일변량 CLT |
| | S5 CMT | $Y_n\overset p\to c$이고 $g$가 **점 $c$에서만** 연속이면 $g(Y_n)\overset p\to g(c)$. 분포수렴도 같음 |
| | S6 슬러츠키, S7 | $Y_n\Rightarrow Y$, $W_n\overset p\to c$ ⇒ $(Y_n,W_n)\Rightarrow(Y,c)$. 상수로의 확률수렴은 묶어도 성립 |
| | Pre 8.5 | 랜덤행렬의 확률수렴 = **원소별** 확률수렴 $\iff\lVert A_n-A\rVert_F\overset p\to0$ → 벡터용 CMT·슬러츠키를 그대로 씀 |
| | Pre 8.7 Cramér–Wold | $Y_n\Rightarrow Y\iff$ 모든 $t$에서 $t^\top Y_n\Rightarrow t^\top Y$ |

---

## 2. Result 4.11 — MLE (L p.7)

$X_1,\dots,X_n$ i.i.d. $N_p(\mu,\Sigma)$, $\Sigma\succ0$, 그리고 $S_0\succ0$ (사실상 $n>p$)이면

$$\hat\mu_{\text{MLE}}=\bar x,\qquad \hat\Sigma_{\text{MLE}}=S_0=\frac1n\sum_{j=1}^n(x_j-\bar x)(x_j-\bar x)^\top$$

### 증명 1 — 행렬미분 (10/21 필기)

**① 로그가능도.** 밀도 $f(x)=\lvert2\pi\Sigma\rvert^{-1/2}\exp\lbrace-\frac12(x-\mu)^\top\Sigma^{-1}(x-\mu)\rbrace$에서

$$\ell(\mu,\Sigma)=\log L(\mu,\Sigma)=-\frac n2\log\lvert\Sigma\rvert-\frac12\sum_{j=1}^n(x_j-\mu)^\top\Sigma^{-1}(x_j-\mu)-c$$

목표: $\arg\max_{\mu,\Sigma}\ell(\mu,\Sigma)$. **프로파일링(Pre 6.3)** — $\Sigma$를 고정하고 $\mu$부터 푼다.

**② $\Sigma\succ0$ 고정, $\mu$ 최적화.** $\hat\mu(\Sigma)=\arg\min_\mu g(\mu)$, $g(\mu)=\sum_j(x_j-\mu)^\top\Sigma^{-1}(x_j-\mu)$ (Pre 6.2로 부호·상수 정리).

$$\frac{\partial g}{\partial\mu}=\sum_j\bigl(-2\Sigma^{-1}(x_j-\mu)\bigr)=-2n\Sigma^{-1}(\bar x-\mu),\qquad \frac{\partial^2g}{\partial\mu\thinspace\partial\mu^\top}=2n\Sigma^{-1}\succ0$$

헤시안이 PD → $g$는 강볼록(Pre 6.1 (iii)), $\partial g/\partial\mu=0\iff\mu=\bar x$. **∴ 어떤 $\Sigma$에서도 $\hat\mu(\Sigma)=\bar x$.** (이차형식의 미분은 Pre 4.5: $\partial(x^\top Ax)/\partial x=2Ax$.)

**③ 프로파일 로그가능도.** 트레이스 트릭 $a^\top Ba=\mathop{\mathrm{tr}}\nolimits(Baa^\top)$으로

$$\ell_p(\Sigma)=\ell(\bar x,\Sigma)=-\frac n2\log\lvert\Sigma\rvert-\frac12\mathop{\mathrm{tr}}\nolimits\Bigl(\Sigma^{-1}\sum_j(x_j-\bar x)(x_j-\bar x)^\top\Bigr)-c=-\frac n2\thinspace g(\Sigma)-c,\qquad g(\Sigma)=\log\lvert\Sigma\rvert+\mathop{\mathrm{tr}}\nolimits(\Sigma^{-1}S_0)$$

→ $\hat\Sigma=\arg\max\ell_p=\arg\min g(\Sigma)$.

**④ 변수변환 $\Omega=\Sigma^{-1}$ (정밀도 행렬).** $\Sigma\mapsto\Sigma^{-1}$은 PD 행렬 집합 위의 전단사(Pre 2.7 (iv))이므로 Pre 6.2 (iv)에 의해

$$\arg\min_{\Sigma\succ0}g(\Sigma)=\lbrace\Omega^{-1}:\ \Omega\in\arg\min_{\Omega\succ0}h(\Omega)\rbrace,\qquad h(\Omega)=g(\Omega^{-1})=-\log\lvert\Omega\rvert+\mathop{\mathrm{tr}}\nolimits(\Omega S_0)$$

> **왜 바꾸나?** $g$는 $\Sigma$에 대해 볼록이 아니지만, $h$는 $\Omega$에 대해 **볼록**이다 ( $-\log\lvert\Omega\rvert$는 볼록, $\mathop{\mathrm{tr}}\nolimits(\Omega S_0)$는 선형). 볼록이면 "기울기 0인 점 = 유일한 최소점"이 바로 나온다.

**⑤ (a) 정의역** — $\lbrace\Omega\succ0\rbrace$는 볼록이고 열린집합 (Pre 2.5 (iii)).

**⑥ (b) 2계 조건** — 대칭인 $H\ne0$에 대해 (Pre 5.5)

$$D^2h(\Omega)[H]=-D^2\log\lvert\Omega\rvert[H]+D^2\mathop{\mathrm{tr}}\nolimits(\Omega S_0)[H]=\mathop{\mathrm{tr}}\nolimits(\Omega^{-1}H\Omega^{-1}H)+0=\lVert M\rVert_F^2\gt 0,\qquad M=\Omega^{-1/2}H\Omega^{-1/2}$$

( $M$은 대칭이라 $\mathop{\mathrm{tr}}\nolimits(\Omega^{-1}H\Omega^{-1}H)=\mathop{\mathrm{tr}}\nolimits(MM)=\mathop{\mathrm{tr}}\nolimits(M^\top M)$. $H\ne0$이면 $M\ne0$.) → $h$는 강볼록 (Pre 6.1 (i)).

**⑦ (c) 1계 조건** — Pre 5.6으로

$$\frac{\partial h}{\partial\Omega}=-\Omega^{-1}+S_0^\top=S_0-\Omega^{-1}=0\iff\Omega=S_0^{-1}$$

$S_0\succ0$이므로 $S_0^{-1}\succ0$ (정의역 안). 기울기가 대칭이라 Pre 5.7에 의해 "모든 대칭 $H$에 $Dh(S_0^{-1})[H]=0$". Pre 6.1 (ii)로 **$\arg\min h=S_0^{-1}$ (유일)** → $\hat\Sigma=(S_0^{-1})^{-1}=S_0$. ②와 합치면 프로파일링에 의해 $(\hat\mu,\hat\Sigma)=(\bar x,S_0)$가 결합 최대점. ∎

> 📝 10/19 필기: "MLE를 구할 때 matrix derivative가 꼭 필요한 방법은 아니다. Frobenius norm을 활용해서 해 보자." → 위 ⑥의 $\lVert M\rVert_F^2$, 그리고 아래 증명 2(미분 없이 고유값 부등식만 사용)가 그 얘기다.

### 증명 2 — 미분 없이, Von Neumann 부등식 (Pre 3.2·3.3, 강의 Note p.174\~175)

$\Sigma=U\Lambda U^\top$ ($\lambda_1\ge\cdots\ge\lambda_p\gt 0$), $S_0=VDV^\top$ ($d_1\ge\cdots\ge d_p\gt 0$). $\Sigma^{-1}$의 고유값을 큰 순서로 쓰면 $1/\lambda_p\ge\cdots\ge1/\lambda_1$, 즉 $\lambda_{p+1-i}(\Sigma^{-1})=1/\lambda_i$. Von Neumann의 **아래쪽** 경계로

$$g(\Sigma)=\log\lvert\Sigma\rvert+\mathop{\mathrm{tr}}\nolimits(\Sigma^{-1}S_0)\ge\sum_{i=1}^p\Bigl(\log\lambda_i+\frac{d_i}{\lambda_i}\Bigr)=\log\lvert S_0\rvert+\sum_{i=1}^p(a_i-\log a_i)\ge\log\lvert S_0\rvert+p,\qquad a_i=\frac{d_i}{\lambda_i}$$

(가운데 등호: $\log\lambda_i=\log d_i-\log a_i$. 마지막: Pre 3.3.) 등호는 ① 고유벡터가 같고( $U=V$) ② 모든 $a_i=1$( $\Lambda=D$)일 때 → $\hat\Sigma=VDV^\top=S_0$. ∎

> ✏️ Note p.174의 밀도 정규화 상수 $(2\pi)^{n/2}$는 $(2\pi)^{p/2}$가 맞다 ([1번 파일 0.5절](다변량%201-7장%20학습노트%201%20-%20요약·1-2장.md#개정판-note-쪽에서-찾은-오류)).

---

## 3. Result 4.12 — 대수의 법칙 (L p.8)

i.i.d., $E(X_j)=\mu$, **유한한** $\mathop{\mathrm{Cov}}\nolimits(X_j)=\Sigma$ (정규 불필요)이면 $n\to\infty$에서

$$\text{(a) }\bar X_n\overset p\to\mu,\qquad\text{(b) }S\overset p\to\Sigma,\qquad\text{(c) }S_0\overset p\to\Sigma$$

**(a)** 벡터 마코프 부등식(Pre 8.3)과 S2로

$$P(\lVert\bar X_n-\mu\rVert\gt\epsilon)\le\frac{E\lVert\bar X_n-\mu\rVert^2}{\epsilon^2}=\frac{E\thinspace\mathop{\mathrm{tr}}\nolimits\bigl((\bar X_n-\mu)(\bar X_n-\mu)^\top\bigr)}{\epsilon^2}=\frac{\mathop{\mathrm{tr}}\nolimits\mathop{\mathrm{Cov}}\nolimits(\bar X_n)}{\epsilon^2}=\frac{\mathop{\mathrm{tr}}\nolimits\Sigma}{n\epsilon^2}\to0$$

**(b)** 괄호 안에 $-\mu+\mu$를 넣어 전개한다 (📝).

$$(n-1)S=\sum_j(X_j-\mu)(X_j-\mu)^\top-\sum_j(X_j-\mu)(\bar X_n-\mu)^\top-\sum_j(\bar X_n-\mu)(X_j-\mu)^\top+\sum_j(\bar X_n-\mu)(\bar X_n-\mu)^\top$$

가운데 두 합은 각각 $n(\bar X_n-\mu)(\bar X_n-\mu)^\top$이므로 $Y_j=(X_j-\mu)(X_j-\mu)^\top$라 두면

$$S=\frac n{n-1}\Bigl\lbrace\frac1n\sum_{j=1}^nY_j-(\bar X_n-\mu)(\bar X_n-\mu)^\top\Bigr\rbrace$$

- $Y_j$는 i.i.d. 랜덤행렬, $E(Y_j)=\Sigma$. 원소마다 $E\lvert(X_{jk}-\mu_k)(X_{jl}-\mu_l)\rvert\le\sqrt{\sigma_{kk}\sigma_{ll}}\lt\infty$ (코시–슈바르츠, Pre 8.2)라서 WLLN(S3)을 원소별로 쓸 수 있다 → $\frac1n\sum Y_j\overset p\to\Sigma$ (Pre 8.5).
- (a)와 CMT로 $(\bar X_n-\mu)(\bar X_n-\mu)^\top\overset p\to0$, 그리고 $\frac n{n-1}\to1$.
- S7로 묶고 CMT → **$S\overset p\to\Sigma$.**

**(c)** $S_0=\frac{n-1}nS$, $\frac{n-1}n\to1$ → $S_0\overset p\to\Sigma$. ∎

> **의미:** $S$(불편)와 $S_0$(MLE, 편향)는 둘 다 **일치추정량**. 📝 10/14: " $\hat\Sigma=\frac{n-1}nS$인 MLE는 asymptotically unbiased" — 편향 $-\frac1n\Sigma$가 $n\to\infty$에서 사라진다.

---

## 4. Result 4.13 — 다변량 CLT와 T 통계량의 카이제곱 근사 (L p.9)

i.i.d., $E(X_j)=\mu$, $\mathop{\mathrm{Cov}}\nolimits(X_j)=\Sigma\succ0$ (정규 불필요)이면 $n\to\infty$에서

1. $\sqrt n(\bar X_n-\mu)\Rightarrow N_p(0,\Sigma)$
2. $P(S\text{가 가역})\to1$ 이고 $T_n:=n(\bar X_n-\mu)^\top r(S)(\bar X_n-\mu)\Rightarrow\chi^2_p$

여기서 $r(A)=A^{-1}$ ( $\lvert A\rvert\ne0$), $r(A)=I_p$ (그 밖). 즉 $S$가 가역이면 $T_n=n(\bar X_n-\mu)^\top S^{-1}(\bar X_n-\mu)$.

> **왜 $r(\cdot)$이 필요한가?** 유한한 $n$에서는 $S$가 특이할 확률이 0이 아닐 수 있다(예: 이산형 데이터). 📝 "왜냐하면 $x_1,\dots,x_n$이 랜덤이니까" — $S$가 랜덤이라 "항상 가역"이 아니라 "가역일 확률 → 1"로 말한다. $T_n$이 언제나 정의되도록 특이할 때는 $I_p$를 넣어 둔 것이다.

**(i)의 증명 — Cramér–Wold** (📝). $Z_n=\sqrt n(\bar X_n-\mu)$, $Z\sim N_p(0,\Sigma)$일 때 모든 $t$에서 $t^\top Z_n\Rightarrow t^\top Z$를 보이면 된다(Pre 8.7). $t$를 고정하면 $t^\top X_j$는 i.i.d., 평균 $t^\top\mu$, 분산 $t^\top\Sigma t$인 일변량 확률변수 → 일변량 CLT(S4)로

$$t^\top Z_n=\sqrt n(t^\top\bar X_n-t^\top\mu)\Rightarrow N(0,\ t^\top\Sigma t)\overset d=t^\top Z\qquad(\text{Pre 7.2})$$

$t$가 임의이므로 $Z_n\Rightarrow Z$. ∎ ([2번 파일 4.4](다변량%201-7장%20학습노트%202%20-%203-4장.md#44-다변량-중심극한정리와-델타-방법-p144145)의 Homework 풀이와 같다.)

**(ii)의 증명** (📝)

1. **$S$가 가역일 확률.** $A_n=\lbrace S\text{가 가역}\rbrace$. $r$의 정의로
   $T_n=n(\bar X_n-\mu)^\top S^{-1}(\bar X_n-\mu)\thinspace\mathbb 1_{A_n}+n(\bar X_n-\mu)^\top(\bar X_n-\mu)\thinspace\mathbb 1_{A_n^c}$.
   행렬식은 연속이고 4.12로 $S\overset p\to\Sigma$이므로 CMT로 $\lvert S\rvert\overset p\to\lvert\Sigma\rvert\gt 0$. 따라서 $P(A_n)\ge P(\lvert S\rvert\gt\lvert\Sigma\rvert/2)\to1$.
2. **$r(S)\overset p\to\Sigma^{-1}$.** CMT(S5)는 $r$이 **극한점 $\Sigma$에서만** 연속이면 된다. $\lvert\Sigma\rvert\gt 0$이고 $\lbrace A:\lvert A\rvert\ne0\rbrace$는 열린집합이라 $\Sigma$ 근처에서 $r(A)=A^{-1}=\mathop{\mathrm{adj}}\nolimits(A)/\lvert A\rvert$. 각 원소가 "다항식 / 0이 아닌 다항식"이므로 원소별로 연속이고(📝 "element-wise하게 연속인 걸 보이고, 다항식이라면 좋음"), Pre 1.6에 의해 $r$은 $\Sigma$에서 연속. ( $r$은 $\lvert A\rvert=0$인 곳에서는 불연속이지만 상관없다.)
3. **슬러츠키 + CMT.** $Q(z,A)=z^\top Az=\sum_{i,j}z_iz_jA_{ij}$는 원소의 다항식이라 연속. $Z_n\Rightarrow Z$, $r(S)\overset p\to\Sigma^{-1}$ → 슬러츠키(S6)로 $(Z_n,r(S))\Rightarrow(Z,\Sigma^{-1})$ → CMT로

$$T_n=Q(Z_n,r(S))\Rightarrow Q(Z,\Sigma^{-1})=Z^\top\Sigma^{-1}Z\sim\chi^2_p\qquad(\text{Result 4.7})$$

∎

> ✏️ 필기 수정: 마지막 줄이 $Z^\top S^{-1}Z$로 적혀 있는데, 극한에서는 랜덤인 $S$가 아니라 **$\Sigma^{-1}$** 이다. 또 필기는 CMT만 적었지만, $Z_n$(분포수렴)과 $r(S)$(확률수렴)를 **함께** 넣으려면 슬러츠키(S6)로 결합수렴을 먼저 만들어야 한다.

> **의미 — 5장과의 연결:** 모집단이 정규가 아니어도 $n$이 크면 $n(\bar X-\mu_0)^\top S^{-1}(\bar X-\mu_0)\approx\chi^2_p$ → **대표본 검정·신뢰영역**([3번 파일 5.5](다변량%201-7장%20학습노트%203%20-%205-6장.md#55-대표본-추론-p210214))의 근거. 정규라면 정확히 $T^2\sim\frac{(n-1)p}{n-p}F_{p,n-p}$이고, $n\to\infty$에서 이것도 $\chi^2_p$로 간다(일관됨).

---

## 5. 4.6 정규성 가정의 평가 (L p.10\~16)

$X\sim N_p(\mu,\Sigma)$이면 (L p.10):
- 모든 $a^\top X$가 정규(Result 4.2) → **각 주변분포가 정규**
- 등밀도선이 타원체 → **각 변수 쌍의 산점도가 타원 모양**
- $(X-\mu)^\top\Sigma^{-1}(X-\mu)\sim\chi^2_p$ (Result 4.7) → **아주 큰 거리는 드물다**

| 확인할 것 | 방법 |
| --- | --- |
| 1. 주변분포가 정규인가 | 비율 검사, 정규 Q–Q plot, $r_Q$ 검정 |
| 2. 쌍별로 타원인가 | 50% 등고선(타원) 안의 비율 |
| 3. 전체 적합, 이상치 | 카이제곱 plot |

> **이것들은 필요조건일 뿐이다.** 하나라도 위반되면 정규가 아니라고 말할 수 있지만, 모두 통과해도 정규라는 증명은 아니다. (주변이 모두 정규여도 결합정규가 아닐 수 있다 — [Example 4.8](다변량%201-7장%20학습노트%202%20-%203-4장.md#example-48-p128--주변이-정규여도-결합정규가-아닐-수-있다))

### 5.1 주변분포 — 비율 검사 (L p.11)

정규라면 $p_{i1}=P(\lvert X_i-\mu_i\rvert\lt\sqrt{\sigma_{ii}})=0.683$, $p_{i2}=P(\lvert X_i-\mu_i\rvert\lt2\sqrt{\sigma_{ii}})=0.954$.
표본에서는 $\mu_i,\sigma_{ii}$ 대신 $\bar x_i, s_{ii}$를 넣고 개수를 센다: $\hat p_{i1}$ = ( $\lvert x_{ji}-\bar x_i\rvert\lt\sqrt{s_{ii}}$인 관측 수) ÷ $n$, $\hat p_{i2}$ = ( $\lvert x_{ji}-\bar x_i\rvert\lt2\sqrt{s_{ii}}$인 관측 수) ÷ $n$.

**판정:** 표준오차 $\sqrt{p_{ik}(1-p_{ik})/n}$의 **3배**보다 멀면 비정규 의심.

$$\lvert\hat p_{i1}-0.683\rvert\gt3\sqrt{\frac{0.683\times0.317}n}=\frac{1.396}{\sqrt n},\qquad \lvert\hat p_{i2}-0.954\rvert\gt3\sqrt{\frac{0.954\times0.046}n}=\frac{0.628}{\sqrt n}$$

- 비율이 **너무 작으면** 꼬리가 정규보다 **두껍다**(가운데에 덜 모여 있음).
- (검산) $3\sqrt{0.683\cdot0.317}=1.396$, $3\sqrt{0.954\cdot0.046}=0.628$ ✔

### 5.2 주변분포 — 정규 Q–Q plot (L p.12)

- $X\sim N(\mu,\sigma^2)$의 $u$-분위수는 $\mu+\sigma\Phi^{-1}(u)$ → $\Phi^{-1}(u)$의 **일차식**.
- 순서통계량 $x_{(j)}$는 $\frac{j-1/2}n$-분위수의 추정값 → 표준정규 분위수 $q_{(j)}=\Phi^{-1}\bigl(\frac{j-1/2}n\bigr)$.
- 순서: ① $x_{(1)}\le\cdots\le x_{(n)}$ 정렬, $q_{(j)}$ 계산 ② 점 $(q_{(j)},x_{(j)})$ 찍기 ③ 정규면 $x_{(j)}\approx\mu+\sigma q_{(j)}$ → **직선인지** 본다.
- 예 4.9 ( $n=10$): 직선 $\bar x+\sqrt{s_{11}}\thinspace q=0.770+0.970q$. ( $n=10$이면 $q_{(1)}=\Phi^{-1}(0.05)=-1.645$, $q_{(2)}=-1.036$, …, $q_{(10)}=1.645$.)

### 5.3 주변분포 — $r_Q$ 검정 (L p.13)

Q–Q 점들의 상관계수:

$$r_Q=\frac{\sum_{j=1}^n(x_{(j)}-\bar x)(q_{(j)}-\bar q)}{\sqrt{\sum_j(x_{(j)}-\bar x)^2}\sqrt{\sum_j(q_{(j)}-\bar q)^2}}$$

**$r_Q$가 임계값보다 작으면** 수준 $\alpha$에서 정규성 기각 (표 4.2, 일부):

| $n$ | $\alpha=0.01$ | $0.05$ | $0.10$ |
| ---: | ---: | ---: | ---: |
| 10 | .8801 | .9198 | .9351 |
| 20 | .9269 | .9508 | .9604 |
| 30 | .9479 | .9652 | .9715 |
| 50 | .9671 | .9768 | .9809 |
| 100 | .9822 | .9873 | .9895 |

- 예 4.11 (예 4.9 데이터): $r_Q=\frac{8.584}{\sqrt{8.472}\sqrt{8.795}}=0.994\gt0.9351$ → $\alpha=0.10$에서도 **기각 못 함**. (검산 ✔)
- 임계값은 $n$이 클수록, $\alpha$가 클수록 1에 가깝다 → 같은 $r_Q$라도 $n$이 크면 기각되기 쉽다.

### 5.4 쌍별 타원 — 50% 등고선 (L p.14)

$p=2$면 $(X-\mu)^\top\Sigma^{-1}(X-\mu)\sim\chi^2_2$이므로 타원 $\lbrace x:(x-\mu)^\top\Sigma^{-1}(x-\mu)\le\chi^2_2(0.5)\rbrace$의 확률이 0.5 ( $\chi^2_2(0.5)=1.39$).
$\mu,\Sigma$ 대신 $\bar x,S$를 넣은 타원이 **데이터의 약 절반**을 담아야 한다.

- 예 4.12 (매출·이익, $n=10$): 마할라노비스 거리 제곱 1.61, 0.30, 0.62, 1.79, 1.30, 4.38, 1.64, 3.53, 1.71, 1.16 → $1.39$ 이하는 4개(0.30, 0.62, 1.30, 1.16), 비율 0.40 vs 0.50. $n=10$으로는 기각하기에 너무 작다.
- (보충) $\chi^2_2(0.5)=-2\ln0.5=1.386$ ( $\chi^2_2$는 지수분포라 닫힌 꼴).

### 5.5 전체 적합·이상치 — 카이제곱 plot (L p.15\~16)

1. $d_j^2=(x_j-\bar x)^\top S^{-1}(x_j-\bar x)$ 계산, 정렬 $d^2_{(1)}\le\cdots\le d^2_{(n)}$.
2. 점 $\bigl(q_{c,p}((j-1/2)/n),\ d^2_{(j)}\bigr)$을 찍는다 ( $q_{c,p}(u)$ = $\chi^2_p$의 $u$-분위수).
3. **원점을 지나는 기울기 1인 직선**과 비교.

- **휘어짐** → 비정규, **직선 위로 멀리 튀는 몇 점** → 이상치, 약 절반의 $d_j^2\le q_{c,p}(0.5)$.
- $d_j^2$는 서로 독립도 아니고 정확히 $\chi^2_p$도 아니다 → $n$, $n-p$가 25\~30 이상일 때 **참고용**.
- 예 4.13 (매출·이익, $p=2$, $n=10$): 대체로 직선 → 기각하기 어렵다.
- 예 4.14 (판자 강성, $p=4$, $n=30$): $d_j^2\le q_{c,4}(0.5)=3.36$인 것이 17개(약 절반)지만 **가장 큰 두 점(9번, 16번 판자)이 튀어** 위쪽 끝이 휜다 → 이 두 판자를 따로 살펴봐야 한다.
- **$n$이 작으면** 큰 위반만 잡힌다. **$n$이 크면** 정규성 검정은 거의 항상 기각되지만, $\mu$에 대한 추론은 CLT 덕분에 **강건**하다(4.13).

### 5.6 4.7 이상치·4.8 정규 근사 변환 (10/19 필기)

- 강의 PDF에서는 4.6\~4.8이 Discussion 제목만 있다(p.178). 10/19 수업에서 다룬 내용:
- **Box–Cox 변환** 등으로 데이터를 정규에 가깝게 만들 수 있다. 하지만 요즘은 그럴 **필요가 줄었다** — 용도를 고려해 p-value를 직접 구하거나, **bootstrap** 등을 쓰면 되기 때문.
- 이상치 탐지에는 위 카이제곱 plot의 큰 $d_j^2$가 바로 쓰인다.

---

## 6. 예상 문제 (Lab 3 유형)

**L1.** $n=25$인 데이터에서 한 변수의 $\hat p_{i1}=0.56$, $\hat p_{i2}=0.80$이다. 비율 검사로 판정하라.
**L2.** $n=20$에서 $r_Q=0.9502$이다. $\alpha=0.01, 0.05$에서 각각 판정하라.
**L3.** $p=3$, $n=40$인 데이터의 카이제곱 plot에서 가장 큰 $d^2_{(40)}$에 짝지을 가로축 값과, 약 절반의 $d_j^2$가 넘지 않아야 할 값은?
**L4.** $S$가 랜덤인데 Result 4.13 (ii)가 " $S$가 가역"이 아니라 " $P(S\text{ 가역})\to1$"로 쓰는 이유와, $T_n\Rightarrow\chi^2_p$ 증명의 세 단계를 쓰라.
**L5.** MLE 증명에서 $\Sigma$ 대신 $\Omega=\Sigma^{-1}$로 최적화하는 이유는?

**풀이**
- **L1.** 경계 $1.396/5=0.279$, $0.628/5=0.126$. $\lvert0.56-0.683\rvert=0.123\lt0.279$ → 통과. $\lvert0.80-0.954\rvert=0.154\gt0.126$ → **비정규 의심**. 2 표준편차 안의 비율이 너무 작으므로 꼬리가 두껍다.
- **L2.** 표에서 $n=20$: $0.9269$(1%), $0.9508$(5%). $0.9502\lt0.9508$ → 5%에서 **기각**, $0.9502\gt0.9269$ → 1%에서는 기각 못 함.
- **L3.** $q_{c,3}(39.5/40)=q_{c,3}(0.9875)\approx10.86$, $q_{c,3}(0.5)\approx2.37$.
- **L4.** 유한 $n$에서는 $S$가 특이할 확률이 0이 아닐 수 있어서. ① $\lvert S\rvert\overset p\to\lvert\Sigma\rvert\gt0$로 $P(\text{가역})\to1$ ② $r$이 $\Sigma$에서 연속 → $r(S)\overset p\to\Sigma^{-1}$ ③ 슬러츠키로 $(Z_n,r(S))\Rightarrow(Z,\Sigma^{-1})$, CMT로 $T_n\Rightarrow Z^\top\Sigma^{-1}Z\sim\chi^2_p$.
- **L5.** $h(\Omega)=-\log\lvert\Omega\rvert+\mathop{\mathrm{tr}}\nolimits(\Omega S_0)$가 $\Omega$에 대해 강볼록( $D^2h[H]=\lVert\Omega^{-1/2}H\Omega^{-1/2}\rVert_F^2\gt0$)이라 1계 조건의 해가 곧 유일한 최소점이기 때문. 역행렬은 PD 집합 위의 전단사라 최적점을 그대로 옮길 수 있다.
