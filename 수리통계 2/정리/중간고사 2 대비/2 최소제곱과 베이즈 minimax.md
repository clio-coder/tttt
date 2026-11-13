# 2 최소제곱과 베이즈 minimax

> **범위:** 10강 (최소제곱), 11강 (베이즈 추정량, 위험, minimax). 필기 ✍p7–13, 녹음 `수리통계 1022 강의내용.pdf`.
> **자세한 판:** [ch6.1–6.3 노트 A, B](../ch6.1-6.3%20최소제곱·베이즈·UMVUE.md#a-최소제곱추정). 연습중간고사 4번 풀이는 [5](5%20연습중간고사와%20숙제%20풀이.md#연습-4-2모수-지수분포의-베이즈-추정량).
> **표시:** ✍ 수업 필기 · 📄 강의자료만 · 📝 연습시험·hw · (보충). 목차는 [README](README.md).

---

## 1. 최소제곱

### 1.1 단순회귀와 재모수화

✍p7 (10/20)

$Y_i=\beta_0+\beta_1x_i+\varepsilon_i$를 $\mathbb EY_i=\alpha+\beta(x_i-\bar x)$로 쓰면 $\beta_0=\alpha-\beta\bar x$ (중심화).
- $\hat\alpha=\bar Y$, $\hat\beta=\frac{S_{xy}}{S_{xx}}=\frac{\sum(x_i-\bar x)Y_i}{S_{xx}}$ (중심화하면 정규방정식이 대각).
- ✍ $\mathbb E\hat\alpha=\mathbb E\bar Y=\alpha$, $\mathbb E\hat\beta=\frac{\sum(x_i-\bar x)\lbrace\alpha+\beta(x_i-\bar x)\rbrace}{S_{xx}}=\beta\frac{\sum(x_i-\bar x)^2}{S_{xx}}=\beta$.
- $\mathrm{Var}\hat\alpha=\frac{\sigma^2}n$, $\mathrm{Var}\hat\beta=\frac{\sigma^2}{S_{xx}}$ → 기울기는 $S_{xx}\to\infty$일 때만 일치.
- 적합값 $\hat Y^\ast=\hat\beta_0^\ast+\hat\beta_1(x-\bar x)$.

### 1.2 행렬 LSE와 사영

✍p7–8

$X$: $n\times(p+1)$ 완전열계수, $\hat\beta=(X^\top X)^{-1}X^\top Y=\mathop{\mathrm{argmin}}_ b\Vert Y-Xb\Vert^2$, $\hat Y=X\hat\beta=P_XY$, $P_X=X(X^\top X)^{-1}X^\top$ ("hat matrix" $H$) = $C(X)$ 위로의 직교사영 (대칭·멱등).

✍ 판서 그림: $Y$를 $C(X)$에 떨어뜨린 발이 $P_XY$, 수직 성분이 잔차 $Y-P_XY=(I-P_X)Y$.

| | 정의 | 행렬 |
| --- | --- | --- |
| $SST$ | $\sum Y_i^2$ | $Y^\top Y$ |
| $SSE$ | $\Vert Y-X\hat\beta\Vert^2=\Vert(I-P_X)Y\Vert^2$ | $Y^\top(I-P_X)Y$ |
| $SSR$ | $\Vert X\hat\beta\Vert^2=\Vert P_XY\Vert^2$ | $Y^\top P_XY$ |
| $SST_{\mathrm{adj}}$ | $\sum(Y_i-\bar Y)^2$ | $Y^\top(I_n-P_0)Y$, $P_0=\frac1n\mathbf 1\mathbf 1^\top=\frac1nJ_n$ |

$SST=SSR+SSE$ (피타고라스), 절편이 있으면 $SST_{\mathrm{adj}}=SSR_{\mathrm{adj}}+SSE$. $J_n$은 모든 원소가 1인 행렬, $P_0Y=\bar Y\mathbf 1$ (평균 빼기 = 중심화).

### 1.3 분할과 직교화

✍p9–10 (10/22) — 수업에서 가장 길게 설명한 LSE 부분

$X=(X_0,X_1)$, $Y=X_0\beta_0+X_1\beta_1+\varepsilon$. 일반적으로 $X_0$와 $X_1$은 직교하지 않는다.

1. **직교화:** $P_0=X_0(X_0^\top X_0)^{-1}X_0^\top$, $\tilde X_1=(I-P_0)X_1$ ( $X_1$에서 $X_0$로 설명되는 부분 제거). $X_0=\mathbf 1$이면 $P_0=\frac1nJ_n$, $\tilde X_1$ = 중심화한 공변량.
2. **재모수화:** $X_1\beta_1=P_0X_1\beta_1+(I-P_0)X_1\beta_1$이므로

$$X_0\beta_0+X_1\beta_1=X_0\underbrace{\lbrace\beta_0+(X_0^\top X_0)^{-1}X_0^\top X_1\beta_1\rbrace}_ {\beta_0^\ast\ (\text{small shift})}+\tilde X_1\beta_1 .$$

   $C(X)=C(X_0)\oplus C(\tilde X_1)$ — 직교분해라 $C(X)$의 벡터는 $X_0\beta_0^\ast+\tilde X_1\beta_1$로 **유일하게** 표현된다. 모수가 $(\beta_0,\beta_1)$에서 $(\beta_0^\ast,\beta_1)$로 바뀐다 ( $\beta_1$은 그대로).
3. **사영의 분해:** $P_X=P_0+P_{1\mid0}$, $P_{1\mid0}=\tilde X_1(\tilde X_1^\top\tilde X_1)^{-1}\tilde X_1^\top$, $P_0P_{1\mid0}=0$ → $P_XY=P_0Y+P_{1\mid0}Y$.
4. **추정량:** 직교라 따로 적합한다.

$$\hat\beta_1=(\tilde X_1^\top\tilde X_1)^{-1}\tilde X_1^\top Y,\qquad\hat\beta_0^\ast=(X_0^\top X_0)^{-1}X_0^\top Y,\qquad\hat\beta_0=\hat\beta_0^\ast-(X_0^\top X_0)^{-1}X_0^\top X_1\hat\beta_1=(X_0^\top X_0)^{-1}X_0^\top(Y-X_1\hat\beta_1).$$

5. **해석 (수업 강조):** $\hat\beta_0$ = " $Y$에서 $X_1$이 설명하는 부분을 뺀 잔차에 $X_0$를 회귀한 계수", 대칭적으로 $\hat\beta_1$ = " $X_0$의 영향을 제거한 $X_1$에 회귀한 계수" (FWL). 다중회귀 계수는 **다른 변수를 통제한 관계**다 — 팔목 두께·키·몸무게 예: 둘을 동시에 넣으면 단순회귀와 부호가 달라질 수 있다.
6. **제곱합:** $Y^\top(I-P_X)Y+Y^\top P_{1\mid0}Y=Y^\top(I_n-P_0)Y$, 즉 $SSE+SSR_{\mathrm{adj}}=SST_{\mathrm{adj}}$ = $\Vert Y-\hat Y\Vert^2+\Vert\hat Y-\bar Y\mathbf 1\Vert^2=\Vert Y-\bar Y\mathbf 1\Vert^2$. ANOVA 표에서 제곱합이 직교분해되는 이유.

---

## 2. 베이즈 추정의 기초

✍p10 (10/22)

- 세 학파: 빈도주의, 가능도주의, 베이지안. MCMC (1990년대) 이후 베이즈가 실용화.
- $\theta\sim\pi(\theta)$ (사전), $p(x\mid\theta)$ (가능도), **사후분포**

$$\pi(\theta\mid x_1,\dots,x_n)=\frac{p(x_1,\dots,x_n\mid\theta)\pi(\theta)}{m(x_1,\dots,x_n)}\propto p(x\mid\theta)\pi(\theta).$$

  분모 $m(x)$는 $\theta$와 무관 → **$\theta$가 들어간 항만 모아 커널로 분포를 알아본다** (수업에서 가장 강조한 기법).
- **켤레사전분포:** 사후가 사전과 같은 족 → 주변분포를 닫힌 꼴로 계산.

| 가능도 | 켤레 사전 | 사후 |
| --- | --- | --- |
| 베르누이·이항 $\theta$ | $\mathrm{Beta}(\alpha,\beta)$ | $\mathrm{Beta}(\alpha+\sum x_i,\ \beta+n-\sum x_i)$ |
| 정규 $\mu$ ( $\sigma^2$ 기지) | $N(m,\tau^2)$ | $N\Big(\frac{\tau^2\bar x+\frac{\sigma^2}nm}{\tau^2+\frac{\sigma^2}n},\ \big(\frac1{\tau^2}+\frac n{\sigma^2}\big)^{-1}\Big)$ |
| 포아송 $\lambda$ | $\mathrm{Gamma}(\alpha,\text{rate }\beta)$ | $\mathrm{Gamma}(\alpha+\sum x_i,\ \beta+n)$ |
| 지수 율 $\lambda$ | $\mathrm{Gamma}(\alpha,\beta)$ | $\mathrm{Gamma}(\alpha+n,\ \beta+\sum x_i)$ |

- 하이퍼파라미터 ( $\alpha,\beta,m,\tau^2$): 전문가 지식으로 정하거나 자료로 추정 (**경험적 베이즈**, 엄격한 베이지안은 비판).

**베타 분포 (교수님: "시험 범위, 반드시 숙지")** $\mathrm{Beta}(\alpha,\beta)$: $\pi(\theta)=\frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}\theta^{\alpha-1}(1-\theta)^{\beta-1}$, 평균 $\frac\alpha{\alpha+\beta}$, 분산 $\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$, 최빈값 $\frac{\alpha-1}{\alpha+\beta-2}$ ( $\alpha,\beta > 1$), $\int_0^1\theta^{a-1}(1-\theta)^{b-1}d\theta=\frac{\Gamma(a)\Gamma(b)}{\Gamma(a+b)}$. ✍p11: $\alpha=\beta$이면 평균 $\frac12$, 분산 $\frac1{4(2\alpha+1)}$.

**✍ Beta–베르누이** 사후평균

$$\hat\theta_{\mathrm{Bayes}}=\frac{\alpha+\sum x_i}{\alpha+\beta+n}=\frac n{\alpha+\beta+n}\underbrace{\frac{\sum x_i}n}_ {\text{MLE}}+\frac{\alpha+\beta}{\alpha+\beta+n}\underbrace{\frac\alpha{\alpha+\beta}}_ {\text{사전평균}} .$$

$n\to\infty$이면 MLE, 자료가 적고 $\alpha+\beta$가 크면 사전에 끌려간다. (✏️ 필기에 이 식이 " $\hat\theta_{\mathrm{MAP}}(\hat\theta_{\mathrm{Bayes}})$"로 적혀 있는데, 이것은 **사후평균**이다. 사후최빈값 (MAP)은 $\frac{\alpha+\sum x_i-1}{\alpha+\beta+n-2}$)

**✍ 정규–정규** $\pi(\mu\mid x)\propto\exp\lbrace-\frac{(\mu-m)^2}{2\tau^2}\rbrace\exp\lbrace-\frac{\sum(x_i-\mu)^2}{2\sigma^2}\rbrace$ → 완전제곱.

$$\mathbb E(\mu\mid x)=\frac{\tau^2}{\tau^2+\frac{\sigma^2}n}\bar x+\frac{\frac{\sigma^2}n}{\tau^2+\frac{\sigma^2}n}m,\qquad\lbrace\mathrm{Var}(\mu\mid x)\rbrace^{-1}=\frac1{\tau^2}+\frac n{\sigma^2}.$$

**사후 정밀도 = 사전 정밀도 + 자료 정밀도** (정보가 더해진다). $\tau^2\to\infty$ (비정보 사전)이면 사후평균 → $\bar x$.

---

## 3. 손실과 위험

✍p11 (10/27)

| 손실 | $L(\theta,\hat\theta)$ |
| --- | --- |
| 제곱 | $\Vert\theta-\hat\theta\Vert_2^2$ → 위험 = MSE |
| 척도 불변 (분산 추정) | $(\hat\sigma^2/\sigma^2-1)^2$ |
| 절대 | $\lvert\theta-\hat\theta\rvert$ |
| 0-1 | $I(\theta\ne\hat\theta)$ |
| 큰 편차 | $I(\lvert\theta-\hat\theta\rvert\gt c)$ |
| KL | $\int\log\frac{p(x;\theta)}{p(x;\hat\theta)}p(x;\theta)dx=\mathbb E_\theta\log p_\theta(X)-\mathbb E_\theta\log p_{\hat\theta}(X)$ |

위험 $R(\theta,\hat\theta)=\mathbb E_\theta L(\theta,\hat\theta)$. ✍ 제곱손실이면

$$R=\mathbb E(\theta-\mathbb E\hat\theta+\mathbb E\hat\theta-\hat\theta)^2=(\mathbb E\hat\theta-\theta)^2+\mathbb E(\mathbb E\hat\theta-\hat\theta)^2=\mathrm{Bias}^2+\mathrm{Var}.$$

**✍ 위험 비교**
- $X\sim N(\theta,1)$: $\hat\theta_1=X$ (위험 1), $\hat\theta_2=3$ (위험 $(3-\theta)^2$) — 어느 쪽도 지배하지 않는다.
- 베르누이: $\hat p_1=\bar X$, $R=\frac{p(1-p)}n$. $\hat p_2=\frac{Y+\alpha}{\alpha+\beta+n}$ ( $\mathrm{Beta}(\alpha,\beta)$ 사전의 베이즈 추정량):

$$R(\hat p_2,p)=\frac{np(1-p)}{(\alpha+\beta+n)^2}+\Big(\frac{np+\alpha}{\alpha+\beta+n}-p\Big)^2 .$$

  ✍p12: $\alpha=\beta$이면 $=\frac{np(1-p)+\alpha^2(1-2p)^2}{(2\alpha+n)^2}$, $p^2$ 계수 $-n+4\alpha^2=0$ ⟺ **$\alpha=\frac{\sqrt n}2$** → $R(\hat p_2,p)=\frac n{4(n+\sqrt n)^2}$ (**상수 위험**). 그림: $\hat p_1$의 위험은 $p=\frac12$에서 최대인 포물선, $\hat p_2$는 수평선 — 가운데에서만 $\hat p_2$가 낫다.

---

## 4. 최대위험, 베이즈 위험, minimax

✍p12–13

$$R_{\max}(\hat\theta)=\sup_{\theta\in\Theta}R(\theta,\hat\theta),\qquad B_\pi(\hat\theta)=\int R(\theta,\hat\theta)\pi(\theta)d\theta .$$

- **minimax 추정량:** $R_{\max}$를 최소화, $\inf_{\hat\theta}\sup_\theta R(\theta,\hat\theta)$. **베이즈 추정량:** $B_\pi$를 최소화.
- 베르누이: $R_{\max}(\hat p_1)=\max_p\frac{p(1-p)}n=\frac1{4n}$, $R_{\max}(\hat p_2)=\frac n{4(n+\sqrt n)^2}$ → 최대위험으로는 $\hat p_2$가 낫다.
- ✍ 세 원리 비교: $\hat\theta_{\mathrm{MLE}}=\mathop{\mathrm{argmax}}_ \theta p(x\mid\theta)$, $\hat\theta_{\mathrm{MAP}}=\mathop{\mathrm{argmax}}_ \theta p(x\mid\theta)\pi(\theta)$ (사전 필요), $\hat\theta_{\mathrm{MM}}=\mathop{\mathrm{argmin}}_ \delta\sup_\theta\mathbb E_\theta(\theta-\delta(X))^2$.

**✍ 수업 예: MAP는 minimax가 아니다** $X_i\overset{\text{iid}}{\sim}N(\theta,\sigma^2)$.
- $\hat\theta_{\mathrm{MLE}}=\bar X$: $R(\theta,\bar X)=\mathrm{Var}\bar X+\mathrm{Bias}^2=\frac{\sigma^2}n$ → $\sup_\theta R=\frac{\sigma^2}n$.
- 사전 $\theta\sim N(0,\tau^2)$의 MAP (= 사후평균) $\hat\theta_{\mathrm{MAP}}=\frac{n\tau^2}{n\tau^2+\sigma^2}\bar X=c\bar X$, $c < 1$: $\mathbb E(c\bar X)-\theta=(c-1)\theta$, $\mathrm{Var}(c\bar X)=c^2\frac{\sigma^2}n$ →

$$R(\theta,c\bar X)=(1-c)^2\theta^2+c^2\frac{\sigma^2}n\to\infty\quad(\lvert\theta\rvert\to\infty).$$

  최대위험이 무한대라 minimax가 아니다 ( $\bar X$보다 나쁨). 축소 추정량은 $\theta$가 0 근처일 때만 이득.
- (보충) **$\bar X$는 minimax:** 임의의 $\delta$에 대해 $\sup_\theta R(\theta,\delta)\ge B_{\pi_\tau}(\delta)\ge B_{\pi_\tau}(c\bar X)=\frac{\sigma^2\tau^2/n}{\tau^2+\sigma^2/n}$ (사후분산) → $\tau\to\infty$로 보내면 $\sup R(\delta)\ge\frac{\sigma^2}n=\sup R(\bar X)$.

**(보충) 상수위험 베이즈 ⇒ minimax** $\delta_\pi$가 베이즈이고 위험이 상수 $r$이면 임의의 $\delta$에 대해 $\sup R(\delta)\ge B_\pi(\delta)\ge B_\pi(\delta_\pi)=r=\sup R(\delta_\pi)$. → $\hat p_2$ ( $\alpha=\beta=\frac{\sqrt n}2$)는 베르누이의 **minimax** 추정량.

---

## 5. 사후위험과 손실별 베이즈 추정량

✍p13 · 📄 Thm 11.1, 11.2

**Thm 11.1** $B_\pi(\hat\theta)=\iint L(\theta,\hat\theta(x))p(x\mid\theta)dx\thinspace\pi(\theta)d\theta=\iint L\thinspace\pi(\theta\mid x)m(x)d\theta dx=\int r(\hat\theta\mid x)m(x)dx$, **사후위험** $r(\hat\theta\mid x)=\int L(\theta,\hat\theta(x))\pi(\theta\mid x)d\theta$. ( $L\ge0$이라 적분 순서 교환 가능) → **각 $x$에서 사후위험을 최소화하면 베이즈 추정량.**

| 손실 | 베이즈 추정량 | 근거 |
| --- | --- | --- |
| $(\theta-a)^2$ ✍ | 사후평균 $\mathbb E(\theta\mid x)$ | $r(a)=\mathrm{Var}(\theta\mid x)+\lbrace\mathbb E(\theta\mid x)-a\rbrace^2$ |
| $\lvert\theta-a\rvert$ | 사후중앙값 | $r'(a)=2F(a\mid x)-1$, 볼록 |
| $I(\theta\ne a)$ | 사후최빈값 | $r(a)=1-\pi(a\mid x)$ (이산) |
| **$w(\theta)(\theta-a)^2$** 📝 | $\frac{\mathbb E[\theta w(\theta)\mid x]}{\mathbb E[w(\theta)\mid x]}$ | $r'(a)=-2\mathbb E[w(\theta-a)\mid x]=0$, $a$의 2차식 |

**가중 손실이 연습중간고사 4번의 열쇠다.** $(\hat\mu-\mu)^2/\sigma^2$는 $w=\sigma^{-2}$, $(\hat\sigma/\sigma-1)^2=(\hat\sigma-\sigma)^2/\sigma^2$도 $w=\sigma^{-2}$. 그래서

$$\hat\mu_B=\frac{\mathbb E[\mu\sigma^{-2}\mid x]}{\mathbb E[\sigma^{-2}\mid x]},\qquad\hat\sigma_B=\frac{\mathbb E[\sigma^{-1}\mid x]}{\mathbb E[\sigma^{-2}\mid x]}.$$

- 포아송–감마 + $L=\frac{(a-\lambda)^2}\lambda$ ( $w=\lambda^{-1}$): 사후 $\mathrm{Gamma}(A,B)$이면 $\frac{\mathbb E[1\mid x]}{\mathbb E[\lambda^{-1}\mid x]}=\frac1{B/(A-1)}=\frac{A-1}B$ (사후최빈값과 같다).
- 베르누이 + $L=\frac{(a-p)^2}{p(1-p)}$, 균등 사전: $\bar X$ (상수위험 $\frac1n$ → minimax).

---

## 6. 분산 추정의 minimax 클래스

✍p13 (10/29) · 📄 Ex 11.6 · 📝 hw3 #8과 같은 답

$X_i\overset{\text{iid}}{\sim}N(\mu,\sigma^2)$, $\hat\sigma^2_{\mathrm{MLE}}=\frac1n\sum(X_i-\bar X)^2$, $S^2=\frac1{n-1}\sum(X_i-\bar X)^2$ (불편). 손실 $(\hat\sigma^2/\sigma^2-1)^2$, 클래스 $\delta_c=c\sum(X_i-\bar X)^2$.

✍ $V=\frac{\sum(X_i-\bar X)^2}{\sigma^2}\sim\chi^2_{n-1}$ → $\mathbb EV=n-1$, $\mathbb EV^2=2(n-1)+(n-1)^2=n^2-1$.

$$R(\sigma^2,\delta_c)=\mathbb E(cV-1)^2=c^2(n^2-1)-2c(n-1)+1\quad(\sigma\text{와 무관}).$$

$c$로 미분: $2c(n^2-1)-2(n-1)=0$ → **$c=\frac1{n+1}$**, 위험 $\frac2{n+1}$. ( $c=\frac1{n-1}$: $\frac2{n-1}$, $c=\frac1n$: $\frac{2n-1}{n^2}$) — 불편 $S^2$보다 축소한 쪽이 위험이 작다.

---

## 7. 셀프 체크

<details><summary>Q1. 사후분포를 구할 때 m(x)를 계산하지 않아도 되는 이유는?</summary>

$m(x)$는 $\theta$와 무관한 정규화 상수다. $\theta$의 커널이 아는 분포 (베타, 감마, 정규 …)와 같으면 상수는 자동으로 정해진다.

</details>
<details><summary>Q2. 왜 cX̄ (c &lt; 1)는 minimax가 될 수 없나?</summary>

편향 $(c-1)\theta$의 제곱이 $\theta$와 함께 무한히 커져 최대위험이 무한대다. $\bar X$의 최대위험 $\sigma^2/n$보다 나쁘다.

</details>
<details><summary>Q3. 손실 (σ̂/σ − 1)²의 베이즈 추정량이 사후평균이 아닌 이유는?</summary>

$(\hat\sigma-\sigma)^2/\sigma^2$라 가중치 $w=\sigma^{-2}$가 붙는다. 사후위험을 $\hat\sigma$로 미분하면 $\hat\sigma=\mathbb E[\sigma^{-1}\mid x]/\mathbb E[\sigma^{-2}\mid x]$.

</details>
<details><summary>Q4. 분할 회귀에서 β̂₁은 왜 X₀를 신경 쓰지 않고 구해지나?</summary>

$\tilde X_1$이 $X_0$와 직교하도록 만들었기 때문이다. 직교분해는 유일해서 $C(\tilde X_1)$ 성분의 계수가 곧 $\hat\beta_1$이고, $\beta_0$가 $\beta_0^\ast$로 바뀌어도 $\beta_1$은 그대로다.

</details>
