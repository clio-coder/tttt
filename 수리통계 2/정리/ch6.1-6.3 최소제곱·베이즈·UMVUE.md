# ch6.1–6.3 최소제곱, 베이즈 추정, 완비성과 UMVUE

> **원본:** `수업자료/lecture_ch6.1_LSE estimator.pdf` (10강, 6쪽) · `수업자료/lecture_ch6.2_Bayes estimator.pdf` (11강, 7쪽) · `수업자료/lecture_ch6.3_Complete statistics and UMVUE.pdf` (12강, 10쪽) — 세 강의를 한 노트로 합쳤다. 출처 태그는 `[10강 p2]`, `[11강 p5]`, `[12강 p7]`.
> **수업 (강의계획서):** 10/13 최소제곱추정법 (교재 6.5) · 10/15 베이즈추정법 (11.2, 녹화) · 10/20 결정이론과 최소최대추정량 (11.3) · 10/22 최소분산불편추정 (8.3) · 10/27 추정량의 비교 (8.1) → **중간고사 2 (11/16) 범위.** 범위의 앞부분은 8강 후반 ([ch5.3–5.5 노트 §5–§8](ch5.3-5.5%20최대가능도추정.md#5-mle의-일치성))과 9강 ([ch5.6 노트](ch5.6%20MLE의%20점근이론%20II.md)), 뒷부분은 11/3–11/12 검정 수업 (강의자료 대기).
> **관련 숙제:** hw3 (마감 11/2) — #8 ( $aS^2$의 MSE와 첨도), #9 (균등 $(\theta,2\theta)$와 Rao–Blackwell)는 [hw3 풀이](#hw3-풀이)에 풀었다. #1–#7은 교재 연습문제 (6.22, 11.1, 11.4, 11.10, 11.11, 8.1, 8.8)인데 hw3.pdf에 번호만 있고 교재가 폴더에 없어서 **어느 절인지만 표로 정리했다.**
> **범례:** 📝 hw에 나옴 · 🧩 강의노트에 증명 없음 → 보충 증명 · 📐 강의노트에 증명 있음 · 🔢 계산 · 💡 개념 · (보충) 강의노트에 없는 내용 · `[T…]` = [도구상자](00%20문제풀이%20도구상자.md) 카드
> **기호:** 설계행렬 $X$ ( $n\times(p+1)$, 완전열계수), 열공간 $C(X)$, 사영 $P_X=X(X^\top X)^{-1}X^\top$ · 사전분포 $\pi(\theta)$, 사후분포 $\pi(\theta\mid x)$, 주변분포 $m(x)$ · 손실 $L(\theta,a)$, 위험 $R(\theta,\hat\theta)=\mathbb E_\theta L(\theta,\hat\theta)$, 베이즈 위험 $B_\pi$, 최대위험 $R_{\max}$ · 완비충분통계량 $T$. 12강의 $\mathrm{Exp}(\theta)$는 **평균** $\theta$인 척도모형이다.
> **더 연습하려면:** 수업 외 자료인 [튜터링3](튜터링/튜터링3%20최소제곱추정.md) (최소제곱, Gauss–Markov), [튜터링4](튜터링/튜터링4%20베이지안%20추론.md) (켤레사전분포 7문제), [튜터링8](튜터링/튜터링8%20결정이론과%20라오블랙웰.md) (결정이론·라오–블랙웰 10문제), [튜터링9](튜터링/튜터링9%20완비통계량과%20크래머라오%20부등식.md) (완비성·레만–쉐페 13문제).

---

## 0. 한눈에

### 0.1 범위

| 강의 | 쪽 | 절 | 내용 | 증명 | hw3 |
| --- | --- | --- | --- | --- | --- |
| 10강 | p1–2 | 10.1–10.2 | LSE 정의, 평균, **단순회귀** LSE, 불편성, 분산, 일치성 ( $S_{xx}\to\infty$) | 📐 (정규방정식 🧩) | (#1?) |
| 10강 | p2–4 | 10.3–10.4 | 다중회귀, 정규방정식, **사영행렬**, 잔차 직교, $SST=SSR+SSE$ | 🧩 | |
| 10강 | p4–6 | 10.5–10.6 | 블록 직교화, $P_X=P_0+P_{1\mid0}$, 계수 비교, **FWL**, 절편 분리, 보정 제곱합 | 📐 ((10.8) 🧩) | |
| 11강 | p1–2 | 11.1 | 사후분포, **Beta–베르누이**, **정규–정규** | 📐 / 🧩 | #2–#5 |
| 11강 | p2–5 | 11.2–11.4 | 손실함수, 위험, MSE 분해, 위험 비교 (Ex 11.3, 11.4), 최대위험·베이즈 위험·minimax | 🔢 | #8 |
| 11강 | p5–7 | 11.4 | **Thm 11.1** (베이즈 위험 = 사후위험의 평균), **Thm 11.2** (손실별 베이즈 추정량), **Ex 11.6** ( $cS^2$의 minimax) | 📐 (중앙값·최빈값 🧩) | #8 |
| 12강 | p1–2 | 12.1 | 충분통계량, 인수분해, MLE는 충분통계량의 함수, 최소충분, Ex 12.1 | (ch4) | #9 |
| 12강 | p2–4 | 12.2 | **Rao–Blackwell**, Ex 12.2 균등 $(0,\theta)$ | 📐 | #9 |
| 12강 | p4–7 | 12.3 | UMVUE, **유일성 (Thm 12.4)**, 완비성, **Lehmann–Scheffé (Thm 12.5)**, 지수족 완비성 (Thm 12.6), Ex 12.3 | 📐 (Thm 12.6 증명 없음) | #6, #7 |
| 12강 | p7–10 | 12.4 | UMVUE 찾는 두 방법, Ex 12.4 베르누이, 12.5 포아송, 12.6 지수, 12.7 균등, 12.8 $(-\theta,\theta)$ | 🔢 | #6, #7 |

### 0.2 시험 포인트 (출제 가능성 순)

근거: hw3 (시험 전 마지막으로 채점된 숙제) > 강의노트 증명 > 증명 없이 쓴 결과 > 예제. hw3의 교재 문제 배분 (11장 4문제, 8장 2문제)으로 보면 **베이즈와 UMVUE 비중이 크다.**

**★★★ 매우 유력**

1. 📝 **Rao–Blackwell로 개선하기** — 충분통계량이 주어졌을 때 조건부 기댓값 계산 (hw3 #9, Ex 12.2). 계산 요령: "최댓값이 주어지면 나머지는 iid 균등", 대칭성 $\mathbb E[X_1\mid T]=T/n$. → [C2](#c2-raoblackwell), [hw3 #9](#hw3-9-균등분포-θ-2θ와-raoblackwell)
2. 📐 **UMVUE 찾기** — 완비충분통계량 → Lehmann–Scheffé, 두 경로 (RB / 방정식 풀기). Ex 12.4 ( $\theta(1-\theta){}$), 12.5 ( $e^{-2\theta}$), 12.7 ( $\frac{n+1}nX_{(n)}$). → [C3](#c3-umvue-완비성-lehmannscheffé), [C4](#c4-umvue를-찾는-두-방법과-예)
3. 📝 **MSE 계산과 최소화** — 편향–분산 분해, $aS^2$ 꼴에서 최적 $a$ (hw3 #8, Ex 11.6은 같은 답 $\frac1{n+1}\sum(X_i-\bar X)^2$). → [B2](#b2-손실과-위험), [B6](#b6-정규평균과-분산의-minimax), [hw3 #8](#hw3-8-as2-꼴-추정량의-mse와-첨도)
4. 📝📐 **베이즈 추정량** — 켤레 사후분포 (커널 맞추기), 손실별 베이즈 추정량 (Thm 11.2), Thm 11.1의 증명. hw3 교재 11장 문제가 이것. → [B1](#b1-사후분포와-켤레사전분포), [B5](#b5-thm-111과-thm-112)

**★★ 유력**

5. 📐 **UMVUE 유일성 (Thm 12.4)과 Lehmann–Scheffé 증명**, 완비성 증명 (다항식·멱급수·라플라스·미분)과 비완비 반례 (Ex 12.3, 12.8). → [C3](#c3-umvue-완비성-lehmannscheffé), [C4](#c4-umvue를-찾는-두-방법과-예)
6. 🔢 위험함수 비교, 최대위험, Ex 11.4 상수위험 추정량. (보충) **상수위험 베이즈 ⇒ minimax.** → [B3](#b3-위험함수-비교), [B4](#b4-최대위험-베이즈-위험-minimax)
7. 🧩 LSE: 정규방정식 유도, 사영행렬 성질, 제곱합 분해, 단순회귀의 불편성·분산·일치성 조건. → [A2](#a2-단순선형회귀)–[A4](#a4-제곱합-분해)
8. 📐 블록 직교화와 FWL 정리. → [A5](#a5-블록-직교화와-fwl-정리)

**★ 가능성 있음**

9. 💡 손실함수 목록, 최소충분성 (ch4 복습), Ex 12.1, Ex 12.5의 $n=1$ 이상한 UMVUE, 지수족 완비성 정리의 조건.

### 0.3 이 장의 도구

| 도구 | 쓰는 곳 |
| --- | --- |
| [T15] 편향–분산 분해 | 위험 비교, hw3 #8, Ex 11.6 |
| [T31] (신규) **사영:** $P$ 대칭·멱등, $X^\top e=0$, 피타고라스, FWL | A3–A5 |
| [T32] (신규) **사후분포 = 가능도 × 사전의 커널 맞추기, 사후위험 최소화** | B1, B5, V3, V4 |
| [T33] (신규) **minimax:** 상수위험 베이즈 ⇒ minimax, 클래스 안에서는 위험을 $c$의 2차식으로 | B4, B6, hw3 #8 |
| [T34] (신규) **조건부 기댓값 계산:** 최댓값이 주어지면 나머지는 iid 균등, 대칭성, 조건부확률 | C2, C4, hw3 #9 |
| [T35] (신규) **완비성:** 다항식·멱급수 계수, 라플라스 변환, 적분을 모수로 미분 / 비완비는 평균 0인 함수 하나 | C3, C4, V8 |
| [T36] (신규) **UMVUE 두 경로:** RB $\mathbb E[\delta_0\mid T]$ / $\mathbb E\delta(T)=g(\theta)$를 풀기 (멱급수, PGF, 미분) | C4, V5–V7 |
| [T22] 분해정리 + 지시함수, [T16] 최댓값·최솟값의 분포 | C1, C2, hw3 #9 |

---

## A 최소제곱추정

### A1 정의와 평균모형

💡 [10강 p1]

독립인 $Y_1,\dots,Y_n$, 평균함수 $f_i(\theta)=\mathbb E_\theta Y_i$의 꼴을 안다고 하자. **최소제곱추정량 (LSE)**

$$\hat\theta_{\mathrm{LSE}}=\mathop{\mathrm{argmin}}_ {\theta\in\Theta}\sum_{i=1}^n\lbrace Y_i-f_i(\theta)\rbrace^2.\qquad(10.1)$$

- 정의에 "분산이 모두 같다"는 가정은 없다. 다만 분산이 다르면 가중하지 않은 (10.1)은 효율적이지 않을 수 있다 ((보충) 가중 최소제곱: $\sum(Y_i-f_i)^2/\sigma_i^2$).
- **Ex 10.1** $f_i(\mu)=\mu$: $Q'(\mu)=-2\sum(Y_i-\mu)=0$ → $\hat\mu=\bar Y$ ( $Q''=2n > 0$).

### A2 단순선형회귀

📐 [10강 p1–2] (정규방정식 🧩)

**모형** $x_i$ 고정, $Y_i$ 독립, $\mathbb EY_i=\alpha+\beta(x_i-\bar x)$, $\mathrm{Var}Y_i=\sigma^2$. $S_{xx}=\sum(x_i-\bar x)^2$, $S_{xy}=\sum(x_i-\bar x)(Y_i-\bar Y)$.

**🧩 정규방정식** $Q(\alpha,\beta)=\sum\lbrace Y_i-\alpha-\beta(x_i-\bar x)\rbrace^2$.
- $\partial_\alpha Q=-2\sum\lbrace Y_i-\alpha-\beta(x_i-\bar x)\rbrace=0$이고 $\sum(x_i-\bar x)=0$ → $\hat\alpha=\bar Y$.
- $\partial_\beta Q=-2\sum(x_i-\bar x)\lbrace Y_i-\alpha-\beta(x_i-\bar x)\rbrace=0$ → $\alpha$ 항이 사라져 $\sum(x_i-\bar x)Y_i=\beta S_{xx}$. $\sum(x_i-\bar x)\bar Y=0$이라 좌변 $=S_{xy}$ → $\hat\beta=S_{xy}/S_{xx}$ ( $S_{xx} > 0$). (10.2)
- 헤시안 $2\mathrm{diag}(n,S_{xx})$이 양정치 → 최소. **중심화하면 연립방정식이 대각이 된다.**

**불편성** 두 추정량은 선형결합 $\hat\alpha=\sum\frac1nY_i$, $\hat\beta=\sum\frac{x_i-\bar x}{S_{xx}}Y_i$. 평균모형과 $\sum(x_i-\bar x)=0$으로

$$\mathbb E\hat\alpha=\frac1n\sum\lbrace\alpha+\beta(x_i-\bar x)\rbrace=\alpha,\qquad\mathbb E\hat\beta=\frac1{S_{xx}}\sum(x_i-\bar x)\lbrace\alpha+\beta(x_i-\bar x)\rbrace=\beta .$$

**분산** 독립·등분산이면 $\mathrm{Var}(\sum w_iY_i)=\sigma^2\sum w_i^2$ → $\mathrm{Var}\hat\alpha=\frac{\sigma^2}n$, $\mathrm{Var}\hat\beta=\frac{\sigma^2}{S_{xx}}$. (보충) $\mathrm{Cov}(\hat\alpha,\hat\beta)=\sigma^2\sum\frac{x_i-\bar x}{nS_{xx}}=0$.

**일치성** [T14] Chebyshev: $\Pr(\lvert\hat\beta-\beta\rvert\gt\epsilon)\le\frac{\sigma^2}{S_{xx}\epsilon^2}$. $\hat\alpha$는 항상 일치, **$\hat\beta$는 $S_{xx}\to\infty$일 때** 일치. $n$이 커진다고 $S_{xx}$가 발산하지는 않는다 — (보충) $x_i=1/i^2$이면 $S_{xx}\le\sum x_i^2\le\sum i^{-4} < \infty$라서 분산이 0으로 가지 않는다.

- (보충) 원래 꼴 $\mathbb EY=a+bx$이면 $a=\alpha-\beta\bar x$ → $\hat a=\bar Y-\hat\beta\bar x$.

### A3 다중회귀와 사영행렬

🧩 [10강 p2–3]

**모형** $Y=X\beta+\varepsilon$, $\mathbb E\varepsilon=0$, $\mathrm{Var}\varepsilon=\sigma^2I_n$, $\mathrm{rank}X=p+1$. (10.3)

**🧩 정규방정식과 해** $\lVert Y-Xb\rVert^2=Y^\top Y-2b^\top X^\top Y+b^\top X^\top Xb$의 기울기 $-2X^\top Y+2X^\top Xb=0$ → $X^\top Xb=X^\top Y$. 완전열계수이면 $b\ne0$에서 $b^\top X^\top Xb=\lVert Xb\rVert^2 > 0$ → $X^\top X$ 양정치 → 가역, 목적함수 순볼록 →

$$\hat\beta_{\mathrm{LSE}}=(X^\top X)^{-1}X^\top Y,\qquad\hat Y=X\hat\beta=P_XY .\qquad(10.4),\ (10.5)$$

**🧩 사영행렬의 성질** [T31]
- 대칭: $P_X^\top=X(X^\top X)^{-1}X^\top=P_X$. 멱등: $P_X^2=X(X^\top X)^{-1}X^\top X(X^\top X)^{-1}X^\top=P_X$.
- $P_XX=X$ → $C(X)$의 벡터는 그대로, $P_XY\in C(X)$ → $P_X$는 $C(X)$ 위로의 **직교사영**. $I-P_X$도 대칭·멱등이고 $P_X(I-P_X)=0$.
- 잔차 $e=(I-P_X)Y$: $X^\top e=(X^\top-X^\top P_X)Y=0$ (잔차 ⟂ $C(X){}$).
- 최소 잔차제곱합 $=\lVert(I-P_X)Y\rVert^2=Y^\top(I-P_X)^\top(I-P_X)Y=Y^\top(I-P_X)Y$.
- (보충) 다른 증명: $\lVert Y-Xb\rVert^2=\lVert Y-X\hat\beta\rVert^2+\lVert X(\hat\beta-b)\rVert^2$ (교차항 $=2e^\top X(\hat\beta-b)=0$) → $b=\hat\beta$에서만 최소.

**(보충, 시험 단골) 평균·분산·σ² 추정**

$$\mathbb E\hat\beta=(X^\top X)^{-1}X^\top X\beta=\beta,\qquad\mathrm{Var}\hat\beta=(X^\top X)^{-1}X^\top(\sigma^2I)X(X^\top X)^{-1}=\sigma^2(X^\top X)^{-1}.$$

$(I-P_X)X=0$이라 $SSE=\varepsilon^\top(I-P_X)\varepsilon$, $\mathbb E[\varepsilon^\top A\varepsilon]=\sigma^2\mathrm{tr}A$, $\mathrm{tr}P_X=\mathrm{tr}\lbrace(X^\top X)^{-1}X^\top X\rbrace=p+1$ → $\mathbb E\thinspace SSE=\sigma^2(n-p-1)$, $\hat\sigma^2=\frac{SSE}{n-p-1}$이 불편. (선형 불편추정량 중 분산 최소 = Gauss–Markov → [튜터링3](튜터링/튜터링3%20최소제곱추정.md))
- 단순회귀 검산: $X=[\mathbf 1,\ x-\bar x]$이면 $X^\top X=\mathrm{diag}(n,S_{xx})$ → $\mathrm{Var}=\sigma^2\mathrm{diag}(\frac1n,\frac1{S_{xx}})$ ✓ (A2와 같다).

### A4 제곱합 분해

🧩 [10강 p3–4]

$SSE=Y^\top(I-P_X)Y$, $SSR=Y^\top P_XY=\lVert X\hat\beta\rVert^2$, $SST=Y^\top Y$ (**보정하지 않은** 총제곱합, $\sum Y_i^2$).

**🧩 $SST=SSR+SSE$ (10.6)** $Y=P_XY+(I-P_X)Y$이고 $(P_XY)^\top(I-P_X)Y=Y^\top P_X(I-P_X)Y=0$ → 피타고라스. (또는 $Y^\top Y=Y^\top\lbrace P_X+(I-P_X)\rbrace Y$)

- 절편이 있으면 상수 방향을 분리한다: $P_0=\frac1n\mathbf 1\mathbf 1^\top$, $SST_{\mathrm{adj}}=\sum(Y_i-\bar Y)^2=Y^\top(I-P_0)Y=SSR_{\mathrm{adj}}+SSE$ (유도는 A5 ⑦).

### A5 블록 직교화와 FWL 정리

📐 [10강 p4–6] ((10.8)은 🧩)

$X=(X_0,X_1)$ (완전열계수), $\beta=(\beta_0^\top,\beta_1^\top)^\top$, $P_0=X_0(X_0^\top X_0)^{-1}X_0^\top$, $\tilde X_1=(I-P_0)X_1$ (10.7) — $X_1$에서 $X_0$가 설명하는 성분을 뺀 것.

① **직교:** $X_0^\top\tilde X_1=X_0^\top(I-P_0)X_1=0$ ( $X_0^\top P_0=X_0^\top$) → $C(X_0)\perp C(\tilde X_1)$.

② **계수 변환:** $P_0X_1\beta_1=X_0(X_0^\top X_0)^{-1}X_0^\top X_1\beta_1$이므로

$$X_0\beta_0+X_1\beta_1=X_0\underbrace{\lbrace\beta_0+(X_0^\top X_0)^{-1}X_0^\top X_1\beta_1\rbrace}_ {\beta_0^\ast}+\tilde X_1\beta_1 .$$

되돌릴 수 있으므로 ( $\beta_0=\beta_0^\ast-(X_0^\top X_0)^{-1}X_0^\top X_1\beta_1$) $C(X)=C(X_0)\oplus C(\tilde X_1)$. **첫 계수는 $\beta_0^\ast$로 바뀌고 $\beta_1$은 그대로다.**

③ **🧩 $P_X=P_0+P_{1\mid0}$, $P_{1\mid0}=\tilde X_1(\tilde X_1^\top\tilde X_1)^{-1}\tilde X_1^\top$ (10.8).** $Q=P_0+P_{1\mid0}$로 두자.
- (a) $P_0\tilde X_1=0$, $\tilde X_1^\top P_0=0$ → $P_0P_{1\mid0}=P_{1\mid0}P_0=0$. 그래서 $Q$는 대칭이고 $Q^2=P_0^2+P_{1\mid0}^2=Q$ (멱등) → $Q$는 자기 열공간 위로의 직교사영.
- (b) $QX_0=X_0+0=X_0$, $Q\tilde X_1=0+\tilde X_1=\tilde X_1$ → $Q$는 $C(X_0)\oplus C(\tilde X_1)=C(X)$의 모든 벡터를 고정하고, $Q$의 열공간은 $C(X_0)+C(\tilde X_1)=C(X)$ 안에 있다 → $Q$는 $C(X)$ 위로의 직교사영.
- (c) 같은 부분공간 위로의 직교사영은 하나뿐이다 (둘 다 $y$에서 가장 가까운 점을 준다) → $Q=P_X$. $\blacksquare$

④ **계수 비교:** 같은 적합값을 두 가지로 쓴다. 위 식은 ②의 계수 변환, 아래 식은 ③의 사영 분해다.

$$X\hat\beta=X_0\hat\beta_0^\ast+\tilde X_1\hat\beta_1,$$

$$X\hat\beta=P_0Y+P_{1\mid0}Y=X_0(X_0^\top X_0)^{-1}X_0^\top Y+\tilde X_1(\tilde X_1^\top\tilde X_1)^{-1}\tilde X_1^\top Y .$$

직교분해는 유일하고 각 블록이 완전열계수이므로 성분별로 계수가 같다:

$$\hat\beta_1=(\tilde X_1^\top\tilde X_1)^{-1}\tilde X_1^\top Y,\qquad\hat\beta_0=(X_0^\top X_0)^{-1}X_0^\top(Y-X_1\hat\beta_1).\qquad(10.9),\ (10.10)$$

- $P_0Y=X_0\hat\beta_0^\ast$이지 $X_0\hat\beta_0$가 아니다. $Y-X_1\hat\beta_1$은 전체 잔차가 아니라 $X_1$의 기여만 뺀 **부분잔차**다.

⑤ **FWL (부분회귀):** $\tilde Y=(I-P_0)Y$로 두면 $\tilde X_1^\top\tilde Y=X_1^\top(I-P_0)^2Y=\tilde X_1^\top Y$ → $\hat\beta_1=(\tilde X_1^\top\tilde X_1)^{-1}\tilde X_1^\top\tilde Y$. **$Y$와 $X_1$을 각각 $X_0$에 대해 잔차화한 뒤 회귀한 계수 = 전체 회귀의 $\hat\beta_1$.**

⑥ **절편 분리 ( $X_0=\mathbf 1$):** $P_0Y=\bar Y\mathbf 1$, $\tilde X_1$ = 각 열에서 평균을 뺀 것, $\hat\beta_0^\ast=\bar Y$, $\hat\beta_0=\bar Y-\bar X_1^\top\hat\beta_1$. 중심화 모형의 절편 $\bar Y$와 원래 절편 $\hat\beta_0$를 구별하자. (단순회귀면 $\hat\beta_1=S_{xy}/S_{xx}$ — A2와 같다)

⑦ **보정 제곱합:** $I-P_0=(P_X-P_0)+(I-P_X)=P_{1\mid0}+(I-P_X)$이고 $P_{1\mid0}(I-P_X)=P_{1\mid0}-P_{1\mid0}P_X=0$ ( $C(\tilde X_1)\subset C(X){}$) → $(I-P_0)Y=P_{1\mid0}Y+e$가 직교분해

$$\underbrace{Y^\top(I-P_0)Y}_ {SST_{\mathrm{adj}}}=\underbrace{Y^\top P_{1\mid0}Y}_ {SSR_{\mathrm{adj}}}+\underbrace{Y^\top(I-P_X)Y}_ {SSE}.$$

**셀프 체크**
<details><summary>Q1. FWL에서 Y를 잔차화하지 않아도 같은 계수가 나오는 이유는?</summary>

$\tilde X_1^\top P_0=0$이라 $\tilde X_1^\top Y=\tilde X_1^\top\tilde Y$. $X_1$만 잔차화하면 충분하다 (단 잔차의 제곱합은 달라진다).

</details>
<details><summary>Q2. 기울기 LSE가 일치하지 않을 수 있는 이유는?</summary>

$\mathrm{Var}\hat\beta=\sigma^2/S_{xx}$이고 $S_{xx}$는 설계점의 퍼짐이 정한다. 설계점이 한 점 근처에 모이면 $S_{xx}$가 유계로 남아 분산이 0으로 가지 않는다.

</details>

---

## B 베이즈 추정과 결정이론

### B1 사후분포와 켤레사전분포

📐 [11강 p1–2] (Ex 11.2 유도는 🧩)

$\theta$를 확률변수로 보고 사전분포 $\pi(\theta)$를 둔다. Bayes 정리로

$$\pi(\theta\mid x)=\frac{p(x\mid\theta)\pi(\theta)}{m(x)},\qquad m(x)=\int p(x\mid\theta)\pi(\theta)d\theta,\qquad\pi(\theta\mid x)\propto L(\theta)\pi(\theta).$$

사후평균 $\hat\theta=\mathbb E(\theta\mid x)=\frac{\int\theta p(x\mid\theta)\pi(\theta)d\theta}{\int p(x\mid\theta)\pi(\theta)d\theta}$. [T32] **$\theta$에 대한 커널만 보고 아는 분포로 맞춘다.**

**Ex 11.1 (Beta–베르누이)** $Y=\sum X_i$, $\pi(\theta)\propto\theta^{\alpha-1}(1-\theta)^{\beta-1}$:

$$\pi(\theta\mid X)\propto\theta^Y(1-\theta)^{n-Y}\cdot\theta^{\alpha-1}(1-\theta)^{\beta-1}=\theta^{Y+\alpha-1}(1-\theta)^{n-Y+\beta-1}\ \Rightarrow\ \theta\mid X\sim\mathrm{Beta}(Y+\alpha,n-Y+\beta).$$

사후평균 $\tilde\theta=\frac{Y+\alpha}{n+\alpha+\beta}=(1-\lambda)\hat\theta_{\mathrm{MLE}}+\lambda m$, $\hat\theta_{\mathrm{MLE}}=Y/n$, $m=\frac\alpha{\alpha+\beta}$ (사전평균), $\lambda=\frac{\alpha+\beta}{\alpha+\beta+n}$.
- 검산: $(1-\lambda)\frac Yn+\lambda m=\frac n{\alpha+\beta+n}\frac Yn+\frac{\alpha+\beta}{\alpha+\beta+n}\frac\alpha{\alpha+\beta}=\frac{Y+\alpha}{n+\alpha+\beta}$ ✓.
- 해석: 사전분포 = 성공 $\alpha$번, 실패 $\beta$번의 가상 관측. $n\to\infty$이면 $\lambda\to0$ (자료가 사전을 압도). 사후분포가 사전과 같은 족 → **켤레사전분포**.

**Ex 11.2 (정규–정규)** $\sigma^2$ 알려짐, $\mu\sim N(m,\tau^2)$:

$$\mathbb E(\mu\mid X)=\frac{\tau^2}{\tau^2+\sigma^2/n}\bar X+\frac{\sigma^2/n}{\tau^2+\sigma^2/n}m,\qquad\mathrm{Var}(\mu\mid X)=\frac{\sigma^2\tau^2/n}{\tau^2+\sigma^2/n}.$$

**🧩 유도 (완전제곱)**
- (a) $\sum(x_i-\mu)^2=\sum(x_i-\bar x)^2+n(\bar x-\mu)^2$ → $\mu$에 대한 커널은 $\exp\lbrace-\frac n{2\sigma^2}(\mu-\bar x)^2-\frac1{2\tau^2}(\mu-m)^2\rbrace$.
- (b) 지수의 $\mu^2$ 계수 $-\frac12\big(\frac n{\sigma^2}+\frac1{\tau^2}\big)$ → 사후정밀도 $\frac1v=\frac n{\sigma^2}+\frac1{\tau^2}$ (**정밀도가 더해진다**), $v=\frac{\sigma^2\tau^2/n}{\tau^2+\sigma^2/n}$.
- (c) $\mu$의 1차 계수 $\frac{n\bar x}{\sigma^2}+\frac m{\tau^2}$ → 사후평균 $v\big(\frac{n\bar x}{\sigma^2}+\frac m{\tau^2}\big)$ = 위 식 (정밀도 가중평균). 사후분포는 $N(\text{평균},v)$.

### B2 손실과 위험

🧩 [11강 p2–3]

| 손실 | $L(\theta,\hat\theta)$ |
| --- | --- |
| 제곱오차 | $(\theta-\hat\theta)^2$ |
| 절대오차 | $\lvert\theta-\hat\theta\rvert$ |
| $L_p$ | $\lvert\theta-\hat\theta\rvert^p$ |
| 0-1 | $I(\theta\ne\hat\theta)$ |
| 큰 편차 | $I(\lvert\hat\theta-\theta\rvert\gt c)$ |
| Kullback–Leibler | $\int\log\frac{p(x;\theta)}{p(x;\hat\theta)}p(x;\theta)dx$ |

벡터 모수는 $\lVert\theta-\hat\theta\rVert^2$, 분류기는 $I(Y\ne h(X))$. **위험함수** $R(\theta,\hat\theta)=\mathbb E_\theta L(\theta,\hat\theta(X))$. 따로 밝히지 않으면 제곱오차 → 위험 = MSE.

**🧩 편향–분산 분해** [T15] $\mathbb E_\theta(\hat\theta-\theta)^2=\mathbb E_\theta(\hat\theta-\mathbb E\hat\theta+\mathbb E\hat\theta-\theta)^2=\mathrm{Var}_ \theta(\hat\theta)+\mathrm{Bias}_ \theta(\hat\theta)^2$ (교차항 $2(\mathbb E\hat\theta-\theta)\mathbb E(\hat\theta-\mathbb E\hat\theta)=0$).

### B3 위험함수 비교

🔢 [11강 p3–4]

**Ex 11.3** $X\sim N(\theta,1)$: $\hat\theta_1=X$의 위험 1, $\hat\theta_2=3$의 위험 $(3-\theta)^2$. $2 < \theta < 4$에서는 $\hat\theta_2$가, 밖에서는 $\hat\theta_1$이 낫다 → **어느 위험함수도 다른 것을 지배하지 않는다.**

**Ex 11.4** 베르누이, $\hat p_1=\bar X$: $R(p,\hat p_1)=\frac{p(1-p)}n$. 사후평균 $\hat p_2=\frac{Y+\alpha}{\alpha+\beta+n}$:

$$R(p,\hat p_2)=\frac{np(1-p)}{(\alpha+\beta+n)^2}+\Big(\frac{np+\alpha}{\alpha+\beta+n}-p\Big)^2.$$

**🧩 $\alpha=\beta=\sqrt n/2$이면 상수:** 분모 $n+\sqrt n$, 편향 $\frac{np+\sqrt n/2-p(n+\sqrt n)}{n+\sqrt n}=\frac{\sqrt n(\frac12-p)}{n+\sqrt n}$ →

$$R(p,\hat p_2)=\frac{n\lbrace p(1-p)+(\frac12-p)^2\rbrace}{(n+\sqrt n)^2}=\frac{n}{4(n+\sqrt n)^2}\quad(\because p-p^2+\tfrac14-p+p^2=\tfrac14).$$

### B4 최대위험 베이즈 위험 minimax

💡 [11강 p4–5]

$$R_{\max}(\hat\theta)=\sup_{\theta\in\Theta}R(\theta,\hat\theta),\qquad B_\pi(\hat\theta)=\int R(\theta,\hat\theta)\pi(\theta)d\theta .$$

- **베이즈 추정량:** $B_\pi(\hat\theta)=\inf_{\tilde\theta}B_\pi(\tilde\theta)$. **minimax 추정량:** $\sup_\theta R(\theta,\hat\theta)=\inf_{\tilde\theta}\sup_\theta R(\theta,\tilde\theta)$. 그 값 $R_n$이 minimax 위험.
- Ex 11.4: $R_{\max}(\hat p_1)=\max_p\frac{p(1-p)}n=\frac1{4n}$, $R_{\max}(\hat p_2)=\frac1{4(\sqrt n+1)^2}$ → 최대위험으로는 $\hat p_2$가 낫다. 그러나 (보충) $\hat p_1$이 더 나은 곳은 $p(1-p) < \frac n{4(\sqrt n+1)^2}$, 즉 $\lvert p-\frac12\rvert\gt\frac12\sqrt{\frac{2\sqrt n+1}{(\sqrt n+1)^2}}$ — $n$이 크면 $\frac12$ 근처의 좁아지는 구간 밖 전부다. **단일 요약값의 한계.**

**(보충) 상수위험 베이즈 ⇒ minimax** [T33] (강의계획서 10/20 "최소최대추정량"의 핵심 도구, [튜터링8](튜터링/튜터링8%20결정이론과%20라오블랙웰.md))

**정리** $\delta_\pi$가 사전 $\pi$에 대한 베이즈 추정량이고 위험이 상수 $r$이면 $\delta_\pi$는 minimax다.
**증명** 임의의 $\delta$에 대해 $\sup_\theta R(\theta,\delta)\ge\int R(\theta,\delta)\pi(\theta)d\theta=B_\pi(\delta)\ge B_\pi(\delta_\pi)=\int r\thinspace\pi=r=\sup_\theta R(\theta,\delta_\pi)$. (첫 부등식: 평균 ≤ 상한, 둘째: 베이즈의 정의) $\blacksquare$
- 따라서 $\hat p_2=\frac{Y+\sqrt n/2}{n+\sqrt n}$ ( $\mathrm{Beta}(\frac{\sqrt n}2,\frac{\sqrt n}2)$ 사전의 사후평균, 상수위험)은 제곱손실에서 베르누이 $p$의 **minimax 추정량**이다.

### B5 Thm 11.1과 Thm 11.2

📐 [11강 p5–6] (중앙값·최빈값은 🧩)

**Thm 11.1** $B_\pi(\hat\theta)=\int r(\hat\theta\mid x)m(x)dx$, **사후위험** $r(\hat\theta\mid x)=\int L(\theta,\hat\theta(x))\pi(\theta\mid x)d\theta$. 따라서 **각 $x$에서 사후위험을 최소화하는 추정량이 베이즈 추정량**이다.

**증명** 결합밀도 $p(x,\theta)=p(x\mid\theta)\pi(\theta)=\pi(\theta\mid x)m(x)$로

$$B_\pi(\hat\theta)=\int\Big\lbrace\int L(\theta,\hat\theta(x))p(x\mid\theta)dx\Big\rbrace\pi(\theta)d\theta=\iint L\thinspace p(x,\theta)dxd\theta=\int\Big\lbrace\int L\thinspace\pi(\theta\mid x)d\theta\Big\rbrace m(x)dx .$$

적분 순서 교환은 $L\ge0$이라 Tonelli로 정당하다. $m(x)\ge0$이므로 각 $x$에서 안쪽을 최소화하면 가중평균도 최소다. $\blacksquare$

**Thm 11.2** 손실별 베이즈 추정량:

| 손실 | 베이즈 추정량 |
| --- | --- |
| $(\theta-\hat\theta)^2$ | 사후평균 $\mathbb E(\theta\mid X=x)$ |
| $\lvert\theta-\hat\theta\rvert$ | 사후중앙값 |
| $I(\theta\ne\hat\theta)$ | 사후최빈값 (mode) |

**제곱손실 (강의)** $r(a\mid x)=\int(\theta-a)^2\pi(\theta\mid x)d\theta$를 $a$로 미분: $2\int(a-\theta)\pi(\theta\mid x)d\theta=0$ → $a=\mathbb E(\theta\mid x)$.
- (보충) 미분 없이: $r(a\mid x)=\mathrm{Var}(\theta\mid x)+\lbrace\mathbb E(\theta\mid x)-a\rbrace^2$ → 최솟값은 사후분산.

**🧩 절대손실 → 중앙값** (사후분포가 연속, CDF $F$, $\mathbb E\lvert\theta\rvert < \infty$)
- (a) $a\mapsto\lvert\theta-a\rvert$가 볼록 → 평균인 $r(a)=\mathbb E[\lvert\theta-a\rvert\mid x]$도 볼록.
- (b) $\frac d{da}\lvert\theta-a\rvert=-\mathrm{sign}(\theta-a)$ ( $\theta\ne a$, 확률 1) → $r'(a)=\Pr(\theta\lt a\mid x)-\Pr(\theta\gt a\mid x)=2F(a)-1$.
- (c) 볼록함수에서 $r'(a)=0$ ⟺ 전역 최소 → $F(a)=\frac12$, 즉 사후중앙값. $\blacksquare$

**🧩 0-1 손실 → 최빈값**
- 이산 사후분포: $r(a\mid x)=\Pr(\theta\ne a\mid x)=1-\pi(a\mid x)$ → $\pi(a\mid x)$가 가장 큰 $a$ = 최빈값.
- 연속이면 모든 $a$에서 $\Pr(\theta=a)=0$이라 위험이 모두 1이다. 큰 편차 손실 $I(\lvert\theta-a\rvert\gt c)$로 바꾸면 $r=1-\Pr(\lvert\theta-a\rvert\le c\mid x)\approx1-2c\thinspace\pi(a\mid x)$ → $c\to0$에서 최빈값 (직관적 정당화).

### B6 정규평균과 분산의 minimax

🔢 [11강 p6–7]

**Ex 11.5** $\mu\sim N(a,b^2)$이면 제곱손실 베이즈 추정량 = 사후평균 $\frac{b^2}{b^2+\sigma^2/n}\bar X+\frac{\sigma^2/n}{b^2+\sigma^2/n}a$ (B1과 같다).

**Ex 11.6 (분산의 minimax, 클래스 안)** $X_i\overset{\text{iid}}{\sim}N(\mu,\sigma^2)$, $n\ge2$, 손실 $L(\theta,a)=(a/\sigma^2-1)^2$, 클래스 $\delta_c=cS^2$ ( $c > 0$), **여기서 $S^2=\sum(X_i-\bar X)^2$ ( $n-1$로 나누지 않음).**
- $S^2/\sigma^2\sim\chi^2_{n-1}$ → $\mathbb E=n-1$, $\mathrm{Var}=2(n-1)$.
- 편향–분산: $R(\theta,\delta_c)=c^2\mathrm{Var}(S^2/\sigma^2)+\lbrace c\mathbb E(S^2/\sigma^2)-1\rbrace^2=2c^2(n-1)+\lbrace c(n-1)-1\rbrace^2$ — **$\theta$와 무관** → 최대위험 = 이 값. [T33]
- $c$로 미분: $4c(n-1)+2(n-1)\lbrace c(n-1)-1\rbrace=0$ → $c(n+1)=1$ → $c^\ast=\frac1{n+1}$ (2차식이라 최소).
- 위험: $\frac{2(n-1)}{(n+1)^2}+\frac4{(n+1)^2}=\frac2{n+1}$.

| $c$ | 추정량 | 위험 |
| --- | --- | --- |
| $\frac1{n-1}$ | 불편 표본분산 | $\frac2{n-1}$ |
| $\frac1n$ | MLE | $\frac{2n-1}{n^2}$ |
| $\frac1{n+1}$ | 클래스 안 minimax | $\frac2{n+1}$ (가장 작음) |

- 정규분포 hw3 #8 (c)와 **같은 추정량**이다: $\frac{n-1}{n+1}\times\frac{S^2}{n-1}=\frac{S^2}{n+1}$. 손실이 MSE를 $\sigma^4$로 나눈 것이라 같은 답이 나온다.
- 강의노트의 결론은 "클래스 $\lbrace cS^2\rbrace$ 안에서" 최대위험을 최소화한다는 것이다.

---

## C 충분성, Rao–Blackwell, UMVUE

### C1 충분통계량 복습

💡 [12강 p1–2] (증명은 [ch4 노트](ch4%20자료축약.md))

- **충분통계량:** $\Pr_\theta(X\mid T=t)$가 $\theta$에 의존하지 않는 $T$. **인수분해정리 (Thm 12.1):** $p_\theta(x)=h(x)g_\theta(T(x))$ ⟺ $T$ 충분.
- **MLE는 충분통계량의 함수:** $L(\theta;x)=g_\theta(T(x))h(x)$에서 $h$는 $\theta$와 무관 → 최대점은 $T(x)$로만 정해진다. MLE가 유일하지 않으면 최대점 **집합**이 $T$의 함수이고, 그중 하나를 고르는 규칙은 따로 정해야 한다.
- **최소충분통계량:** 충분하고, 모든 충분통계량 $S$에 대해 $T=a(S)$. 일대일 변환을 빼면 유일. **Thm 12.2 (가능도비 판정법):** $p_\theta(y)/p_\theta(x)$가 $\theta$와 무관 ⟺ $T(x)=T(y)$이면 $T$는 최소충분 (ch4 [T23]).
- **Ex 12.1** $U(\theta_1-\theta_2,\theta_1+\theta_2)$: $\eta_1=\theta_1-\theta_2$, $\eta_2=\theta_1+\theta_2$로 $L=(\eta_2-\eta_1)^{-n}I(\eta_1\lt X_{(1)},\ X_{(n)}\lt\eta_2)$ → $(X_{(1)},X_{(n)})$ 최소충분. (지지집합이 모수에 의존해서 Thm 12.2의 "공통 지지집합" 가정은 엄밀히는 맞지 않는다. ch4처럼 지시함수를 직접 비교하면 된다: $(\min,\max)$가 다르면 한쪽 지시함수만 1이 되는 $\eta$가 있다)

### C2 Rao–Blackwell

📐 [12강 p2–4]

**Thm 12.3** $T$가 충분, $\delta(X)$가 $g(\theta)$의 추정량이면 $\delta^\ast(T)=\mathbb E_\theta[\delta(X)\mid T]$는 $g(\theta)$의 추정량이고

$$\mathbb E_\theta\delta^\ast=\mathbb E_\theta\delta,\qquad\mathrm{Var}_ \theta(\delta^\ast)\le\mathrm{Var}_ \theta(\delta),\qquad\mathrm{MSE}_ \theta(\delta^\ast)\le\mathrm{MSE}_ \theta(\delta).$$

**증명**
- (a) **추정량인 이유 (충분성이 쓰이는 곳):** $X\mid T$의 분포가 $\theta$와 무관하므로 $\mathbb E[\delta(X)\mid T]$는 $\theta$ 없이 계산된다. 충분하지 않은 통계량으로 조건부 기댓값을 취하면 결과가 $\theta$를 포함해 추정량이 아니다.
- (b) 반복기댓값: $\mathbb E_\theta\delta^\ast=\mathbb E_\theta\lbrace\mathbb E(\delta\mid T)\rbrace=\mathbb E_\theta\delta$.
- (c) 조건부 분산 분해: $\mathrm{Var}(\delta)=\mathrm{Var}\lbrace\mathbb E(\delta\mid T)\rbrace+\mathbb E\lbrace\mathrm{Var}(\delta\mid T)\rbrace\ge\mathrm{Var}(\delta^\ast)$.
- (d) 평균이 같아 편향이 같으므로 MSE도 증가하지 않는다. 등호는 $\mathrm{Var}(\delta\mid T)=0$ a.s., 즉 $\delta$가 이미 $T$의 함수일 때뿐. $\blacksquare$

**Ex 12.2 ( $U(0,\theta)$에서 $2\bar X$를 개선)** $M=X_{(n)}$은 충분, $f_M(y)=ny^{n-1}/\theta^n$.
- **강의의 방법:** $(X_{(k)},M)$의 결합밀도 $\frac{n!}{(k-1)!(n-k-1)!}\frac{x^{k-1}(y-x)^{n-k-1}}{\theta^n}$ ( $0 < x < y < \theta$)를 $f_M$으로 나누고 $R_k=X_{(k)}/M$으로 바꾸면 $R_k\mid M=y\sim\mathrm{Beta}(k,n-k)$ ( $y$, $\theta$와 무관) → $\mathbb E[X_{(k)}\mid M=y]=\frac kny$ ( $k=1,\dots,n-1$). 따라서

$$\mathbb E[2\bar X\mid M=y]=\frac2n\Big(y+\sum_{k=1}^{n-1}\frac kny\Big)=\frac{2y}n\Big(1+\frac{n-1}2\Big)=\frac{n+1}ny .$$

- **(보충) 빠른 방법 [T34]:** $M=y$가 주어지면 나머지 $n-1$개 관측은 (순서를 잊으면) iid $U(0,y)$ → $\mathbb E[\sum X_i\mid M=y]=y+(n-1)\frac y2=\frac{n+1}2y$. 또는 대칭성: $\Pr(X_1=M)=\frac1n$이라 $\mathbb E[X_1\mid M=y]=\frac1ny+\frac{n-1}n\frac y2$.
- **분산:** $\mathrm{Var}\big(\frac{n+1}nM\big)=\frac{\theta^2}{n(n+2)}\le\frac{\theta^2}{3n}=\mathrm{Var}(2\bar X)$, $n > 1$이면 엄격 (✏️ 강의노트는 우변을 "MSE( $\hat\theta_{\mathrm{MLE}}$)"라고 썼지만 $\frac{\theta^2}{3n}$은 $2\bar X$의 분산이다).

### C3 UMVUE 완비성 Lehmann–Scheffé

📐 [12강 p4–7]

**UMVUE** $\hat g$가 $g(\theta)$의 불편추정량이고 모든 불편추정량 $\tilde g$와 **모든** $\theta$에서 $\mathrm{Var}_ \theta(\hat g)\le\mathrm{Var}_ \theta(\tilde g)$.

**Thm 12.4 (유일성)** 분산이 유한한 UMVUE가 있으면 유일하다: $\Pr_\theta(\hat g_1=\hat g_2)=1$.
**증명** 공통 분산 $V=\mathrm{Var}(\hat g_1)=\mathrm{Var}(\hat g_2)$ (둘 다 UMVUE라 서로 이하), $C=\mathrm{Cov}(\hat g_1,\hat g_2)$.
- (a) $\frac{\hat g_1+\hat g_2}2$도 불편 → UMVUE의 최소성으로 $\mathrm{Var}\big(\frac{\hat g_1+\hat g_2}2\big)\ge V$.
- (b) 전개하면 $\mathrm{Var}\big(\frac{\hat g_1+\hat g_2}2\big)=\frac14(V+V+2C)=\frac12(V+C)$ → $C\ge V$.
- (c) $\mathrm{Var}(\hat g_1-\hat g_2)=2V-2C\le0$ (✏️ 강의노트는 계수를 $\frac12$로 씀) → 분산 0 → $\hat g_1-\hat g_2$는 상수 a.s. 평균이 $g-g=0$이므로 그 상수는 0. $\blacksquare$

**완비성** $T$가 완비 ⟺ 모든 함수 $a$에 대해 " $\mathbb E_\theta a(T)=0\ \forall\theta$ ⟹ $\Pr_\theta(a(T)=0)=1\ \forall\theta$" (✏️ 강의노트는 $=0$으로 씀). 충분성이 정보를 보존한다면 **완비성은 $T$의 함수인 불편추정량을 하나로 만든다.**

**Thm 12.5 (Lehmann–Scheffé)** $T$가 완비충분이고 $g(\theta)$의 불편추정량이 있으면, 임의의 불편추정량 $\delta$에 대해 $\mathbb E[\delta\mid T]$가 $g(\theta)$의 UMVUE다. $T$의 함수인 불편추정량은 유일하며 UMVUE다.
**증명**
- (a) RB: $\delta^\ast=\mathbb E(\delta\mid T)$는 불편, $\mathrm{Var}\delta^\ast\le\mathrm{Var}\delta$.
- (b) 다른 불편추정량 $\tilde\delta$도 RB하면 $\tilde\delta^\ast=\mathbb E(\tilde\delta\mid T)$ ( $T$의 함수, 불편).
- (c) $\mathbb E_\theta(\delta^\ast-\tilde\delta^\ast)=0$이 모든 $\theta$에서 성립 → 완비성으로 $\delta^\ast=\tilde\delta^\ast$ a.s.
- (d) $\mathrm{Var}\delta^\ast=\mathrm{Var}\tilde\delta^\ast\le\mathrm{Var}\tilde\delta$ — 모든 불편 $\tilde\delta$보다 작거나 같다 → UMVUE (유일성은 Thm 12.4). $\blacksquare$

**Thm 12.6 (지수족의 완비성)** $f_\alpha(x)=\exp\lbrace\sum_{j=1}^k\alpha_jT_j(x)-\psi(\alpha)\rbrace f_0(x)$에서 ① 지지집합이 모수와 무관 ② 자연모수공간 $A$가 $\mathbb R^k$의 **열린 직육면체를 포함** ③ $c\ne0$이면 $c^\top T(X)$가 상수가 아님 — 이면 $Y=\sum_iT(X_i)$는 $\alpha$에 대한 완비충분통계량이다.
- 증명은 강의노트에 없다. (보충) 아이디어: $\mathbb E_\alpha a(Y)=0$이 열린 집합의 모든 $\alpha$에서 성립 → $a^+$와 $a^-$로 정한 두 측도의 라플라스 변환이 열린 집합에서 같다 → 라플라스 변환의 유일성으로 두 측도가 같다 → $a=0$ a.e. 시험에서는 **조건을 확인하고 쓰는** 정리다.

**Ex 12.3 ( $N(\mu,\mu^2)$, 완비가 아닌 충분통계량)** $f=\exp\lbrace\frac1\mu x-\frac1{2\mu^2}x^2-\frac12-\frac12\log(2\pi\mu^2)\rbrace$ → $\alpha_1=\frac1\mu$, $\alpha_2=-\frac1{2\mu^2}=-\frac12\alpha_1^2$. $A$는 곡선이라 열린 직육면체를 포함하지 않는다 → Thm 12.6을 쓸 수 없다 (✏️ 강의노트는 "조건 3"이라 썼지만 깨지는 것은 조건 ②).
- 직접 반례: $Y=(Y_1,Y_2)=(\sum X_i,\sum X_i^2)$은 충분. $\mathbb EX_i^2=\mu^2+\mu^2=2\mu^2$ → $\mathbb EY_2=2n\mu^2$, $\mathbb EY_1^2=\mathrm{Var}Y_1+(\mathbb EY_1)^2=n\mu^2+n^2\mu^2=n(n+1)\mu^2$.

$$g(Y)=\frac{Y_2}{2n}-\frac{Y_1^2}{n(n+1)}\ \Rightarrow\ \mathbb E_\mu g(Y)=\mu^2-\mu^2=0\quad\forall\mu\gt 0,$$

$g\not\equiv0$ → **충분하지만 완비가 아니다.** [T35]

### C4 UMVUE를 찾는 두 방법과 예

🔢 [12강 p7–10]

**두 방법** [T36] ① **RB:** 계산하기 쉬운 불편추정량 $\delta_0$ → $\mathbb E(\delta_0\mid T)$. ② **직접:** $T$의 함수 $\delta_1(T)$를 정해 $\mathbb E_\theta\delta_1(T)=g(\theta)$를 푼다. 둘 다 완비충분통계량의 함수인 불편추정량 → Lehmann–Scheffé로 UMVUE.

**Ex 12.4 (베르누이)** $T=\sum X_i\sim\mathrm{Bin}(n,\theta)$.
- **완비성:** $\sum_ya(y)\binom ny\theta^y(1-\theta)^{n-y}=0\ \forall\theta$ → $(1-\theta)^n$으로 나누고 $z=\frac\theta{1-\theta}\in(0,\infty)$: $\sum_ya(y)\binom nyz^y=0\ \forall z > 0$. 양의 실수 전체에서 0인 다항식은 계수가 모두 0 → $a(y)=0$.
- $\mathbb E(T/n)=\theta$ → $\bar X$가 UMVUE.
- **$g=\theta(1-\theta)$:** $\delta_0=X_1(1-X_2)$는 불편 (독립이라 $\theta(1-\theta){}$).

$$\mathbb E[\delta_0\mid T=y]=\Pr(X_1=1,X_2=0\mid T=y)=\frac{\theta(1-\theta)\binom{n-2}{y-1}\theta^{y-1}(1-\theta)^{n-y-1}}{\binom ny\theta^y(1-\theta)^{n-y}}=\frac{\binom{n-2}{y-1}}{\binom ny}=\frac{y(n-y)}{n(n-1)}.$$

  (🧩 $\frac{(n-2)!}{(y-1)!(n-y-1)!}\cdot\frac{y!(n-y)!}{n!}=\frac{y(n-y)}{n(n-1)}$) → UMVUE $\frac{T(n-T)}{n(n-1)}=\frac n{n-1}\bar X(1-\bar X)$ ([ch5.6 Ex 9.5](ch5.6%20MLE의%20점근이론%20II.md#62-ex-95-베르누이와-분산-함수)의 불편 보정과 같다).

**Ex 12.5 (포아송)** $T\sim\mathrm{Poisson}(n\theta)$.
- **완비성:** $e^{-n\theta}\sum_ya(y)\frac{(n\theta)^y}{y!}=0\ \forall\theta > 0$ → 멱급수가 0이면 계수가 0 → $a(y)=0$. $\bar X$가 UMVUE.
- **$g=e^{-2\theta}$ ( $n\ge2$):** $\sum_t\delta(t)e^{-n\theta}\frac{(n\theta)^t}{t!}=e^{-2\theta}$. $\lambda=n\theta$로 두고 $e^{-n\theta}$로 나누면

$$\sum_{t=0}^\infty\delta(t)\frac{\lambda^t}{t!}=e^{(1-2/n)\lambda}=\sum_{t=0}^\infty\frac{\lbrace(1-2/n)\lambda\rbrace^t}{t!}\ \Rightarrow\ \delta(T)=\Big(1-\frac2n\Big)^T.$$

- **PGF로:** $\mathbb Es^T=e^{n\theta(s-1)}=e^{-2\theta}$ ⟺ $s=1-\frac2n$. 같은 답.
- 큰 $n$에서 $(1-\frac2n)^T\approx e^{-2\bar X}$ (MLE). 그러나 $n=1$이면 $(-1)^T$ — 양수를 $\pm1$로 추정하는 엉터리. **UMVUE가 늘 합리적인 것은 아니다.**

**Ex 12.6 (지수, 평균 $\theta$)** $T=\sum X_i\sim\mathrm{Gamma}(n,\theta)$. $\mathbb Ea(T)=0\ \forall\theta$ ⟺ $\int_0^\infty a(t)t^{n-1}e^{-t/\theta}dt=0\ \forall\theta > 0$ → $a(t)t^{n-1}$의 라플라스 변환이 $(0,\infty)$에서 0 → 유일성으로 $a(t)t^{n-1}=0$, $a=0$ (a.e.). 완비충분이고 $\mathbb E(T/n)=\theta$ → $\bar X$가 UMVUE.

**Ex 12.7 (균등 $(0,\theta){}$)** $f_M(m)=\frac n\theta(\frac m\theta)^{n-1}I(0 < m < \theta)$. $\mathbb Ea(M)=0\ \forall\theta$ → $\int_0^\theta a(m)m^{n-1}dm=0\ \forall\theta$ → $\theta$로 미분 (미적분의 기본정리) → $a(\theta)\theta^{n-1}=0$ → $a=0$ (a.e.) → $M$은 완비. $\mathbb EM=\frac n{n+1}\theta$ → **$\frac{n+1}nX_{(n)}$이 UMVUE** (Ex 12.2의 RB 결과와 같다 — Lehmann–Scheffé가 말하는 대로).

**Ex 12.8 ( $U(-\theta,\theta){}$)** $(X_{(1)},X_{(n)})$은 충분하지만 완비가 아니다: $a(T)=X_{(1)}+X_{(n)}$은 $\mathbb E=-\frac{n-1}{n+1}\theta+\frac{n-1}{n+1}\theta=0$인데 0이 아니다.
- $f_\theta(x)=(2\theta)^{-n}I(\max_i\lvert x_i\rvert\lt\theta)\cdot1$ → $T=\max_i\lvert X_i\rvert$가 충분 (인수분해). $\lvert X_i\rvert\overset{\text{iid}}{\sim}U(0,\theta)$라 Ex 12.7로 완비 → $\frac{n+1}nT$가 UMVUE. **충분성과 완비성은 다른 성질이다.**

---

## hw3 풀이

📝 [hw3, 마감 11/2] · 제출 풀이는 폴더에 없어서 "내 풀이 점검" 칸은 비워 둔다.

### hw3 1–7 교재 연습문제

hw3.pdf에는 번호만 있다. 교재 (김우철, 수리통계학 2판)가 폴더에 없어서 문제 원문을 확인하지 못했다 — **교재 페이지를 사진으로 넣어 주면 풀이를 추가한다.**

| hw3 | 교재 | 교재 장의 주제 (강의계획서) | 이 노트에서 볼 곳 |
| --- | --- | --- | --- |
| #1 | 6.22 | 6장 추정 (6.5 최소제곱추정 = 10강) | [A](#a-최소제곱추정) |
| #2–#5 | 11.1, 11.4, 11.10, 11.11 | 11장 베이즈 (11.2 베이즈추정, 11.3 결정이론) | [B](#b-베이즈-추정과-결정이론) |
| #6, #7 | 8.1, 8.8 | 8장 (8.1 추정량의 비교, 8.3 최소분산불편추정) | [B2](#b2-손실과-위험), [C](#c-충분성-raoblackwell-umvue) |

### hw3 8 aS2 꼴 추정량의 MSE와 첨도

$X_i$ iid, 평균 $\mu$, 분산 $\sigma^2$, $S^2=\frac1{n-1}\sum(X_i-\bar X)^2$, 첨도 $\kappa=\mathbb E(X-\mu)^4/\sigma^4$. (a) $\mathrm{MSE}(aS^2)=a^2\mathrm{Var}(S^2)+(a-1)^2\sigma^4$ (b) $\mathrm{Var}(S^2)=\frac1n\big(\kappa-\frac{n-3}{n-1}\big)\sigma^4$ (c) 정규면 $\kappa=3$, 최적 $a=\frac{n-1}{n+1}$ (d) 일반 최적 $a$ (e) $\kappa\gtrless3$일 때 $a$의 위치.

**전략** (a) 편향–분산 [T15]. (b) $\mu=0$으로 두고 $(n-1)S^2=\frac{n-1}n\sum X_i^2-\frac1n\sum_{i\ne j}X_iX_j$로 쪼개 4차 적률을 센다. (c)–(e) MSE가 $a$의 2차식.

**모범 답안**

(a) $\mathbb ES^2=\sigma^2$이므로 $\mathbb E(aS^2)=a\sigma^2$, 편향 $(a-1)\sigma^2$. 편향–분산 분해로 $\mathrm{MSE}(aS^2)=\mathrm{Var}(aS^2)+(a-1)^2\sigma^4=a^2\mathrm{Var}(S^2)+(a-1)^2\sigma^4$.

(b) $S^2$은 이동에 불변이므로 $\mu=0$으로 둔다. $A=\sum_iX_i^2$, $B=\sum_{i\ne j}X_iX_j$로 두면 $n\bar X^2=\frac1n(A+B)$이고

$$(n-1)S^2=A-n\bar X^2=\frac{n-1}nA-\frac1nB .$$

- $\mathbb EA^2=\sum_i\mathbb EX_i^4+\sum_{i\ne j}\mathbb EX_i^2X_j^2=n\mu_4+n(n-1)\sigma^4$ ( $\mu_4=\kappa\sigma^4$).
- $\mathbb EB^2=\sum_{i\ne j}\sum_{k\ne l}\mathbb E[X_iX_jX_kX_l]$: 평균이 0이라 $\lbrace k,l\rbrace=\lbrace i,j\rbrace$일 때만 남는다 → $2n(n-1)\sigma^4$.
- $\mathbb E[AB]=\sum_m\sum_{i\ne j}\mathbb E[X_m^2X_iX_j]=0$ ( $i\ne j$라 적어도 한 인수가 홀로 남는다).

따라서

$$\mathbb E\lbrace(n-1)S^2\rbrace^2=\frac{(n-1)^2}{n^2}\lbrace n\mu_4+n(n-1)\sigma^4\rbrace+\frac{2n(n-1)}{n^2}\sigma^4,$$

$$\mathbb ES^4=\frac{\mu_4}n+\frac{n-1}n\sigma^4+\frac{2\sigma^4}{n(n-1)},\qquad\mathrm{Var}(S^2)=\mathbb ES^4-\sigma^4=\frac{\mu_4}n-\frac{n-3}{n(n-1)}\sigma^4=\frac1n\Big(\kappa-\frac{n-3}{n-1}\Big)\sigma^4 .$$

(마지막 단계: $\frac{n-1}n-1+\frac2{n(n-1)}=\frac{-(n-1)+2}{n(n-1)}=-\frac{n-3}{n(n-1)}$)

(c) $Z\sim N(0,1)$이면 부분적분으로 $\mathbb EZ^4=\int z^3\cdot z\phi(z)dz=3\int z^2\phi(z)dz=3$ ( $z\phi=-\phi'$) → $\kappa=3$. (b)에 넣으면 $\mathrm{Var}(S^2)=\frac{\sigma^4}n\cdot\frac{3(n-1)-(n-3)}{n-1}=\frac{2\sigma^4}{n-1}$ (Ex 9.6과 같다). $\mathrm{MSE}/\sigma^4=\frac{2a^2}{n-1}+(a-1)^2$를 $a$로 미분: $\frac{4a}{n-1}+2(a-1)=0$ → $a\big(\frac2{n-1}+1\big)=1$ → $a=\frac{n-1}{n+1}$ (2차 계수 양수라 최소). 즉 $\frac{n-1}{n+1}S^2=\frac1{n+1}\sum(X_i-\bar X)^2$ (Ex 11.6과 같은 추정량).

(d) $v=\mathrm{Var}(S^2)/\sigma^4$로 두면 $\mathrm{MSE}/\sigma^4=a^2v+(a-1)^2$, 미분 $2av+2(a-1)=0$ → $a=\frac1{1+v}$. 이제

$$(n-1)(1+v)=(n-1)+\frac{(n-1)\kappa-(n-3)}n=\frac{n^2-2n+3+(n-1)\kappa}n=\frac{n(n+1)+(\kappa-3)(n-1)}n$$

( $n(n+1)-3(n-1)=n^2-2n+3$) → $a=\dfrac{n-1}{(n+1)+(\kappa-3)(n-1)/n}$. $\kappa$ (모수)에 의존해서 실제로는 쓸 수 없다.

(e) 분모 $D=(n+1)+(\kappa-3)\frac{n-1}n$.
- (i) $\kappa > 3$ → $D > n+1$ → $a < \frac{n-1}{n+1}$.
- (ii) $\kappa < 3$ → $D < n+1$ → $a > \frac{n-1}{n+1}$. 그리고 $a=\frac1{1+v} < 1$ ⟺ $v > 0$, 즉 $\mathrm{Var}(S^2) > 0$ ( $S^2$이 상수가 아님). (다른 길: Jensen으로 $\kappa=\mathbb EZ^4\ge(\mathbb EZ^2)^2=1$이라 $(3-\kappa)\frac{n-1}n < 2$ → $D > n-1$ → $a < 1$)

**채점 포인트** (a) $\mathbb ES^2=\sigma^2$을 먼저 밝히기 · (b) $\mu=0$으로 두는 근거 (이동 불변), 세 기댓값을 **왜 그 값인지** 세기 · (c) 정규 4차 적률 유도 + 2차식 최소 확인 · (d) 대수 정리 · (e) $a < 1$의 근거 ( $v > 0$ 또는 $\kappa\ge1$).

### hw3 9 균등분포 θ 2θ와 Rao–Blackwell

$X_1,X_2,X_3\overset{\text{iid}}{\sim}U(\theta,2\theta)$, $\theta > 0$. (a) MoM (b) MLE $\hat\theta$와 $\mathbb E(k\hat\theta)=\theta$인 $k$ (c) 어느 추정량을 충분성으로 개선할 수 있나, 어떻게 (d) 자료 1.29, 0.86, 1.33에서 MoM과 MLE.

**전략** support가 $\theta$에 의존 → 지시함수 [T22]. 순서통계량의 평균은 $U(0,1)$로 표준화 [T16]. 개선은 Rao–Blackwell: 충분통계량의 함수가 아닌 쪽을 조건부 기댓값으로 [T34].

**모범 답안** (일반 $n$으로 풀고 $n=3$을 넣는다)

(a) $\mathbb EX=\frac{3\theta}2$ → $\bar X=\frac{3\tilde\theta}2$ → $\tilde\theta=\frac23\bar X$.

(b) $L(\theta)=\theta^{-n}\prod I(\theta\le x_i\le2\theta)=\theta^{-n}I\big(\frac{x_{(n)}}2\le\theta\le x_{(1)}\big)$. 구간에서 $\theta^{-n}$은 감소 → 왼쪽 끝 $\hat\theta=\frac{X_{(n)}}2$. $X=\theta(1+U)$, $U\sim U(0,1)$로 쓰면 $X_{(n)}=\theta(1+U_{(n)})$, $\mathbb EU_{(n)}=\frac n{n+1}$ → $\mathbb E\hat\theta=\frac\theta2\cdot\frac{2n+1}{n+1}$. 따라서 $k=\frac{2(n+1)}{2n+1}$, **$n=3$이면 $\mathbb E\hat\theta=\frac78\theta$, $k=\frac87$.**

(c) 인수분해로 $(X_{(1)},X_{(n)})$이 충분통계량이다 ( $h=1$, $g_\theta=\theta^{-n}I(\cdot){}$).
- MLE $\frac{X_{(3)}}2$는 이미 충분통계량의 함수 → RB해도 그대로다.
- MoM $\frac23\bar X$는 가운데 관측값에 의존해 충분통계량의 함수가 아니다 → **MoM을 개선할 수 있다.**
- 계산: $X_{(1)}=a$, $X_{(3)}=b$가 주어지면 가운데 값은 $U(a,b)$ (나머지 한 관측은 $(a,b)$ 위의 균등) → 조건부 평균 $\frac{a+b}2$. 따라서 $\mathbb E[\sum X_i\mid a,b]=a+b+\frac{a+b}2=\frac32(a+b)$이고

$$\mathbb E\Big[\frac23\bar X\ \Big\vert\ X_{(1)},X_{(3)}\Big]=\frac29\cdot\frac32(X_{(1)}+X_{(3)})=\frac{X_{(1)}+X_{(3)}}3 .$$

- 불편 검산: $\mathbb EX_{(1)}=\theta(1+\frac14)$, $\mathbb EX_{(3)}=\theta(1+\frac34)$ → 합 $3\theta$ ✓. RB 정리로 분산이 $\frac23\bar X$ 이하.

(d) $\bar X=\frac{1.29+0.86+1.33}3=1.16$ → $\tilde\theta=\frac23(1.16)\approx0.773$. $\hat\theta=\frac{1.33}2=0.665$. (가능한 $\theta$의 범위는 $[\frac{X_{(3)}}2,X_{(1)}]=[0.665,0.86]$이고 둘 다 그 안에 있다. 참고로 $k\hat\theta=0.76$, 개선된 추정량 $\frac{0.86+1.33}3=0.73$)

**(보충) 분산 비교 ( $n=3$, 정확한 값, 시뮬레이션으로 확인)**

| 추정량 | 편향 | MSE / $\theta^2$ |
| --- | --- | --- |
| MLE $\frac{X_{(3)}}2$ | $-\frac\theta8$ | $\frac1{40}$ |
| MoM $\frac23\bar X$ | 0 | $\frac1{81}\approx0.0123$ |
| $\frac87\hat\theta=\frac47X_{(3)}$ | 0 | $\frac3{245}\approx0.0122$ |
| RB $\frac{X_{(1)}+X_{(3)}}3$ | 0 | $\frac1{90}\approx0.0111$ |

- 계산: $\mathrm{Var}U_{(1)}=\mathrm{Var}U_{(3)}=\frac3{80}$, $\mathrm{Cov}(U_{(1)},U_{(3)})=\frac1{80}$ → $\mathrm{Var}(U_{(1)}+U_{(3)})=\frac1{10}$, $\mathrm{Var}\frac{X_{(1)}+X_{(3)}}3=\frac{\theta^2}{90}$.
- **불편추정량이 둘** ( $\frac47X_{(3)}$, $\frac{X_{(1)}+X_{(3)}}3$) 다 충분통계량의 함수다 → $(X_{(1)},X_{(3)})$은 **완비가 아니다** (차이의 평균이 0인데 0이 아님, [V8](#v8-균등분포-θ-2θ의-비완비성)). 그래서 Lehmann–Scheffé를 쓸 수 없고 RB 결과가 UMVUE라는 보장도 없다.

**채점 포인트** (b) 지시함수를 $\frac{x_{(n)}}2\le\theta\le x_{(1)}$로 정리하고 "감소 → 왼쪽 끝" · (c) "MLE는 이미 충분통계량의 함수"라는 이유, 조건부 분포 (가운데 값이 균등)를 근거와 함께 · (d) 수치.

---

## 변형 문제

난이도: `●` hw·예제의 분포만 바꿈 · `●●` 도구 두세 개 조합 · `●●●` 장을 넘나듦. 힌트부터 열고, 써 본 뒤 풀이를 연다. (더 많은 연습은 위의 튜터링 노트 링크)

### V1 원점을 지나는 회귀

`●` $Y_i=\beta x_i+\varepsilon_i$, $\varepsilon_i$ 독립, 평균 0, 분산 $\sigma^2$. LSE, 불편성, 분산, 일치성 조건.

<details><summary>힌트</summary>

A2와 같이 미분. $X$가 한 열 $x$인 다중회귀로 보면 $(x^\top x)^{-1}x^\top Y$.

</details>
<details><summary>풀이</summary>

$Q'(\beta)=-2\sum x_i(Y_i-\beta x_i)=0$ → $\hat\beta=\frac{\sum x_iY_i}{\sum x_i^2}$. $\mathbb E\hat\beta=\frac{\sum x_i\cdot\beta x_i}{\sum x_i^2}=\beta$, $\mathrm{Var}\hat\beta=\frac{\sigma^2\sum x_i^2}{(\sum x_i^2)^2}=\frac{\sigma^2}{\sum x_i^2}$. Chebyshev로 $\sum x_i^2\to\infty$이면 일치.

</details>

### V2 단순회귀를 행렬과 FWL로

`●●` $X=[\mathbf 1,x]$ (중심화하지 않음). (a) FWL로 $\hat\beta_1=S_{xy}/S_{xx}$를 유도 (b) $SSR_{\mathrm{adj}}=\hat\beta_1^2S_{xx}$ (c) $\mathbb E\thinspace SSE=\sigma^2(n-2)$.

<details><summary>힌트</summary>

$X_0=\mathbf 1$이면 $\tilde x=x-\bar x\mathbf 1$. $SSR_{\mathrm{adj}}=\lVert P_{1\mid0}Y\rVert^2$, $\mathrm{tr}P_X=2$.

</details>
<details><summary>풀이</summary>

(a) $\tilde x=(I-P_0)x=x-\bar x\mathbf 1$ → $\hat\beta_1=\frac{\tilde x^\top Y}{\tilde x^\top\tilde x}=\frac{\sum(x_i-\bar x)Y_i}{S_{xx}}=\frac{S_{xy}}{S_{xx}}$. (b) $P_{1\mid0}Y=\tilde x\hat\beta_1$ → $\lVert P_{1\mid0}Y\rVert^2=\hat\beta_1^2\tilde x^\top\tilde x=\hat\beta_1^2S_{xx}=S_{xy}^2/S_{xx}$. (c) A3의 보충: $\mathbb E\thinspace SSE=\sigma^2\mathrm{tr}(I-P_X)=\sigma^2(n-2)$ → $\hat\sigma^2=SSE/(n-2)$.

</details>

### V3 포아송과 감마 사전분포

`●` $X_i\overset{\text{iid}}{\sim}\mathrm{Poisson}(\lambda)$, $\lambda\sim\mathrm{Gamma}(\alpha,\beta)$ (밀도 $\propto\lambda^{\alpha-1}e^{-\beta\lambda}$). 사후분포, 제곱손실·0-1 손실의 베이즈 추정량, 사후평균을 MLE와 사전평균의 가중평균으로.

<details><summary>힌트</summary>

커널 $\lambda^{\sum x}e^{-n\lambda}\cdot\lambda^{\alpha-1}e^{-\beta\lambda}$. [T32]

</details>
<details><summary>풀이</summary>

$\pi(\lambda\mid x)\propto\lambda^{\alpha+\sum x_i-1}e^{-(\beta+n)\lambda}$ → $\mathrm{Gamma}(\alpha+\sum x_i,\beta+n)$. 사후평균 $\frac{\alpha+\sum x_i}{\beta+n}=\frac n{n+\beta}\bar X+\frac\beta{n+\beta}\cdot\frac\alpha\beta$ (사전평균 $\alpha/\beta$). 0-1 손실 (최빈값): $\frac{\alpha+\sum x_i-1}{\beta+n}$ ( $\alpha+\sum x_i > 1$일 때).

</details>

### V4 가중 제곱손실과 minimax

`●●●` 베르누이 $p$, 손실 $L(p,a)=\frac{(p-a)^2}{p(1-p)}$, 사전 $U(0,1)$. (a) 가중손실 $w(p)(p-a)^2$의 베이즈 추정량이 $\frac{\mathbb E[pw(p)\mid x]}{\mathbb E[w(p)\mid x]}$임을 보여라 (b) 이 문제의 베이즈 추정량이 $\bar X$임을 보여라 (c) $\bar X$가 이 손실에서 minimax임을 보여라.

<details><summary>힌트</summary>

(a) 사후위험을 $a$로 미분. (b) 사후 $\mathrm{Beta}(y+1,n-y+1)$에 $w$를 곱하면 $\mathrm{Beta}(y,n-y)$ 커널. (c) 위험이 상수 → B4의 정리.

</details>
<details><summary>풀이</summary>

(a) $r(a)=\int w(p)(p-a)^2\pi(p\mid x)dp$, $r'(a)=-2\int w(p)(p-a)\pi(p\mid x)dp=0$ → $a=\frac{\int pw\pi}{\int w\pi}$ ( $r$은 $a$의 2차식이라 최소).

(b) $w(p)\pi(p\mid x)\propto p^{y-1}(1-p)^{n-y-1}$ ( $0 < y < n$) = $\mathrm{Beta}(y,n-y)$ 커널 → 비율 = 그 분포의 평균 $\frac y{y+n-y}=\frac yn$. ( $y=0$이면 $\int\frac{(p-a)^2}p(1-p)^{n-1}dp$가 유한하려면 $a=0=\frac yn$, $y=n$도 같음)

(c) $R(p,\bar X)=\frac{\mathbb E(\bar X-p)^2}{p(1-p)}=\frac{p(1-p)/n}{p(1-p)}=\frac1n$ — 상수. 베이즈 추정량이 상수위험이므로 B4의 정리로 minimax.

</details>

### V5 포아송에서 k의 확률

`●●` $X_i\overset{\text{iid}}{\sim}\mathrm{Poisson}(\theta)$, $\eta_k=\Pr(X_1=k)=e^{-\theta}\theta^k/k!$. (a) MLE (b) UMVUE (RB로) (c) $k=0$일 때 두 추정량의 극한분포를 비교하라. (24-2 중간고사 #1과 같은 설정)

<details><summary>힌트</summary>

$\delta_0=I(X_1=k)$는 불편. $X_1\mid T=t\sim\mathrm{Bin}(t,\frac1n)$.

</details>
<details><summary>풀이</summary>

(a) 불변성: $\hat\eta_k=e^{-\bar X}\bar X^k/k!$.

(b) $\mathbb E[I(X_1=k)\mid T=t]=\Pr(X_1=k\mid T=t)=\binom tk\big(\frac1n\big)^k\big(1-\frac1n\big)^{t-k}$ ( $X_1$과 $\sum_{i\ge2}X_i$가 독립 포아송이라 조건부 이항). $T$가 완비충분 (Ex 12.5) → Lehmann–Scheffé로 UMVUE $\binom Tk n^{-k}(1-\frac1n)^{T-k}$.

(c) $k=0$: MLE $e^{-\bar X}$, UMVUE $(1-\frac1n)^T=\exp\lbrace T\log(1-\frac1n)\rbrace=e^{-\bar X}\lbrace1+O_P(\frac1n)\rbrace$. 차이가 $O_P(1/n)$이라 $\sqrt n$ 스케일에서 같은 극한 $N(0,\theta e^{-2\theta})$ (델타 방법). 유한 표본에서 UMVUE의 분산은 $e^{-2\theta}(e^{\theta/n}-1)$ ([ch5.6 V4](ch5.6%20MLE의%20점근이론%20II.md#v4-포아송의-영-확률과-하한)).

</details>

### V6 균등분포에서 g의 UMVUE

`●●` $X_i\overset{\text{iid}}{\sim}U(0,\theta)$, $g$는 미분 가능. (a) $g(\theta)$의 UMVUE가 $g(M)+\frac{Mg'(M)}n$임을 보여라 ( $M=X_{(n)}$) (b) $\theta^2$과 $1/\theta$ ( $n\ge2$)의 UMVUE.

<details><summary>힌트</summary>

직접 경로 [T36]: $\int_0^\theta\delta(m)nm^{n-1}dm=g(\theta)\theta^n$을 $\theta$로 미분.

</details>
<details><summary>풀이</summary>

(a) $\mathbb E\delta(M)=g(\theta)$ ⟺ $\int_0^\theta\delta(m)nm^{n-1}dm=g(\theta)\theta^n$. 미분: $\delta(\theta)n\theta^{n-1}=g'(\theta)\theta^n+ng(\theta)\theta^{n-1}$ → $\delta(\theta)=g(\theta)+\frac{\theta g'(\theta)}n$. $M$이 완비충분 (Ex 12.7)이라 UMVUE. ( $g=\theta$이면 $\frac{n+1}nM$ ✓)

(b) $\theta^2$: $M^2+\frac{2M^2}n=\frac{n+2}nM^2$. $1/\theta$: $\frac1M-\frac1{nM}=\frac{n-1}{nM}$ (검산: $\mathbb E\frac1M=\frac n{(n-1)\theta}$).

</details>

### V7 지수분포에서 생존확률의 UMVUE

`●●●` $X_i\overset{\text{iid}}{\sim}\mathrm{Exp}(\text{평균 }\theta)$, $c > 0$, $\eta=\Pr(X_1 > c)=e^{-c/\theta}$. UMVUE를 구하라.

<details><summary>힌트</summary>

$\delta_0=I(X_1 > c)$로 RB. $X_1/T\sim\mathrm{Beta}(1,n-1)$이고 $T$와 독립.

</details>
<details><summary>풀이</summary>

$X_1/T$는 $T$와 독립인 $\mathrm{Beta}(1,n-1)$ (감마의 성질) → $\Pr(X_1 > c\mid T=t)=\Pr\big(\frac{X_1}T > \frac ct\big)=\big(1-\frac ct\big)^{n-1}$ ( $t > c$), $t\le c$이면 0. $T$는 완비충분 (Ex 12.6) → UMVUE $\big(1-\frac cT\big)^{n-1}I(T > c)$. 큰 $n$에서 $\approx e^{-c/\bar X}$ (MLE).

</details>

### V8 균등분포 θ 2θ의 비완비성

`●●` $X_i\overset{\text{iid}}{\sim}U(\theta,2\theta)$. $(X_{(1)},X_{(n)})$이 완비가 아님을 보이고, hw3 #9의 두 불편추정량의 차이와 연결하라.

<details><summary>힌트</summary>

$\mathbb EX_{(1)}=\theta\frac{n+2}{n+1}$, $\mathbb EX_{(n)}=\theta\frac{2n+1}{n+1}$. 둘 다 $\theta$에 비례 → 선형결합으로 평균 0. [T35]

</details>
<details><summary>풀이</summary>

$a(T)=(n+2)X_{(n)}-(2n+1)X_{(1)}$이면 $\mathbb Ea(T)=\frac\theta{n+1}\lbrace(n+2)(2n+1)-(2n+1)(n+2)\rbrace=0$ ∀θ인데 $a(T)\not\equiv0$ → 완비 아님. $n=3$: $\frac{X_{(1)}+X_{(3)}}3-\frac47X_{(3)}=\frac{7X_{(1)}-5X_{(3)}}{21}=-\frac{a(T)}{21}$. 두 불편추정량이 다른 것이 바로 비완비성 때문이다 (척도모형에서 $X_{(1)}/X_{(n)}$의 분포가 $\theta$와 무관한 것과 같은 현상).

</details>

---

## 셀프 테스트

### 백지 증명 체크리스트

- [ ] 단순회귀 정규방정식 → $\hat\alpha=\bar Y$, $\hat\beta=S_{xy}/S_{xx}$, 불편성·분산·일치성 조건.
- [ ] $\hat\beta=(X^\top X)^{-1}X^\top Y$, $P_X$ 대칭·멱등, $X^\top e=0$, $SST=SSR+SSE$, (보충) $\mathrm{Var}\hat\beta=\sigma^2(X^\top X)^{-1}$, $\mathbb E\thinspace SSE=\sigma^2(n-p-1)$.
- [ ] **$P_X=P_0+P_{1\mid0}$** (a)(b)(c) → 계수 비교 → FWL.
- [ ] Beta–베르누이, 정규–정규 사후분포 (완전제곱).
- [ ] **Thm 11.1** (Tonelli), **Thm 11.2** 평균 (미분 또는 분산 분해), 중앙값 (볼록 + $2F-1$), 최빈값.
- [ ] Ex 11.4 상수위험 계산, (보충) 상수위험 베이즈 ⇒ minimax.
- [ ] Ex 11.6과 hw3 #8: 위험을 $c$의 2차식으로 → $\frac1{n+1}$.
- [ ] **Rao–Blackwell** (a) 추정량인 이유 (b) 불편 (c) 분산 분해 (d) 등호.
- [ ] **Thm 12.4** (a)(b)(c), **Lehmann–Scheffé** (a)–(d).
- [ ] 완비성 증명 4종 (다항식, 멱급수, 라플라스, 미분)과 비완비 반례 2개 (Ex 12.3, 12.8).
- [ ] UMVUE 예: $\theta(1-\theta)$, $e^{-2\theta}$, $\frac{n+1}nX_{(n)}$, $\frac{n+1}n\max\lvert X_i\rvert$.
- [ ] hw3 #8 (b)의 4차 적률 세기, hw3 #9 (c)의 조건부 기댓값.

### 짧은 문답

<details><summary>Q1. Rao–Blackwell에서 충분성이 정확히 어디에 쓰이나?</summary>

조건부 기댓값 $\mathbb E[\delta\mid T]$가 $\theta$를 포함하지 않아 **추정량이 된다**는 데. 분산 감소 자체는 아무 통계량으로 조건부를 취해도 성립한다.

</details>
<details><summary>Q2. Lehmann–Scheffé에서 완비성이 없으면 무엇이 무너지나?</summary>

서로 다른 불편추정량을 RB한 결과들이 같다는 보장이 없다 (hw3 #9: $\frac47X_{(3)}$와 $\frac{X_{(1)}+X_{(3)}}3$). 그러면 어느 하나가 모든 불편추정량보다 낫다고 말할 수 없다.

</details>
<details><summary>Q3. 베이즈 추정량을 구할 때 왜 사후분포만 보면 되나?</summary>

Thm 11.1: 베이즈 위험 = 사후위험을 $m(x)$로 가중평균한 것. 각 $x$에서 사후위험을 최소화하면 전체도 최소가 된다.

</details>
<details><summary>Q4. 베르누이에서 표본비율은 minimax인가?</summary>

제곱손실에서는 아니다. 최대위험 $\frac1{4n}$이 상수위험 추정량 $\frac{Y+\sqrt n/2}{n+\sqrt n}$의 $\frac1{4(\sqrt n+1)^2}$보다 크다. 가중손실 $\frac{(p-a)^2}{p(1-p)}$에서는 minimax다 (V4).

</details>
<details><summary>Q5. N(μ, μ²)에서 Thm 12.6을 쓸 수 없는 이유는?</summary>

자연모수 $(\frac1\mu,-\frac1{2\mu^2})$가 곡선 위에 있어 열린 직육면체를 포함하지 않는다 (곡선 지수족). 실제로 평균이 0인 0이 아닌 함수가 있다.

</details>
<details><summary>Q6. SST를 Y의 제곱합으로 쓸 때와 편차제곱합으로 쓸 때의 차이는?</summary>

$SST=Y^\top Y$ (보정 없음)는 $SSR+SSE$로, $SST_{\mathrm{adj}}=Y^\top(I-P_0)Y$는 절편이 있을 때 $SSR_{\mathrm{adj}}+SSE$로 분해된다. 10강은 둘을 구별한다.

</details>
<details><summary>Q7. 0-1 손실의 베이즈 추정량이 최빈값이라는 말이 연속분포에서 엄밀하지 않은 이유는?</summary>

연속이면 모든 $a$에서 $\Pr(\theta=a\mid x)=0$이라 사후위험이 모두 1이다. 큰 편차 손실 $I(\lvert\theta-a\rvert\gt c)$의 $c\to0$ 극한으로 이해한다.

</details>

---

## 강의노트 확인

| 위치 | 강의노트 | 노트에 반영한 것 |
| --- | --- | --- |
| 10강 p4 (10.8) | $P_X=P_0+P_{1\mid0}$를 "위의 직교분해에 따라"로 넘어감 | (a)(b)(c) 보충 증명 (A5 ③) |
| 10강 p1–3 | 정규방정식·사영 성질은 결과만 | A2, A3에 유도 보충 |
| 11강 p6 | Thm 11.2는 제곱손실만 증명 | 중앙값 (볼록성), 최빈값 (이산·극한) 보충 (B5) |
| 11강 p6–7 Ex 11.6 | $S^2=\sum(X_i-\bar X)^2$ ( $n-1$로 나누지 않음) | 다른 절의 $S^2$과 정의가 다르다고 표시 (B6) |
| 11강 p7 | "maximum risk를 최소화하는 estimator" | 클래스 $\lbrace cS^2\rbrace$ 안에서의 결론임을 명시 |
| 12강 p2 | $(X_{(r)},X_{(s)})$ 결합밀도 식에 $[1-F(y)]^{n-r}$와 뒤에 $x^{k-1}(y-x)^{n-k-1}$이 붙어 있음 | 지수는 $n-s$이고 뒤의 인수는 다음 줄의 균등분포 식이 섞여 들어간 것 ✏️ (C2에는 균등분포 식만 씀) |
| 12강 p4 | " $\frac{\theta^2}{n(n+2)}\le\frac{\theta^2}{3n}=\mathrm{MSE}(\hat\theta_{\mathrm{MLE}})$" | 우변은 $2\bar X$ (MoM)의 분산 ✏️. MLE $X_{(n)}$의 MSE는 $\frac{2\theta^2}{(n+1)(n+2)}$ |
| 12강 p4 Thm 12.4 증명 | $\mathrm{Var}(\hat g_1-\hat g_2)=\frac12\lbrace V-C\rbrace$ | $2(V-C)$ ✏️ (결론은 같음) |
| 12강 p5 완비성 정의 | " $\Pr_\theta(a(T)=0)=0$" | $=1$ ✏️ |
| 12강 p5 Thm 12.6 | " $\lbrace x:f(\alpha x) > 0\rbrace$" | $f_\alpha(x)$ ✏️ |
| 12강 p6 Ex 12.3 | "조건 3이 성립하지 않으며" | 깨지는 것은 조건 ② (열린 직육면체) ✏️ |
| 12강 p1–2 Ex 12.1 | Thm 12.2를 균등분포에 적용 | 공통 지지집합 가정이 맞지 않아 지시함수 비교로 확인 (C1) |
| 12강 p9 Ex 12.6 | "따라서 이 경우 a(t)=0이다/ 해보인다" | 문장이 깨져 있음 → $a(t)t^{n-1}=0$ ⟹ $a=0$ a.e. |
| 12강 p8–9 Ex 12.5 | "완바 충분통계량", 조건 $n > 2$ | 완비 충분통계량. $n=2$도 $\delta=I(T=0)$로 성립 |
