# ch1 집중부등식 (Concentration Inequality)

> **원본:** `lecture_ch1_concentration_inequality.pdf` (12쪽, 9/3 수업, 9/9 갱신판). 이전 판 `1주차_0903_Concentration_Inequality.pdf`는 쓰지 않는다.
> **관련 숙제:** hw1 #1 (Hoeffding's lemma 증명) · #2 (Bernstein vs Hoeffding) · #4 (sub-Gaussian과 대칭화)
> **기호:** $S_n=\sum_{i=1}^n X_i$ (hw1 #2에서는 $S_n=\sum(X_i-\mathbb E X_i){}$), $\bar X_n=S_n/n$. Chernoff 변수는 $\lambda$로 쓴다 (강의노트 Thm 1.6의 증명은 $t$로 씀). $\epsilon$ = 합의 편차, $t$ = 평균의 편차 ($\epsilon=nt$).
> **범례:** 📝 hw1에 나옴 · 🧩 강의노트에 증명 없음 → 보충 증명 · 📐 강의노트에 증명 있음 · 🔢 계산 · 💡 개념·해석 · (보충) 강의노트에 없는 내용
> **도구 번호** `[T1]`… 는 [00 문제풀이 도구상자](00%20문제풀이%20도구상자.md)의 도구 카드 번호다.

---

## 0. 한눈에

### 0.1 범위

| 쪽 | 절 | 내용 | 증명 | hw1 |
| --- | --- | --- | --- | --- |
| p1–4 | 1.1–1.2 | 왜 집중부등식인가: expected risk, test error와 generalization gap | – | – |
| p4 | 1.3 | Markov (Thm 1.1) | 📐 | #4(f) |
| p4–5 | 1.4 | Chebyshev (Thm 1.2), typical deviation | 📐 | – |
| p5 | 1.5 | 지수형 집중부등식 개요 | – | – |
| p6–7 | 1.5.1 | Chernoff (Thm 1.3), Mills (Thm 1.4), 표본평균에 적용 | 📐 | #4(f) |
| p7–8 | – | Hoeffding의 ML 해석 (학습 데이터에 조건부로 고정) | – | – |
| p8–9 | 1.6 | Hoeffding's lemma (Lemma 1.5), Hoeffding 부등식 (Thm 1.6), 베르누이 예제 | Lemma 🧩 · Thm 📐 | #1, #4 |
| p9–10 | – | 집중부등식 비교표 | – | #2 |
| p10 | 1.7 | Bernstein (Thm 1.7), Remark | 🧩 | #2 |
| p11–12 | 2 | ML 연결 (경로 A·B), Summary | – | – |

### 0.2 시험 포인트 (출제 가능성 순)

**★★★ 매우 유력**

1. 📝🧩 **Hoeffding's lemma 증명** — hw1 #1이 통째로 이것이다. 현(chord) 부등식 → $\mathbb E X=0$ → $p,h$로 재매개화 → $L(h)$ → $L''=u(1-u)\le\frac14$ → Taylor. → [5.1](#51-hoeffdings-lemma)
2. 📝📐 **Chernoff 방법으로 Hoeffding 부등식 유도** — Markov를 $e^{\lambda S}$에 → 독립 → lemma → $\lambda^\star$ 최적화. 강의노트 증명 + hw1 #4(f). → [5.2](#52-hoeffding의-부등식)
3. 📝 **sub-Gaussian과 대칭화** (숙제에서 도입한 개념) — 정의, 정규분포, Rademacher의 $\cosh s\le e^{s^2/2}$, 조건부 Jensen, $X-X'\overset{d}{=}\varepsilon(X-X')$, 표본평균의 parameter $(b-a)/\sqrt n$. → [6](#6-sub-gaussian과-대칭화)
4. 📝🧩 **Bernstein vs Hoeffding** — 분산을 최악값으로 두면 Hoeffding이 항상 강하다. 실제 분산이 작으면 Bernstein이 강하다. 정확한 조건은 $\sigma^2 < \frac{n(b-a)^2}{4}-\frac{(b-a)t}{3}$. 큰 $t$에서는 지수가 1차 대 2차. → [7.5](#75-bernstein-vs-hoeffding), [hw1 #2](#hw1-2-bernstein-vs-hoeffding)

**★★ 유력**

5. 📐 Markov·Chebyshev·Chernoff·Mills의 증명 (짧아서 한 문항의 소문항으로 나오기 좋다)
6. 🧩 Bernstein의 증명 (적률조건 → MGF 상계 → Chernoff), 그리고 hw1의 유계형($Mt/3$)과의 관계 → [7.2](#72-bernstein-부등식의-보충-증명)
7. 🔢 베르누이 예제 (Chebyshev 0.0625 vs Hoeffding 0.00067), 표본 크기 계산 $n\ge\log(2/\delta)/(2\epsilon^2)$
8. 💡 test error에는 Hoeffding을 바로 쓸 수 있고 training error에는 못 쓰는 이유, 유한 함수집합의 union bound (보충)

**★ 가능성 있음**

9. 💡 비교표 (가정과 꼬리 모양), Bernstein의 적률조건 ≈ sub-exponential 꼬리
10. 🔢 정규 표본평균에서 Mills vs Chebyshev

**비중 낮음** — 1.1–1.2절과 §2의 ML 서술 (VC dimension, Rademacher complexity, PAC-Bayes 등은 이름만 나온다)

### 0.3 이 장의 도구

| 도구 | 쓰는 곳 |
| --- | --- |
| [T1] Markov: $t\thinspace I(X\ge t)\le X$ | Chebyshev, Chernoff의 출발점 |
| [T2] Chernoff: 지수로 올려 Markov, $\lambda$ 최적화 | Hoeffding, sub-Gaussian 꼬리, Bernstein |
| [T3] 독립이면 MGF가 곱으로 | Hoeffding, 대칭화 (e) |
| [T4] 볼록성: 현 부등식, Jensen (조건부 포함) | Hoeffding's lemma (b), 대칭화 (c) |
| [T5] Taylor 나머지항 + 2계도함수 상계 | Hoeffding's lemma (f) |
| [T6] $u(1-u)\le\frac14$ | Hoeffding's lemma (e) |
| [T7] $\mathop{\mathrm{Var}}\nolimits Z=\min_c\mathbb E(Z-c)^2$ | hw1 #2(b) |
| [T8] 대칭화: 독립 복사본 + Rademacher | hw1 #4 |
| [T9] 급수 비교 $(2k)!\ge 2^k k!$ | Rademacher MGF |
| [T10] 합집합: 양측 = 2 × 단측 | Hoeffding 양측, 유한 함수집합 |

---

## 1. 왜 집중부등식인가

💡 [ch1 p1–4]

집중부등식은 **확률변수가 기댓값에서 크게 벗어날 확률을 정량적으로 누르는 도구**다.

- **기대위험(expected risk):** 임의의 예측함수 $f$에 대해 $R(f)=\mathbb E[L(f(X),Y)]$. 학습된 $\hat f$에 대한 $R(\hat f)$를 **일반화 오차(generalization error)**라 한다. $\hat f$가 training set에 따라 달라지므로 $R(\hat f)$는 확률변수일 수 있다.
- **훈련오차·테스트오차:**

$$\widehat R_{\text{train}}(\hat f)=\frac1n\sum_{i=1}^n L(\hat f(x_i),y_i),\qquad \widehat R_{\text{test}}(\hat f)=\frac1m\sum_{i=1}^m L(\hat f(x_{\text{test},i}),y_{\text{test},i})$$

- **두 질문은 수학적으로 다르다.**
  1. 독립인 test set: test error가 $R(\hat f)$를 얼마나 정확히 근사하나? → training data에 **조건부로** $\hat f$는 고정되고 test loss들은 독립이다. $0\le L\le 1$이면 Hoeffding으로

  $$\Pr\Big(\big\lvert\widehat R_{\text{test}}(\hat f)-R(\hat f)\big\rvert \gt  \epsilon\ \Big\vert\ D_{\text{train}}\Big)\le 2e^{-2m\epsilon^2}$$

  우변이 $D_{\text{train}}$에 의존하지 않으므로 **무조건부로도 같은 bound**가 성립한다 (∵ $\Pr(A)=\mathbb E[\Pr(A\mid D_{\text{train}})]\le 2e^{-2m\epsilon^2}$).
  2. training set: generalization gap $R(\hat f)-\widehat R_{\text{train}}(\hat f)$는 얼마나 큰가? → $\hat f$가 **바로 그 training data로 선택**되었으므로 $L(\hat f(x_i),y_i)$를 고정된 함수의 독립표본으로 볼 수 없다. Hoeffding을 그대로 쓰면 안 된다.
- **해결책 = uniform convergence:** 높은 확률로 $\sup_{f\in\mathcal F}\lvert R(f)-\widehat R_{\text{train}}(f)\rvert\le\epsilon_n$이면, $\hat f\in\mathcal F$가 데이터로 골라졌어도 자동으로 $\lvert R(\hat f)-\widehat R_{\text{train}}(\hat f)\rvert\le\epsilon_n$. 전형적인 결과는 $O_P\big(\sqrt{\text{complexity}(\mathcal F)/n}\big)$ 꼴이다. 유한 함수집합은 Hoeffding + union bound로 얻고, 무한 집합은 VC theory, symmetrization, Rademacher complexity로 확장한다.

(보충) **유한 함수집합의 uniform bound** — 변형 문제 [V7](#v7-유한-함수집합의-uniform-bound)에서 유도한다: $\lvert\mathcal F\rvert=K$, $0\le L\le1$이면

$$\Pr\Big(\max_{f\in\mathcal F}\big\lvert\widehat R_{\text{train}}(f)-R(f)\big\rvert \gt  \epsilon\Big)\le 2Ke^{-2n\epsilon^2}.$$

<details><summary>시험에 나온다면 — Q1. 왜 training error에는 Hoeffding을 바로 쓸 수 없나?</summary>

$\hat f$가 같은 training data에 의존하므로, $L(\hat f(x_i),y_i)$들은 "고정된 하나의 함수 $f$에 대한 독립표본"이 아니다. Hoeffding은 고정된 $f$에 대한 손실 $L(f(x_i),y_i)$들의 평균에만 쓸 수 있다. training data에 조건부로 보면 $\hat f$는 고정되지만, 그러면 훈련 손실도 모두 고정값이 되어 확률이 남지 않는다.

</details>
<details><summary>Q2. 조건부 bound가 무조건부로도 성립하는 이유는?</summary>

반복기댓값: $\Pr(A)=\mathbb E\big[\Pr(A\mid D_{\text{train}})\big]$. 조건부 확률이 모든 $D_{\text{train}}$에서 $2e^{-2m\epsilon^2}$ 이하이면 그 기댓값도 이하다.

</details>

---

## 2. Markov 부등식

📐 [ch1 p4 Thm 1.1] · 쓰이는 곳: Chebyshev, Chernoff, hw1 #4(f)

**가정** $X\ge 0$, $\mathbb E X < \infty$
**결론** 모든 $t > 0$에 대해

$$\Pr(X \gt  t)\le\frac{\mathbb E X}{t},\qquad\text{같은 말로}\quad \Pr\big(X \gt  t\thinspace\mathbb E X\big)\le\frac1t .$$

**핵심 한 수** $X\ge t\thinspace I(X\ge t)$ [T1]

**증명 (강의노트: 밀도가 있는 경우)** $X\ge0$이므로

$$\mathbb E X=\int_0^t xf(x)\thinspace dx+\int_t^\infty xf(x)\thinspace dx\ \ge\ \int_t^\infty xf(x)\thinspace dx\ \ge\ t\int_t^\infty f(x)\thinspace dx=t\Pr(X\ge t).$$

- 첫 부등호: $\int_0^t xf\thinspace dx\ge0$ (∵ $x\ge0$). 둘째 부등호: 적분 구간에서 $x\ge t$.
- 결론은 $\Pr(X\ge t)$로 나오는데 $\Pr(X > t)\le\Pr(X\ge t)$이므로 정리의 꼴도 성립한다.

**(보충) 일반 증명 — 이산·연속 가리지 않음.** $X\ge0$이면 항상 $t\thinspace I(X\ge t)\le X$ ($X\ge t$이면 좌변 $=t\le X$, 아니면 좌변 $=0\le X$). 양변에 기댓값을 취하면 $t\Pr(X\ge t)\le\mathbb E X$. ∎

**가정이 빠지면** $X\ge0$이 필요하다. 반례: $X=-100$ 또는 $10$ (각 확률 1/2)이면 $\mathbb E X=-45 < 0$인데 $\Pr(X > 1)=1/2$.

**(보충) 가장 많이 쓰는 따름정리.** $\varphi\ge0$이 증가함수이면 $\Pr(X\ge t)\le\mathbb E\varphi(X)/\varphi(t)$ ($\varphi(t) > 0$).
- $\varphi(x)=x^2$을 $\lvert X-\mu\rvert$에 → Chebyshev
- $\varphi(x)=e^{\lambda x}$ → Chernoff
- $\varphi(x)=\lvert x\rvert^k$ → $\Pr(\lvert X\rvert\ge t)\le\mathbb E\lvert X\rvert^k/t^k$

**해석** 평균이 1인 비음 확률변수가 10보다 클 확률은 최대 10%다. $X\in\lbrace0,t\rbrace$이면 등호가 되므로 ($\Pr(X=t)=\mathbb E X/t$) **평균만 알 때는 더 좋은 bound가 없다.**

**셀프 체크**
<details><summary>Q1. 일반 증명의 한 줄은?</summary>

$t\thinspace I(X\ge t)\le X$에 기댓값.

</details>
<details><summary>Q2. Markov에서 등호가 되는 분포는?</summary>

$X\in\lbrace0,t\rbrace$, $\Pr(X=t)=\mathbb E X/t$.

</details>

---

## 3. Chebyshev 부등식

📐 [ch1 p4–5 Thm 1.2] · 쓰이는 곳: ch3 WLLN (Thm 3.1), qm ⟹ p (Thm 3.2), 베르누이 예제

**가정** $\mathbb E X=\mu_X$, $\mathop{\mathrm{Var}}\nolimits X=\sigma_X^2 < \infty$
**결론** 모든 $t > 0$에 대해 $\Pr(\lvert X-\mu_X\rvert > t\sigma_X)\le\dfrac1{t^2}$. 같은 말로 $\Pr(\lvert X-\mu_X\rvert\ge\epsilon)\le\dfrac{\sigma_X^2}{\epsilon^2}$.

**핵심 한 수** $(X-\mu)^2$에 Markov

**증명**

$$\Pr(\lvert X-\mu_X\rvert\ge t\sigma_X)=\Pr\big((X-\mu_X)^2\ge t^2\sigma_X^2\big)\le\frac{\mathbb E(X-\mu_X)^2}{t^2\sigma_X^2}=\frac1{t^2}.$$

- 등호: $\lvert y\rvert\ge a\iff y^2\ge a^2$ ($a\ge 0$). 부등호: Markov (∵ $(X-\mu)^2\ge0$).

**해석** 편차는 **표준편차 정도의 크기(typical deviation)**를 갖고, 그 $t$배 이상 벗어날 확률은 $1/t^2$ 수준이다. 가정(적률)을 더하면 확률이 줄어드는 속도는 빨라지지만 **편차의 크기는 여전히 $\sigma_X$ 차수**다. Markov보다 가정이 하나 더 있다 (분산의 존재, 대신 비음 조건은 필요 없음).

**(보충) 등호 예.** $t\ge1$일 때 $X=\mu\pm t\sigma$ (각 확률 $\frac1{2t^2}$), $X=\mu$ (확률 $1-\frac1{t^2}$)이면 $\mathop{\mathrm{Var}}\nolimits X=\sigma^2$이고 $\Pr(\lvert X-\mu\rvert\ge t\sigma)=1/t^2$. **분산만 알 때는 더 좋은 bound가 없다.**

---

## 4. Chernoff 방법과 Mills 부등식

### 4.1 Chernoff bound

📐 [ch1 p6 Thm 1.3] · 쓰이는 곳: Hoeffding, sub-Gaussian 꼬리, Bernstein, hw1 #4(f)

**가정** 어떤 $\lambda > 0$에서 $M_X(\lambda)=\mathbb E e^{\lambda X} < \infty$
**결론** 모든 $t > 0$에 대해

$$\Pr(X\ge t)\le e^{-\lambda t}M_X(\lambda),\qquad \Pr(X\ge t)\le\inf_{\lambda \gt  0}\exp\lbrace-\lambda t+\log M_X(\lambda)\rbrace.$$

**핵심 한 수** 지수함수로 올린 뒤 Markov [T2]

**증명** $\lambda > 0$이면 $x\mapsto e^{\lambda x}$가 증가함수이므로 $X\ge t\iff\lambda X-\lambda t\ge0\iff e^{\lambda X-\lambda t}\ge1$. 따라서

$$\Pr(X\ge t)=\Pr\big(e^{\lambda X-\lambda t}\ge1\big)\le\mathbb E\thinspace e^{\lambda X-\lambda t}=e^{-\lambda t}M_X(\lambda)$$

(부등호: Markov, ∵ $e^{\lambda X-\lambda t}\ge0$). 모든 $\lambda > 0$에서 성립하므로 inf를 취한다. ∎

**왜 강력한가** Markov를 $X$가 아니라 $e^{\lambda X}$에 쓰면 polynomial tail 대신 **exponential tail**을 얻는다.

**(보충) 최적화 공식 — 이 장에서 계속 쓴다.** $c > 0$, $t > 0$이면

$$\min_{\lambda \gt  0}\Big(-\lambda t+\frac{c\lambda^2}{2}\Big)=-\frac{t^2}{2c}\quad(\lambda^\star=t/c).$$

그래서 $\mathbb E e^{\lambda X}\le e^{c\lambda^2/2}$ 꼴의 MGF 상계가 있으면 곧바로 $\Pr(X\ge t)\le e^{-t^2/(2c)}$.

**(보충) 예: 정규분포.** $X\sim N(0,\sigma^2)$이면 $\log M_X(\lambda)=\sigma^2\lambda^2/2$ → $\lambda^\star=t/\sigma^2$ → $\Pr(X\ge t)\le e^{-t^2/(2\sigma^2)}$.

**셀프 체크**
<details><summary>Q1. λ > 0이어야 하는 이유는?</summary>

$e^{\lambda x}$가 증가함수여야 $X\ge t\iff e^{\lambda X}\ge e^{\lambda t}$가 성립한다. $\lambda < 0$이면 부등호 방향이 뒤집혀 아래쪽 꼬리 $\Pr(X\le t)$의 bound가 된다.

</details>
<details><summary>Q2. −λt + cλ²/2 의 최솟값과 최소점은?</summary>

$\lambda^\star=t/c$에서 $-t^2/(2c)$.

</details>

### 4.2 Mills 부등식

📐 [ch1 p6–7 Thm 1.4]

**결론** $Z\sim N(0,1)$이면 $\displaystyle\Pr(\lvert Z\rvert > t)\le\sqrt{\frac2\pi}\thinspace\frac{e^{-t^2/2}}{t}$ ($t > 0$).

**핵심 한 수** 적분 구간 $[t,\infty)$에서 $s/t\ge1$을 곱해 적분 가능한 꼴 $se^{-s^2/2}$로 만든다.

**증명**

$$\begin{aligned}
\Pr(\lvert Z\rvert\ge t)&=\frac{2}{\sqrt{2\pi}}\int_t^\infty e^{-s^2/2}\thinspace ds &&(\because\ \text{대칭})\cr
&\le\sqrt{\frac2\pi}\thinspace\frac1t\int_t^\infty s\thinspace e^{-s^2/2}\thinspace ds &&(\because\ s\ge t\ \text{이면}\ 1\le s/t)\cr
&=\sqrt{\frac2\pi}\thinspace\frac1t\int_{t^2/2}^\infty e^{-u}\thinspace du &&(u=s^2/2)\cr
&=\sqrt{\frac2\pi}\thinspace\frac{e^{-t^2/2}}{t}.
\end{aligned}$$

**표본평균에 적용** $X_i\overset{\text{iid}}{\sim}N(\mu,\sigma^2)$이면 $Z=\frac{\bar X-\mu}{\sigma/\sqrt n}\sim N(0,1)$이므로

$$\Pr\Big(\lvert\bar X-\mu\rvert \gt  \frac{t\sigma}{\sqrt n}\Big)\le\sqrt{\frac2\pi}\frac{e^{-t^2/2}}{t}\qquad\text{vs. Chebyshev}\quad\le\frac1{t^2}.$$

🔢 (보충, 계산으로 확인) $\Pr(\lvert Z\rvert > t)$ 비교:

| $t$ | Mills | 실제 값 | Chebyshev $1/t^2$ |
| ---: | ---: | ---: | ---: |
| 1 | 0.484 | 0.317 | 1 |
| 2 | 0.0540 | 0.0455 | 0.25 |
| 3 | 0.00295 | 0.00270 | 0.111 |

$t$가 클수록 Mills가 실제 값에 붙는다. 편차가 $t\sigma/\sqrt n$으로 같은 크기라도 확률이 $e^{-t^2/2}$ 속도로 준다.

---

## 5. Hoeffding의 부등식

Mills는 정규분포 전용이다. Hoeffding은 **유계(bounded)** 확률변수라면 분포와 상관없이 성립하는 지수형 부등식이다. [ch1 p8]

### 5.1 Hoeffding's lemma

🧩📝 [ch1 p8 Lemma 1.5] — 강의노트에는 증명이 없고, **hw1 #1이 이 증명이다.** 아래가 모범 답안 전문이다.

**가정** $a\le X\le b$ (a.s.), $\mathbb E X=0$
**결론** 모든 $\lambda\in\mathbb R$에 대해

$$\mathbb E e^{\lambda X}\le\exp\Big(\frac{\lambda^2(b-a)^2}{8}\Big).$$

**뜻** 유계이고 평균이 0인 확률변수는 $\frac{b-a}{2}$-sub-Gaussian이다 ([6.1](#61-정의와-해석)).

**핵심 한 수** $e^{\lambda x}$를 **현(chord)으로 위에서 누르고**, 그 결과를 $L(h)=-ph+\log(1-p+pe^h)$로 쓴 뒤 **2계도함수를 1/4로 눌러 Taylor**.

**증명 (hw1 #1의 (a)–(f) 순서)**

**(a) 자명한 경우와 부호.** $a=b$이면 $X=a$ a.s.이고 $\mathbb E X=a=0$이므로 $X=0$ a.s., $\mathbb E e^{\lambda X}=1=\exp(0)$으로 성립한다. 이제 $a < b$라 하자. 만약 $a > 0$이면 $X\ge a > 0$ a.s.라서 $\mathbb E X\ge a > 0$, 모순. 만약 $b < 0$이면 $\mathbb E X\le b < 0$, 모순. 따라서 $a\le0\le b$. (∵ $\mathbb E X=0$)

**(b) 현 부등식.** $x\in[a,b]$를 $a,b$의 볼록결합으로 쓰면

$$x=\underbrace{\frac{b-x}{b-a}}_ {=:\thinspace\theta}\thinspace a+\underbrace{\frac{x-a}{b-a}}_ {=\thinspace1-\theta}\thinspace b,\qquad\theta\in[0,1].$$

$g(x)=e^{\lambda x}$는 $g''(x)=\lambda^2e^{\lambda x}\ge0$이라 볼록이므로 $g(\theta a+(1-\theta)b)\le\theta g(a)+(1-\theta)g(b)$, 즉

$$e^{\lambda x}\le\frac{b-x}{b-a}e^{\lambda a}+\frac{x-a}{b-a}e^{\lambda b}\qquad(x\in[a,b]).$$

**(c) 기댓값.** $a\le X\le b$ a.s.이므로 $x=X$를 대입할 수 있다. 기댓값의 선형성과 단조성으로

$$\mathbb E e^{\lambda X}\le\frac{b-\mathbb E X}{b-a}e^{\lambda a}+\frac{\mathbb E X-a}{b-a}e^{\lambda b}=\frac{b}{b-a}e^{\lambda a}-\frac{a}{b-a}e^{\lambda b}.$$

마지막 등호에서 **$\mathbb E X=0$을 쓴다.**

**(d) 재매개화.** $p=\frac{-a}{b-a}$, $h=\lambda(b-a)$로 둔다. $b-a > 0$, $-a\ge0$이므로 $p\ge0$이고, $1-p=\frac{b}{b-a}\ge0$이므로 $p\le1$. 또 $\lambda a=-\lambda(b-a)p=-ph$, $\lambda b=\lambda a+\lambda(b-a)=-ph+h$. 따라서 (c)의 우변은

$$(1-p)e^{-ph}+p\thinspace e^{-ph+h}=e^{-ph}\big(1-p+pe^h\big)=\exp\big(L(h)\big),\qquad L(h):=-ph+\log(1-p+pe^h).$$

($1-p+pe^h > 0$이라 log가 정의된다.)

**(e) $L$의 도함수.** $L(0)=0+\log1=0$.

$$L'(h)=-p+\frac{pe^h}{1-p+pe^h}\ \Rightarrow\ L'(0)=-p+p=0,$$

$$L''(h)=\frac{pe^h(1-p+pe^h)-pe^h\cdot pe^h}{(1-p+pe^h)^2}=\frac{pe^h}{1-p+pe^h}\cdot\frac{1-p}{1-p+pe^h}=u(1-u),\qquad u=\frac{pe^h}{1-p+pe^h}.$$

$p\in[0,1]$이라 분자·분모가 모두 비음이고 분자 ≤ 분모이므로 $u\in[0,1]$. $u(1-u)=\frac14-(u-\frac12)^2\le\frac14$. [T6]

**(f) Taylor.** 나머지항이 있는 Taylor 정리: $0$과 $h$ 사이의 어떤 $\xi$에 대해

$$L(h)=L(0)+L'(0)h+\frac{L''(\xi)}{2}h^2=\frac{L''(\xi)}{2}h^2\le\frac{h^2}{8}.$$

따라서 $\mathbb E e^{\lambda X}\le e^{L(h)}\le e^{h^2/8}=\exp\big(\lambda^2(b-a)^2/8\big)$. ∎

**가정이 빠지면**
- $\mathbb E X=0$이 없으면 틀린다: $X\equiv c\ne0$ ($a=b=c$)이면 $\mathbb E e^{\lambda X}=e^{\lambda c} > 1=\exp(0)$ ($\lambda c > 0$). 평균이 0이 아니면 $X-\mathbb E X$ (범위의 길이는 그대로 $b-a$)에 적용한다: $\mathbb E e^{\lambda(X-\mathbb E X)}\le e^{\lambda^2(b-a)^2/8}$.
- 상수 1/8은 더 줄일 수 없다: $X=\pm1$ (각 1/2)이면 $\mathbb E e^{\lambda X}=\cosh\lambda=1+\frac{\lambda^2}{2}+\cdots$이고 bound는 $e^{\lambda^2/2}=1+\frac{\lambda^2}{2}+\cdots$ (2차항까지 같다).

**자주 하는 실수**
- (b)에서 가중치를 명시하지 않음. (c)에서 **어디서 $\mathbb E X=0$을 쓰는지** 안 밝힘 (hw가 콕 집어 물었다).
- (d)에서 " $-a > 0$"이라고 씀 → $a=0$일 수 있으니 $-a\ge0$.
- (f)에서 나머지항의 꼴을 안 씀 (" $\xi$는 0과 $h$ 사이").

**셀프 체크**
<details><summary>Q1. p와 h의 정의, 그리고 λa를 p, h로 쓰면?</summary>

$p=-a/(b-a)$, $h=\lambda(b-a)$, $\lambda a=-ph$.

</details>
<details><summary>Q2. L″(h)는? 왜 1/4 이하인가?</summary>

$L''(h)=u(1-u)$, $u=pe^h/(1-p+pe^h)\in[0,1]$. $u(1-u)\le1/4$ (산술–기하, 또는 $\frac14-(u-\frac12)^2$).

</details>
<details><summary>Q3. EX = 0은 증명의 어디에서 쓰이나?</summary>

(c)의 마지막 등호 (기댓값을 취한 뒤 $\mathbb E X$ 자리에 0). 간접적으로는 (a)의 $a\le0\le b$ → (d)의 $p\in[0,1]$에도 쓰인다.

</details>

### 5.2 Hoeffding의 부등식

📐 [ch1 p8–9 Thm 1.6] · hw1 #4(f)에서 같은 틀을 다시 씀

**가정** $X_1,\dots,X_n$ 독립, $a_i\le X_i\le b_i$, $\mathbb E X_i=0$
**결론** 모든 $t > 0$에 대해

$$\Pr\Big(\frac1n\sum_{i=1}^nX_i\ge t\Big)\le\exp\Big(-\frac{2n^2t^2}{\sum_{i=1}^n(b_i-a_i)^2}\Big),\qquad\text{합의 꼴}:\ \Pr\Big(\sum_{i=1}^nX_i\ge\epsilon\Big)\le\exp\Big(-\frac{2\epsilon^2}{\sum_{i=1}^n(b_i-a_i)^2}\Big).$$

**핵심 한 수** Chernoff → 독립으로 곱 → lemma → $\lambda$ 최적화 [T2][T3]

**증명** ($\epsilon=nt$로 두면 두 꼴이 같다. 강의노트는 Chernoff 변수를 $t$로 썼는데 여기서는 $\lambda$로 쓴다.)

1. $\lambda > 0$에 대해 $\Pr\big(\sum X_i\ge\epsilon\big)\le e^{-\lambda\epsilon}\thinspace\mathbb E\thinspace e^{\lambda\sum X_i}$ [Chernoff]
2. $\mathbb E\thinspace e^{\lambda\sum X_i}=\prod_{i=1}^n\mathbb E\thinspace e^{\lambda X_i}$ (∵ 독립)
3. $\mathbb E\thinspace e^{\lambda X_i}\le\exp\big(\lambda^2(b_i-a_i)^2/8\big)$ (∵ Lemma 1.5: $a_i\le X_i\le b_i$, $\mathbb E X_i=0$). 따라서

$$\Pr\Big(\sum X_i\ge\epsilon\Big)\le\exp\Big(\phi(\lambda)\Big),\qquad\phi(\lambda)=-\lambda\epsilon+\frac{\lambda^2}{8}\sum_{i=1}^n(b_i-a_i)^2.$$

4. $\phi'(\lambda)=-\epsilon+\frac{\lambda}{4}\sum(b_i-a_i)^2=0$ → $\lambda^\star=\dfrac{4\epsilon}{\sum(b_i-a_i)^2} > 0$ → $\phi(\lambda^\star)=-\dfrac{2\epsilon^2}{\sum(b_i-a_i)^2}$. ∎

   (4.1의 최적화 공식에서 $c=\frac14\sum(b_i-a_i)^2$인 경우다.)

**따름 꼴들**
- **양측:** $-X_i$도 가정을 만족하므로 같은 bound. 합집합으로 $\Pr\big(\lvert\sum X_i\rvert\ge\epsilon\big)\le2\exp\big(-2\epsilon^2/\sum(b_i-a_i)^2\big)$. [T10]
- **평균이 0이 아니면:** $X_i-\mathbb E X_i\in[a_i-\mathbb E X_i,\ b_i-\mathbb E X_i]$ — 범위 길이가 그대로 $b_i-a_i$라서 같은 bound가 $\sum(X_i-\mathbb E X_i)$에 성립한다.
- **편차 크기로 다시 쓰기** [p9]:

$$\Pr\Big(\Big\lvert\frac1n\sum X_i\Big\rvert \gt  t\thinspace\frac{\sqrt{\sum(b_i-a_i)^2}}{n}\Big)\le 2e^{-2t^2}.$$

  편차의 크기는 여전히 표준편차 수준 (같은 범위면 $(b-a)/\sqrt n$)이지만, 확률이 $1/t^2$ 대신 $e^{-2t^2}$로 **지수적으로** 준다.
- **베르누이:** $X_i\sim\text{Bernoulli}(p)$이면 $b_i-a_i=1$이므로 $\Pr(\lvert\bar X_n-p\rvert\ge\epsilon)\le2e^{-2n\epsilon^2}$.

🔢 **Example [p9]** $n=100$, $\epsilon=0.2$:
- Chebyshev: $\Pr(\lvert\bar X_n-p\rvert\ge\epsilon)\le\dfrac{p(1-p)}{n\epsilon^2}\le\dfrac{0.5\cdot0.5}{100\cdot0.2^2}=0.0625$ (∵ $p(1-p)\le1/4$)
- Hoeffding: $\le2e^{-2\cdot100\cdot0.2^2}=2e^{-8}=0.00067$

🔢 (보충) **표본 크기.** $\Pr(\lvert\bar X_n-p\rvert\ge\epsilon)\le\delta$가 되려면 $2e^{-2n\epsilon^2}\le\delta\iff n\ge\dfrac{\log(2/\delta)}{2\epsilon^2}$. 같은 말로, 확률 $1-\delta$ 이상으로 $\lvert\bar X_n-p\rvert < \sqrt{\log(2/\delta)/(2n)}$. ($\epsilon=\delta=0.05$이면 Hoeffding은 $n\ge738$, Chebyshev는 $n\ge2000$ → [V2](#v2-표본-크기-chebyshev-vs-hoeffding))

**가정이 빠지면**
- 독립이 없으면 2단계가 안 된다. 반례: $X_1=\cdots=X_n=X$ ($\pm1$, 각 1/2)이면 $\bar X_n=X$라서 $\Pr(\bar X_n\ge1)=1/2$로 $n$과 무관하다. 하지만 bound는 $e^{-n/2}\to0$.
- 유계가 없으면 lemma를 못 쓴다 (MGF가 없을 수도 있다) → Bernstein이나 sub-Gaussian 조건으로 바꾼다.

**자주 하는 실수**
- 합과 평균의 스케일: 평균의 편차 $t$ ↔ 합의 편차 $\epsilon=nt$. 지수의 $n^2t^2$과 $\epsilon^2$을 섞지 않는다.
- 분모는 $\sum(b_i-a_i)^2$ (제곱의 합). $\big(\sum(b_i-a_i)\big)^2$가 아니다.
- 양측이면 앞에 2. $\lambda^\star > 0$을 확인한다 ($\epsilon > 0$일 때만).

**셀프 체크**
<details><summary>Q1. λ* 와 최적 지수는?</summary>

$\lambda^\star=4\epsilon/\sum(b_i-a_i)^2$, 지수 $-2\epsilon^2/\sum(b_i-a_i)^2$.

</details>
<details><summary>Q2. 증명의 네 단계에서 쓰는 가정을 순서대로?</summary>

① Chernoff는 가정 없음 ($\lambda > 0$만) ② 독립 ③ 유계 + 평균 0 (lemma) ④ $\epsilon > 0$ ($\lambda^\star > 0$).

</details>

### 5.3 집중부등식 비교

💡 [ch1 p9–10]

| 부등식 | 주요 가정 | 꼬리 모양 (schematic) | 한 줄 요약 |
| --- | --- | --- | --- |
| Markov | $X\ge0$, $\mathbb E X < \infty$ | $1/t$ | 평균만 사용 |
| Chebyshev | $\mathop{\mathrm{Var}}\nolimits X < \infty$ | $1/t^2$ | 분산 사용 |
| Hoeffding | 독립, 유계 | $\exp(-ct^2)$ | 범위만 사용 (sub-Gaussian) |
| Bernstein | 독립, sub-exponential 적률조건 | $\exp(-c\min\lbrace t^2,t\rbrace)$ | 분산 + 꼬리 |

Markov·Chebyshev는 매우 일반적이지만 polynomial tail이다. Hoeffding·Bernstein은 가정이 강한 대신 exponential tail을 준다.

---

## 6. sub-Gaussian과 대칭화

📝 [hw1 #4] — 강의노트에는 없고 **숙제에서 도입한 개념**이다. 아래 6.3이 hw1 #4의 모범 답안 전문이다.

### 6.1 정의와 해석

**정의** 평균이 $\mu$인 $X$가 모든 $s\in\mathbb R$에 대해

$$\mathbb E\exp\lbrace s(X-\mu)\rbrace\le\exp\Big(\frac{\sigma^2s^2}{2}\Big)$$

를 만족하면 $X$를 **$\sigma$-sub-Gaussian**이라 한다.

- **자기 말로 (hw1 #4(a)):** 중심화된 MGF가 $N(0,\sigma^2)$의 MGF보다 작다. 그래서 꼬리가 분산 $\sigma^2$인 정규분포만큼 **빨리** 준다: Chernoff와 4.1의 최적화 공식으로 $\Pr(X-\mu\ge t)\le e^{-t^2/(2\sigma^2)}$.
- **정규분포:** $X\sim N(\mu,\sigma^2)$이면 $\mathbb E e^{sX}=e^{s\mu+\sigma^2s^2/2}$이므로 $\mathbb E e^{s(X-\mu)}=e^{\sigma^2s^2/2}$ — 등호로 성립, 즉 $\sigma$-sub-Gaussian.
- **유계:** Lemma 1.5는 " $[a,b]$ 값, 평균 0이면 $\frac{b-a}2$-sub-Gaussian"이라는 말이다.

**(보충) 성질** — 변형 문제 [V4](#v4-sub-gaussian의-성질)에서 증명한다.
1. 독립인 $X_i$가 각각 $\sigma_i$-sub-Gaussian이면 $\sum X_i$는 $\sqrt{\sum\sigma_i^2}$-sub-Gaussian.
2. $cX$는 $\lvert c\rvert\sigma$-sub-Gaussian.
3. $\mathop{\mathrm{Var}}\nolimits X\le\sigma^2$.

### 6.2 Rademacher 변수

$\varepsilon$: $\Pr(\varepsilon=1)=\Pr(\varepsilon=-1)=\frac12$. **(hw1 #4(b))**

$$\mathbb E e^{s\varepsilon}=\frac{e^s+e^{-s}}2=\sum_{k=0}^\infty\frac{s^{2k}}{(2k)!}\le\sum_{k=0}^\infty\frac{(s^2/2)^k}{k!}=e^{s^2/2}.$$

- 등호: 두 급수 $e^{\pm s}=\sum\frac{(\pm s)^k}{k!}$를 더하면 홀수 차수가 소거된다.
- 부등호: $(2k)!\ge2^kk!$ [T9]. 증명: $(2k)!=\prod_{j=1}^k(2j)(2j-1)\ge\prod_{j=1}^k 2j=2^kk!$. 그리고 항마다 $s^{2k}\ge0$.

### 6.3 대칭화로 Hoeffding(약한 판) 증명

**목표 (hw1 #4)** $X_1,\dots,X_n$ iid, $\mathbb E X_i=\mu$, $a\le X_i\le b$ a.s.이면

$$\Pr\Big(\Big\lvert\frac1n\sum X_i-\mu\Big\rvert\ge t\Big)\le2\exp\Big(-\frac{nt^2}{2(b-a)^2}\Big),\qquad t \gt  0.$$

**핵심 한 수** 독립 복사본 $X'$를 빼서 **대칭**으로 만들면, 대칭 변수는 Rademacher 부호를 곱해도 분포가 같다 → Rademacher의 MGF bound를 조건부로.

**(c) Jensen으로 복사본 끼워 넣기.** $\mathbb E X=0$, $a\le X\le b$인 $X$와 그 독립 복사본 $X'$ (같은 분포, $X$와 독립)를 생각한다. $X'$가 $X$와 독립이고 $\mathbb E X'=0$이므로 $\mathbb E[s(X-X')\mid X]=sX$. 볼록함수 $\varphi(u)=e^u$에 **조건부 Jensen**을 쓰면

$$e^{sX}=e^{\mathbb E[s(X-X')\mid X]}\le\mathbb E\big[e^{s(X-X')}\thinspace\big\vert\thinspace X\big].$$

양변에 $\mathbb E_X$를 취하면 $\mathbb E_Xe^{sX}\le\mathbb E_{X,X'}e^{s(X-X')}$. (볼록성을 쓴 함수: $u\mapsto e^u$)

**(d) 대칭성과 Rademacher.** $\varepsilon$을 $(X,X')$와 독립인 Rademacher라 하자.
- $X,X'$가 iid이므로 $(X,X')\overset{d}{=}(X',X)$, 따라서 $X-X'\overset{d}{=}X'-X=-(X-X')$.
- 그러면 모든 $u$에 대해 (∵ $\varepsilon$ 독립)

$$\Pr(\varepsilon(X-X')\le u)=\tfrac12\Pr(X-X'\le u)+\tfrac12\Pr(-(X-X')\le u)=\Pr(X-X'\le u).$$

  즉 $X-X'\overset{d}{=}\varepsilon(X-X')$.
- $(X,X')$에 조건부로 6.2를 $s$ 대신 $s(X-X')$에 쓰면

$$\mathbb E e^{s(X-X')}=\mathbb E e^{s\varepsilon(X-X')}=\mathbb E\Big[\mathbb E\big[e^{s(X-X')\varepsilon}\thinspace\big\vert\thinspace X,X'\big]\Big]\le\mathbb E\exp\Big(\frac{s^2(X-X')^2}{2}\Big)\le\exp\Big(\frac{s^2(b-a)^2}{2}\Big).$$

  마지막 부등호: $X,X'\in[a,b]$이므로 $\lvert X-X'\rvert\le b-a$.
- (c)와 합치면 $\mathbb E e^{sX}\le e^{s^2(b-a)^2/2}$: **유계이고 평균 0인 $X$는 $(b-a)$-sub-Gaussian.**

**(e) 표본평균.** $X_i-\mu$는 평균 0이고 길이 $b-a$인 구간 $[a-\mu,b-\mu]$에 있으므로 (d)가 적용된다. 독립이므로

$$\mathbb E e^{s(\bar X_n-\mu)}=\mathbb E e^{\frac sn\sum(X_i-\mu)}=\prod_{i=1}^n\mathbb E e^{\frac sn(X_i-\mu)}\le\prod_{i=1}^n\exp\Big(\frac{s^2(b-a)^2}{2n^2}\Big)=\exp\Big(\frac{s^2(b-a)^2}{2n}\Big).$$

즉 $\bar X_n$은 **$\frac{b-a}{\sqrt n}$-sub-Gaussian**.

**(f) Chernoff + 합집합.** $s > 0$이면 [Markov]

$$\Pr(\bar X_n-\mu\ge t)=\Pr\big(e^{s(\bar X_n-\mu)}\ge e^{st}\big)\le e^{-st}\thinspace\mathbb E e^{s(\bar X_n-\mu)}\le\exp\Big(-st+\frac{s^2(b-a)^2}{2n}\Big).$$

우변 지수는 $\frac{(b-a)^2}{2n}\big(s-\frac{nt}{(b-a)^2}\big)^2-\frac{nt^2}{2(b-a)^2}$ 이므로 $s^\star=\frac{nt}{(b-a)^2} > 0$에서 최소, 값은 $\exp\big(-\frac{nt^2}{2(b-a)^2}\big)$. $-(X_i-\mu)$도 같은 조건 (평균 0, 범위 길이 $b-a$)이므로 $\Pr(\bar X_n-\mu\le-t)$도 같은 bound. 합집합으로 두 꼬리를 더하면 목표식. ∎

**왜 "약한 판"인가** 대칭화는 parameter $(b-a)$를 주고, Lemma 1.5는 $\frac{b-a}2$를 준다. 지수에서 $\frac{nt^2}{2(b-a)^2}$ vs $\frac{2nt^2}{(b-a)^2}$ — **4배** 차이. 대신 대칭화는 Taylor 계산 없이 끝나고, 나중에 Rademacher complexity (ML의 uniform bound)로 이어지는 방법이다.

**셀프 체크**
<details><summary>Q1. (c)에서 E[s(X−X′) | X] = sX 가 되려면 무엇이 필요한가?</summary>

$X'$가 $X$와 독립이고 $\mathbb E X'=0$.

</details>
<details><summary>Q2. X − X′ 와 ε(X − X′) 가 같은 분포인 이유 두 가지는?</summary>

① $X,X'$ iid라서 $X-X'$가 0에 대해 대칭 ② $\varepsilon$이 $(X,X')$와 독립.

</details>
<details><summary>Q3. 표본평균의 sub-Gaussian parameter와 최적 s는?</summary>

$(b-a)/\sqrt n$, $s^\star=nt/(b-a)^2$.

</details>

---

## 7. Bernstein의 부등식

### 7.1 정리와 해석

🧩 [ch1 p10 Thm 1.7]

**가정** $X_1,\dots,X_n$ 독립, 평균 0, 모든 $k\ge2$에 대해

$$\mathbb E\lvert X_i\rvert^k\le\frac{k!}{2}M^{k-2}v_i .$$

**결론** ✏️ $\bar v=(v_1+\cdots+v_n)/n$ 에 대해

$$\Pr\Big(\frac1n\sum_{i=1}^nX_i\ge t\Big)\le\exp\Big(-\frac{nt^2}{2(\bar v+Mt)}\Big),\qquad\text{합의 꼴}:\ \Pr\Big(\sum X_i\ge\epsilon\Big)\le\exp\Big(-\frac{\epsilon^2}{2(V+M\epsilon)}\Big),\ V=\sum v_i .$$

**Remark [p10]**
- 지수형 bound에는 Chernoff 방법, 즉 MGF의 존재가 필요하다. Hoeffding은 유계라서 MGF가 바로 존재하고, Bernstein은 **모든 적률의 조건**으로 MGF를 확보한다. 이 조건은 $\Pr(\lvert X\rvert\ge t)\le c_1e^{-c_2t}$ 같은 **sub-exponential 꼬리**와 거의 동등하다.
- $v_i$가 **분산 정보**를 담는다 ($k=2$이면 $\mathbb E X_i^2\le v_i$). 범위만 쓰는 Hoeffding보다 정보가 많으므로, **범위는 크지만 분산이 아주 작을 때** Bernstein이 유리하다.
- ⚠ 분산 정보가 없어서 분산을 최악값으로 두면 오히려 Hoeffding보다 약하다 → [7.5](#75-bernstein-vs-hoeffding), hw1 #2(d).

### 7.2 Bernstein 부등식의 보충 증명

🧩 (보충 증명) 강의노트에 증명이 없다. hw1 #1처럼 소문항으로 쪼갰다.

**(a) MGF 상계.** $0 < \lambda < 1/M$이면 $\mathbb E e^{\lambda X_i}\le1+\dfrac{\lambda^2v_i}{2(1-\lambda M)}$.

$$\mathbb E e^{\lambda X_i}=1+\lambda\thinspace\mathbb E X_i+\sum_{k\ge2}\frac{\lambda^k\mathbb E X_i^k}{k!}\le1+\sum_{k\ge2}\frac{\lambda^k\mathbb E\lvert X_i\rvert^k}{k!}\le1+\frac{\lambda^2v_i}{2}\sum_{k\ge2}(\lambda M)^{k-2}=1+\frac{\lambda^2v_i}{2(1-\lambda M)}.$$

(∵ $\mathbb E X_i=0$, $X^k\le\lvert X\rvert^k$, 적률조건, 등비급수 $\lambda M < 1$. 기댓값과 급수의 교환은 우변 급수가 수렴하므로 정당하다.)

**(b) 지수로.** $1+x\le e^x$이므로 $\mathbb E e^{\lambda X_i}\le\exp\Big(\dfrac{\lambda^2v_i}{2(1-\lambda M)}\Big)$.

**(c) Chernoff + 독립.** $\Pr\big(\sum X_i\ge\epsilon\big)\le e^{-\lambda\epsilon}\prod\mathbb E e^{\lambda X_i}\le\exp\Big(-\lambda\epsilon+\dfrac{\lambda^2V}{2(1-\lambda M)}\Big)$.

**(d) $\lambda$ 고르기.** $\lambda=\dfrac{\epsilon}{V+M\epsilon}\in(0,1/M)$로 두면 $1-\lambda M=\dfrac{V}{V+M\epsilon}$이고

$$-\lambda\epsilon+\frac{\lambda^2V}{2(1-\lambda M)}=-\frac{\epsilon^2}{V+M\epsilon}+\frac{\epsilon^2}{2(V+M\epsilon)}=-\frac{\epsilon^2}{2(V+M\epsilon)}.$$

$\epsilon=nt$, $V=n\bar v$를 넣으면 $\exp\big(-\frac{nt^2}{2(\bar v+Mt)}\big)$. ∎

### 7.3 유계형 Bernstein

hw1 #2가 쓰는 꼴: 독립, 평균 0, $\lvert Y_i\rvert\le M$ a.s., $\sigma^2=\sum\mathop{\mathrm{Var}}\nolimits(Y_i)$이면

$$\Pr\Big(\sum_{i=1}^nY_i\ge t\Big)\le\exp\Big(-\frac{t^2}{2(\sigma^2+Mt/3)}\Big).$$

**(보충) 강의노트 Thm 1.7과의 관계 — $M$ 대신 $M/3$을 넣은 것이다.** $\lvert Y\rvert\le M$이면 $k\ge2$에서

$$\mathbb E\lvert Y\rvert^k\le M^{k-2}\thinspace\mathbb E Y^2\le\frac{k!}{2}\Big(\frac M3\Big)^{k-2}\mathop{\mathrm{Var}}\nolimits(Y)$$

(∵ $2\cdot3^{k-2}\le k!$: $k=2,3$에서 등호, $k\ge4$는 귀납법으로 $k!=k\thinspace(k-1)!\ge3\cdot2\cdot3^{k-3}$). 그래서 적률조건이 $(M/3,\ v_i=\mathop{\mathrm{Var}}\nolimits Y_i)$로 성립하고, 7.2에 넣으면 위 식이 나온다.

### 7.4 두 영역

합의 꼴 지수 $\dfrac{\epsilon^2}{2(\sigma^2+M\epsilon/3)}$ (유계형 기준):
- $\epsilon\ll\sigma^2/M$: 분모 ≈ $2\sigma^2$ → $\exp\big(-\frac{\epsilon^2}{2\sigma^2}\big)$. **실제 분산**을 쓰는 정규 꼬리.
- $\epsilon\gg\sigma^2/M$: 분모 ≈ $\frac23M\epsilon$ → $\exp\big(-\frac{3\epsilon}{2M}\big)$. 지수 꼬리 ($\epsilon$에 1차).
- 그래서 비교표의 꼴이 $\exp(-c\min\lbrace t^2,t\rbrace)$다.

### 7.5 Bernstein vs Hoeffding

📝 (= hw1 #2 (d)–(f)의 핵심. 문항별 답안은 [hw1 #2](#hw1-2-bernstein-vs-hoeffding))

설정: $X_i$ 독립, $a\le X_i\le b$, $S_n=\sum(X_i-\mathbb E X_i)$, $\sigma^2=\sum\mathop{\mathrm{Var}}\nolimits X_i$, $t > 0$.

| | 지수 (bound = $e^{-\text{지수}}$) | 쓰는 정보 |
| --- | --- | --- |
| Hoeffding | $E_H=\dfrac{2t^2}{n(b-a)^2}=\dfrac{t^2}{\frac12n(b-a)^2}$ | 범위 |
| Bernstein ($M=b-a$) | $E_B=\dfrac{t^2}{2\sigma^2+\frac23(b-a)t}$ | 분산 + 범위 |

1. **분산을 최악값으로 두면 Hoeffding이 항상 강하다.** $\sigma^2\le n(b-a)^2/4$를 넣으면 $E_B$의 분모가 $\frac12n(b-a)^2+\frac23(b-a)t > \frac12n(b-a)^2$ (∵ $t > 0$).
2. **정확한 조건.** $E_B > E_H\iff 2\sigma^2+\frac23(b-a)t < \frac12n(b-a)^2\iff\sigma^2 < \dfrac{n(b-a)^2}{4}-\dfrac{(b-a)t}{3}$. 우변이 양수여야 하므로 $t < \frac34n(b-a)$일 때만 가능하다.
3. **작은 $t$** ($t\ll\sigma^2/(b-a){}$): $E_B\approx t^2/(2\sigma^2)$. $E_B/E_H\approx\dfrac{n(b-a)^2}{4\sigma^2}$ — 분산이 최악값보다 작을수록 Bernstein이 **지수적으로** 강하다. Hoeffding은 범위만 보고 항마다 분산을 최악값 $(b-a)^2/4$로 취급하기 때문이다.
4. **큰 $t$** ($t\gg\sigma^2/(b-a){}$): $E_B\approx\frac{3t}{2(b-a)}$ ($t$에 1차), $E_H$는 $t$에 2차. $E_H > E_B\iff t > \frac34n(b-a)$. $S_n\le n(b-a)$이므로 실제로는 $t\in(\frac34n(b-a),\ n(b-a)]$ — **최대 가능 편차에 가까운 큰 $t$에서는 Hoeffding이 유리**하다.

🔢 (보충, 계산으로 확인) $X_i\sim\text{Bernoulli}(0.01)$, $n=1000$, $t=20$ (평균 10에서 20 이상 벗어날 확률): $\sigma^2=9.9$.
- Hoeffding: $e^{-2\cdot400/1000}=0.449$
- Bernstein: $\exp\big(-\frac{400}{19.8+13.3}\big)=5.7\times10^{-6}$
- Bernstein에 최악 분산 $n/4$을 넣으면: $0.459$ (Hoeffding보다 약간 나쁨)
- 실제 확률 $\Pr(\text{Bin}(1000,0.01)\ge30)=2.1\times10^{-7}$

---

## 8. 머신러닝 연결과 Summary

💡 [ch1 p11–12]

- **경로 A (모델 평가):** $D_{\text{train}}\to\hat f$, 독립인 $D_{\text{test}}$로 $\widehat R_{\text{test}}(\hat f)$ → concentration → $R(\hat f)$. 조건부로 $\hat f$가 고정되므로 Hoeffding을 **직접** 쓴다.
- **경로 B (학습의 일반화):** 같은 데이터가 $\hat f$ 선택과 training error 계산에 모두 쓰인다. Concentration → uniformization (union bound, symmetrization, chaining) → VC dimension / Rademacher complexity / empirical process → uniform generalization bound → $R(\hat f)-\widehat R_{\text{train}}(\hat f)$ 제어.

**Summary [p11–12]**
1. Markov: 비음 확률변수의 꼬리를 기댓값만으로 제어.
2. Chebyshev: 유한 분산으로 편차를 $1/t^2$ 수준으로 제어.
3. Chernoff: MGF로 exponential tail을 유도하는 일반 원리.
4. Mills: 정규분포 꼬리가 지수적으로 감소.
5. Hoeffding: 독립·유계 변수의 평균에 sub-Gaussian tail.
6. Bernstein: 분산 정보와 꼬리를 함께 써서 Hoeffding보다 정교할 수 있다.
7. ML: test error ↔ generalization error는 concentration이 직접 제어. training error ↔ generalization error에는 uniform 확장(VC, Rademacher, empirical process)이 필요.

---

## hw1 풀이

이 장에 속하는 문제: #1, #2, #4. #1·#4의 모범 답안 전문은 본문에 있다 (강의노트 정리의 증명이므로 한 번만 쓴다). 여기서는 전략, 채점 포인트, 내 풀이 점검을 정리한다. #2는 여기에 전문을 쓴다.

### hw1 1 Hoeffdings lemma

📝 [hw1 p1] · 모범 답안: [5.1](#51-hoeffdings-lemma) · 내 풀이 [CamScanner p1–2]

**전략** 현 부등식으로 $e^{\lambda X}$를 $X$의 1차식으로 누르면 기댓값이 계산된다 → 남은 식을 한 변수 $h$의 함수 $L(h)$로 → $L(0)=L'(0)=0$, $L''\le1/4$ → Taylor.

**채점 포인트**
- (a) $a=b$ 경우를 따로 처리, 귀류로 $a\le0\le b$
- (b) **가중치 $\frac{b-x}{b-a},\frac{x-a}{b-a}$ 명시**, 볼록성의 이유 ($g''\ge0$)
- (c) **$\mathbb E X=0$을 쓰는 곳 명시**
- (d) $p\in[0,1]$의 근거, $\lambda a=-ph$ 확인, $e^{-ph}(1-p+pe^h)$로 묶기
- (e) $L''=u(1-u)$ 계산과 $u\in[0,1]$의 이유
- (f) 나머지항 있는 Taylor ($\xi$ 명시), $h=\lambda(b-a)$ 대입

**내 풀이 점검**
- ✅ (a)–(f) 흐름과 계산 모두 맞다. (c)에서 $\mathbb E X=0$을 두 번 쓴 곳을 표시한 것, (e)의 $L''$ 계산, (f)의 Taylor 모두 좋다.
- ⚠ (b) $f''(x)=\lambda^2e^{\lambda x} > 0$이라고 씀 → $\lambda=0$이면 0이므로 **$\ge0$** (볼록성에는 $\ge0$이면 충분).
- ⚠ (d) " $b-a > -a > 0$"이라고 씀 → $a=0$일 수 있으므로 **$-a\ge0$**, 그리고 $b-a\ge-a$ (∵ $b\ge0$). 결론 $p\in[0,1]$은 같다.

### hw1 2 Bernstein vs Hoeffding

📝 [hw1 p2] · 내 풀이 [CamScanner p2–3]

**문제 요지** $a\le X_i\le b$ 독립, $S_n=\sum(X_i-\mathbb E X_i)$. 유계형 Bernstein을 적용하고 Hoeffding $\Pr(S_n\ge t)\le\exp\big(-\frac{2t^2}{n(b-a)^2}\big)$과 비교한다.

**전략** Bernstein의 조건 (독립, 평균 0, $\lvert Y_i\rvert\le M$)을 하나씩 확인 → 분산 상계 → 두 지수의 **분모를 비교**하는 문제로 바꾼다.

**모범 답안**

**(a)** $Y_i=X_i-\mathbb E X_i$는 각각 $X_i$만의 함수이므로 $Y_1,\dots,Y_n$은 독립이다. $\mathbb E Y_i=\mathbb E X_i-\mathbb E X_i=0$. $a\le X_i\le b$ a.s.에서 기댓값을 취하면 $a\le\mathbb E X_i\le b$. 따라서 $Y_i=X_i-\mathbb E X_i\in[a-b,\ b-a]$, 즉 $\lvert Y_i\rvert\le b-a$ a.s. → **$M=b-a$**.

**(b)** 임의의 상수 $c$에 대해 $\mathbb E(Z-c)^2=\mathop{\mathrm{Var}}\nolimits Z+(\mathbb E Z-c)^2\ge\mathop{\mathrm{Var}}\nolimits Z$ [T7]. $c=\frac{a+b}2$로 두면 $Z\in[a,b]$에서 $\lvert Z-\frac{a+b}2\rvert\le\frac{b-a}2$이므로

$$\mathop{\mathrm{Var}}\nolimits Z=\mathbb E(Z-\mathbb EZ)^2\le\mathbb E\Big(Z-\frac{a+b}2\Big)^2\le\frac{(b-a)^2}4 .$$

각 $X_i$에 적용하면 $\sigma^2=\sum\mathop{\mathrm{Var}}\nolimits X_i\le\frac{n(b-a)^2}4$. ($\mathop{\mathrm{Var}}\nolimits Y_i=\mathop{\mathrm{Var}}\nolimits X_i$이므로 Bernstein의 $\sigma^2$과 같다.)

**(c)** Bernstein에 $M=b-a$를 넣으면

$$\Pr(S_n\ge t)\le\exp\Big(-\frac{t^2}{2(\sigma^2+(b-a)t/3)}\Big)=\exp\Big(-\frac{t^2}{2\sigma^2+\frac23(b-a)t}\Big).$$

bound는 $\sigma^2$에 대해 증가하므로 (b)를 넣으면

$$\Pr(S_n\ge t)\le\exp\Big(-\frac{t^2}{\frac12n(b-a)^2+\frac23(b-a)t}\Big).$$

**(d)** Hoeffding은 $\exp\big(-\frac{t^2}{\frac12n(b-a)^2}\big)$. $t > 0$이면 (c)의 분모가 $\frac23(b-a)t$만큼 더 크므로 지수가 작고, **모든 $t > 0$에서 Hoeffding의 bound가 더 작다 (더 강하다).** 분산을 모르고 범위에서 나온 최악값만 쓰면 Bernstein은 이득이 없다. Bernstein의 장점은 실제 $\sigma^2$이 작을 때만 나온다.

**(e)** 실제 $\sigma^2$을 알 때, Bernstein이 더 강할 조건은 지수 비교로

$$\frac{t^2}{2\sigma^2+\frac23(b-a)t} \gt  \frac{t^2}{\frac12n(b-a)^2}\iff2\sigma^2+\frac23(b-a)t \lt  \frac12n(b-a)^2\iff\sigma^2 \lt  \frac{n(b-a)^2}4-\frac{(b-a)t}3 .$$

$\sigma^2\ge0$이므로 이 조건이 가능하려면 우변이 양수, 즉 **$t < \frac34n(b-a)$** 여야 한다.

**(f)** $t\ll\sigma^2/(b-a)$이면 $\frac23(b-a)t\ll\frac23\sigma^2$이라 분모 ≈ $2\sigma^2$, 따라서 $\Pr(S_n\ge t)\lesssim\exp\big(-\frac{t^2}{2\sigma^2}\big)$. Hoeffding 지수 $\frac{2t^2}{n(b-a)^2}$과 비교하면 비율이 $\frac{n(b-a)^2}{4\sigma^2}\ge1$이고, 분산이 작을수록 Bernstein이 지수적으로 강하다. Hoeffding은 범위만 써서 각 항의 분산을 최악값 $(b-a)^2/4$로 취급하기 때문이다. 반대로 $t\gg\sigma^2/(b-a)$이면 분모 ≈ $\frac23(b-a)t$라 Bernstein 지수는 $\frac{3t}{2(b-a)}$로 $t$에 1차 (지수 꼬리)이고, Hoeffding 지수는 $t$에 2차다. 따라서 충분히 큰 $t$ (정확히는 $t > \frac34n(b-a)$, 단 $S_n\le n(b-a)$라 그 이하 범위)에서는 **Hoeffding이 유리**하다.

**채점 포인트**
- (a) 독립·평균 0·유계를 **각각** 확인 ($\mathbb E X_i\in[a,b]$를 써야 $\lvert Y_i\rvert\le b-a$가 나옴)
- (b) " $\mathop{\mathrm{Var}}\nolimits Z\le\mathbb E(Z-c)^2$"의 이유 한 줄
- (c) bound가 $\sigma^2$에 대해 단조라서 상계를 넣을 수 있다는 말
- (d) " $t > 0$이면"
- (e) 부등호 방향, 엄격한 $<$, $t < \frac34 n(b-a)$
- (f) 두 영역 각각의 근사식과 "왜" (분산 정보 vs 범위만)

**내 풀이 점검**
- ✅ (a)(b)(c)(e)(f) 모두 결론이 맞다. (b)에서 $c=\mathbb EZ$로 두고 전개한 방식도 좋다.
- ⚠ (d) " $t < 0$: Bernstein이 더 좋은 상계"라고 씀 → 꼬리부등식은 **$t > 0$에서만** 의미가 있다 (Chernoff의 $\lambda^\star > 0$이 $t > 0$을 요구). 이 줄은 지우고 "모든 $t > 0$에서 Hoeffding이 더 강하다"만 남긴다.
- ⚠ (e) " $t\le\frac34n(b-a)$" → **$t < \frac34n(b-a)$** (등호면 우변이 0이라 $\sigma^2 < 0$이 되어 불가능).

**변형** → [V6](#v6-bernoulli에서-bernstein-vs-hoeffding)

### hw1 4 sub-Gaussian과 대칭화

📝 [hw1 p3] · 모범 답안: [6.1](#61-정의와-해석)–[6.3](#63-대칭화로-hoeffding약한-판-증명) · 내 풀이 [CamScanner p5–6]

**전략** (a) 정의 + 정규 MGF → (b) 급수 비교 → (c) 조건부 Jensen으로 복사본 → (d) 대칭 + Rademacher 조건부 → (e) 독립으로 곱 → (f) Chernoff 최적화 + 합집합.

**채점 포인트**
- (a) 중심화된 MGF $e^{\sigma^2s^2/2}$를 **계산**해서 보이기
- (b) 홀수 차수 소거, $(2k)!\ge2^kk!$의 근거
- (c) **볼록함수 $u\mapsto e^u$ 명시**, $\mathbb E[s(X-X')\mid X]=sX$의 근거 ($X'$ 독립, $\mathbb EX'=0$)
- (d) 같은 분포임을 보이는 계산 ($\varepsilon$ 독립 + $X-X'$ 대칭), $\lvert X-X'\rvert\le b-a$
- (e) $X_i-\mu$에 (d)를 적용할 수 있는 이유 (평균 0, 범위 길이 $b-a$), parameter $(b-a)/\sqrt n$
- (f) $s^\star=nt/(b-a)^2$, 반대쪽 꼬리도 같은 논리, 합집합으로 ×2

**내 풀이 점검**
- ✅ (a)–(f) 모두 맞다. (d)에서 $\Pr(\varepsilon(X-X')\le k)$를 $\varepsilon=\pm1$로 나눠 계산한 것, (f)에서 완전제곱으로 $s^\star$를 찾은 것이 좋다.
- ⚠ (c) 조건부 기댓값을 쓴 Jensen이라는 점을 한 단어로 밝히면 더 좋다 ("조건부 Jensen").

---

## 변형 문제

난이도: `●` hw 문제의 분포·숫자만 바꿈 · `●●` 도구 두세 개 조합 · `●●●` 장을 넘나듦. 힌트부터 열고, 스스로 써 본 뒤 풀이를 연다.

### V1 정규분포의 Chernoff bound와 Mills

`●` $X\sim N(\mu,\sigma^2)$.
(a) Chernoff로 $\Pr(X-\mu\ge t)\le e^{-t^2/(2\sigma^2)}$ ($t > 0$)을 보여라.
(b) $t=3\sigma$에서 (a), Mills의 한쪽 꼬리 판 $\Pr(Z > t)\le\frac{1}{\sqrt{2\pi}}\frac{e^{-t^2/2}}{t}$, Chebyshev를 비교하라.

<details><summary>전략 힌트</summary>

$\mathbb E e^{\lambda(X-\mu)}=e^{\sigma^2\lambda^2/2}$ → 4.1의 최적화 공식. Mills 한쪽 판은 Thm 1.4 증명에서 대칭 인수 2를 뺀 것.

</details>
<details><summary>풀이</summary>

(a) $\lambda > 0$: $\Pr(X-\mu\ge t)\le e^{-\lambda t}e^{\sigma^2\lambda^2/2}$. $\lambda^\star=t/\sigma^2$에서 지수 $-t^2/(2\sigma^2)$.

(b) $Z=(X-\mu)/\sigma$, $t=3$: Chernoff $e^{-4.5}=0.0111$, Mills 한쪽 $\frac{e^{-4.5}}{3\sqrt{2\pi}}=0.00148$, 실제 $\Pr(Z > 3)=0.00135$. Chebyshev는 양측 $1/9=0.111$ (한쪽만 따로 주지 않음). Mills가 $1/t$ 인수만큼 Chernoff보다 날카롭다.

</details>

### V2 표본 크기 Chebyshev vs Hoeffding

`●` $X_i\overset{\text{iid}}{\sim}\text{Bernoulli}(p)$, $p$는 모른다. $\Pr(\lvert\bar X_n-p\rvert\ge0.05)\le\delta$가 되도록 하는 최소 $n$을 (a) Chebyshev (b) Hoeffding으로 구하라. $\delta=0.05$와 $\delta=0.01$ 각각. (c) $\delta\to0$일 때 두 표본 크기의 차수를 비교하라.

<details><summary>전략 힌트</summary>

Chebyshev: $p(1-p)\le1/4$로 $p$를 없앤다. Hoeffding: $2e^{-2n\epsilon^2}\le\delta$를 $n$에 대해 푼다.

</details>
<details><summary>풀이</summary>

(a) $\Pr\le\frac{p(1-p)}{n\epsilon^2}\le\frac1{4n\epsilon^2}\le\delta\iff n\ge\frac1{4\epsilon^2\delta}$: $\delta=0.05$ → **2000**, $\delta=0.01$ → **10000**.

(b) $n\ge\frac{\log(2/\delta)}{2\epsilon^2}=200\log(2/\delta)$: $\delta=0.05$ → $200\log40=737.8$ → **738**, $\delta=0.01$ → $200\log200=1059.7$ → **1060**.

(c) Chebyshev는 $n\propto1/\delta$, Hoeffding은 $n\propto\log(1/\delta)$. 높은 신뢰도가 필요할수록 차이가 벌어진다.

</details>

### V3 포아송 분포의 Chernoff bound

`●●` $X\sim\text{Poi}(\mu)$.
(a) $M_X(\lambda)=\exp(\mu(e^\lambda-1))$을 보여라.
(b) $t > \mu$이면 $\Pr(X\ge t)\le e^{-\mu}\big(\frac{e\mu}{t}\big)^t$임을 보여라.
(c) $\mu=10$, $t=20$에서 bound와 Chebyshev를 비교하라.

<details><summary>전략 힌트</summary>

Chernoff의 지수 $g(\lambda)=-\lambda t+\mu(e^\lambda-1)$을 미분해 최소화. $\lambda^\star > 0$이려면 $t > \mu$.

</details>
<details><summary>풀이</summary>

(a) $\mathbb E e^{\lambda X}=\sum_x e^{\lambda x}e^{-\mu}\frac{\mu^x}{x!}=e^{-\mu}e^{\mu e^\lambda}$.

(b) $g'(\lambda)=-t+\mu e^\lambda=0$ → $\lambda^\star=\log(t/\mu) > 0$ ($t > \mu$). $g(\lambda^\star)=-t\log\frac t\mu+t-\mu$ → $\Pr(X\ge t)\le e^{-\mu}e^t(\mu/t)^t=e^{-\mu}(e\mu/t)^t$.

(c) bound $=e^{-10}(10e/20)^{20}=0.0210$. Chebyshev: $\Pr(X\ge20)\le\Pr(\lvert X-10\rvert\ge10)\le10/100=0.1$. 실제 값 $0.00345$.

</details>

### V4 sub-Gaussian의 성질

`●●` (a) 독립인 $X_i$가 각각 평균 $\mu_i$, $\sigma_i$-sub-Gaussian이면 $\sum X_i$가 $\sqrt{\sum\sigma_i^2}$-sub-Gaussian임을 보여라.
(b) (a)에서 $\Pr\big(\sum(X_i-\mu_i)\ge t\big)\le\exp\big(-t^2/(2\sum\sigma_i^2)\big)$을 보여라.
(c) $\sigma$-sub-Gaussian이면 $\mathop{\mathrm{Var}}\nolimits X\le\sigma^2$임을 보여라.
(d) (c)와 Lemma 1.5로 hw1 #2(b)의 $\mathop{\mathrm{Var}}\nolimits X\le(b-a)^2/4$를 다시 얻어라.

<details><summary>전략 힌트</summary>

(a) 독립 → MGF 곱. (b) 4.1 최적화 공식. (c) 양변을 $s=0$ 근처에서 2차까지 전개하고 $s^2$으로 나눈 뒤 $s\to0$.

</details>
<details><summary>풀이</summary>

(a) $\mathbb E e^{s\sum(X_i-\mu_i)}=\prod\mathbb E e^{s(X_i-\mu_i)}\le\prod e^{\sigma_i^2s^2/2}=e^{(\sum\sigma_i^2)s^2/2}$.

(b) Chernoff: $\le\exp(-st+\frac{s^2}2\sum\sigma_i^2)$, $s^\star=t/\sum\sigma_i^2$.

(c) $Y=X-\mu$: $\mathbb E e^{sY}=1+s\thinspace\mathbb EY+\frac{s^2}2\mathbb EY^2+o(s^2)=1+\frac{s^2}2\mathop{\mathrm{Var}}\nolimits X+o(s^2)$, 그리고 $e^{\sigma^2s^2/2}=1+\frac{\sigma^2s^2}2+o(s^2)$. 부등식에서 1을 빼고 $s^2/2$로 나눈 뒤 $s\to0$: $\mathop{\mathrm{Var}}\nolimits X\le\sigma^2$.

(d) Lemma 1.5: $[a,b]$ 값이면 $X-\mathbb EX$는 $\frac{b-a}2$-sub-Gaussian → (c)로 $\mathop{\mathrm{Var}}\nolimits X\le\frac{(b-a)^2}4$.

</details>

### V5 최댓값의 기댓값 bound

`●●●` $X_1,\dots,X_n$이 각각 평균 0, $\sigma$-sub-Gaussian이다 (**독립일 필요 없음**).
(a) $\lambda > 0$에 대해 $\exp\big(\lambda\thinspace\mathbb E\max_iX_i\big)\le n\thinspace e^{\lambda^2\sigma^2/2}$임을 보여라.
(b) $\mathbb E\max_iX_i\le\sigma\sqrt{2\log n}$을 보여라.
(c) $\Pr(\max_iX_i\ge t)\le n\thinspace e^{-t^2/(2\sigma^2)}$을 보여라.

<details><summary>전략 힌트</summary>

(a) Jensen ($e^{\lambda x}$ 볼록) → $e^{\lambda\max}=\max e^{\lambda X_i}\le\sum e^{\lambda X_i}$. (b) 로그를 취해 $\lambda$ 최적화. (c) 합집합.

</details>
<details><summary>풀이</summary>

(a) $e^{\lambda\mathbb E\max X_i}\le\mathbb E e^{\lambda\max X_i}$ [Jensen] $=\mathbb E\max_ie^{\lambda X_i}\le\sum_i\mathbb E e^{\lambda X_i}\le ne^{\lambda^2\sigma^2/2}$.

(b) $\mathbb E\max X_i\le\frac{\log n}\lambda+\frac{\lambda\sigma^2}2$. $\lambda=\sqrt{2\log n}/\sigma$에서 $\sigma\sqrt{2\log n}$.

(c) $\Pr(\max X_i\ge t)\le\sum_i\Pr(X_i\ge t)\le ne^{-t^2/(2\sigma^2)}$.

해석: 표준정규 $n$개의 최댓값은 약 $\sqrt{2\log n}$ 크기다 → ch3 hw1 #7 ($M_n\approx\log n/\lambda$)과 비교해 볼 것.

</details>

### V6 Bernoulli에서 Bernstein vs Hoeffding

`●●` $X_i\overset{\text{iid}}{\sim}\text{Bernoulli}(p)$, $S_n=\sum(X_i-p)$.
(a) Hoeffding bound와 유계형 Bernstein bound ($M=1$)를 쓰라.
(b) 상대 편차 $t=c\thinspace np$ ($c > 0$ 고정)에서 두 지수를 구하라.
(c) $np=\lambda$로 고정하고 $p\to0$ (희귀 사건)일 때 어느 bound가 쓸모 있는가?

<details><summary>전략 힌트</summary>

$\lvert X_i-p\rvert\le\max(p,1-p)\le1$, $\sigma^2=np(1-p)$. (b)에서 두 지수를 $n,p,c$로 쓰고 (c)에서 극한.

</details>
<details><summary>풀이</summary>

(a) Hoeffding $\exp(-2t^2/n)$, Bernstein $\exp\big(-\frac{t^2}{2np(1-p)+\frac23t}\big)$.

(b) Hoeffding 지수 $2c^2np^2$, Bernstein 지수 $\frac{c^2n^2p^2}{2np(1-p)+\frac23cnp}=\frac{c^2np}{2(1-p)+\frac23c}$.

(c) Hoeffding 지수 $=2c^2\lambda p\to0$ → bound → 1 (쓸모없음). Bernstein 지수 $\to\frac{c^2\lambda}{2+\frac23c}$ (양수로 남음). 희귀 사건에서는 분산 $np$가 범위 기반 최악값 $n/4$보다 훨씬 작아서 Bernstein만 의미 있는 bound를 준다. (7.5의 수치 예가 $\lambda=10$, $c=2$인 경우)

</details>

### V7 유한 함수집합의 uniform bound

`●●` 손실 $0\le L\le1$, 함수집합 $\mathcal F=\lbrace f_1,\dots,f_K\rbrace$, 학습 데이터는 iid $n$개. $\hat f\in\mathcal F$는 $\widehat R_{\text{train}}$을 최소화하는 함수(ERM), $f^\star=\arg\min_{f\in\mathcal F}R(f)$.
(a) $\Pr\big(\max_k\lvert\widehat R_{\text{train}}(f_k)-R(f_k)\rvert > \epsilon\big)\le2Ke^{-2n\epsilon^2}$.
(b) 확률 $1-\delta$ 이상으로 $\lvert R(\hat f)-\widehat R_{\text{train}}(\hat f)\rvert\le\epsilon_n:=\sqrt{\log(2K/\delta)/(2n)}$.
(c) 같은 사건 위에서 $R(\hat f)\le R(f^\star)+2\epsilon_n$.

<details><summary>전략 힌트</summary>

고정된 $f_k$ 하나에는 Hoeffding이 된다 → 합집합으로 $K$배. (b)는 "모든 $f$에서 성립 ⟹ 데이터로 고른 $\hat f$에서도 성립". (c)는 $\hat f$가 훈련오차 최소라는 사실을 가운데에 끼운다.

</details>
<details><summary>풀이</summary>

(a) 각 $k$에서 $L(f_k(x_i),y_i)$는 iid, $[0,1]$ 값 → Hoeffding $\Pr(\lvert\widehat R(f_k)-R(f_k)\rvert > \epsilon)\le2e^{-2n\epsilon^2}$. 합집합으로 $K$개를 더한다.

(b) $2Ke^{-2n\epsilon^2}=\delta$를 풀면 $\epsilon=\epsilon_n$. 여사건 (확률 $\ge1-\delta$) 위에서 모든 $f\in\mathcal F$에 대해 $\lvert\widehat R(f)-R(f)\rvert\le\epsilon_n$이고, $\hat f\in\mathcal F$이므로 $\hat f$에서도 성립한다. (데이터가 $\hat f$를 골랐어도 상관없다 — 이것이 uniform convergence의 요점)

(c) $R(\hat f)\le\widehat R(\hat f)+\epsilon_n\le\widehat R(f^\star)+\epsilon_n\le R(f^\star)+2\epsilon_n$. (가운데 부등호: ERM의 정의)

</details>

### V8 Markov와 Chebyshev의 등호

`●` (a) $\mathbb EX=m$, $X\ge0$이고 $\Pr(X\ge t)=m/t$가 되는 분포를 만들어라 ($t\ge m$).
(b) 평균 $\mu$, 분산 $\sigma^2$이고 $\Pr(\lvert X-\mu\rvert\ge k\sigma)=1/k^2$인 분포를 만들어라 ($k\ge1$).
(c) 이것이 두 부등식에 대해 무엇을 말하는가?

<details><summary>풀이</summary>

(a) $X=t$ (확률 $m/t$), $X=0$ (확률 $1-m/t$).

(b) $X=\mu\pm k\sigma$ (각 확률 $\frac1{2k^2}$), $X=\mu$ (확률 $1-\frac1{k^2}$). 분산 $=k^2\sigma^2\cdot\frac1{k^2}=\sigma^2$.

(c) 평균만(분산만) 알 때는 bound를 더 줄일 수 없다. 더 빠른 감소에는 추가 가정(유계, MGF)이 필요하다 → Hoeffding·Bernstein.

</details>

### V9 Hoeffding으로 강한 대수의 법칙

`●●●` $X_1,X_2,\dots$ iid, $a\le X_i\le b$, $\mathbb EX_i=\mu$. ch3 보충의 Theorem 2.1 (모든 $\epsilon > 0$에 대해 $\sum_n\Pr(\lvert Y_n-Y\rvert > \epsilon) < \infty$ ⟹ $Y_n\xrightarrow{\text{a.s.}}Y$)을 써서 $\bar X_n\xrightarrow{\text{a.s.}}\mu$를 보여라.

<details><summary>풀이</summary>

Hoeffding 양측: $\Pr(\lvert\bar X_n-\mu\rvert > \epsilon)\le2\exp\big(-\frac{2n\epsilon^2}{(b-a)^2}\big)=2r^n$, $r=e^{-2\epsilon^2/(b-a)^2}\in(0,1)$. $\sum_n2r^n=\frac{2r}{1-r} < \infty$ (등비급수). Theorem 2.1로 $\bar X_n\xrightarrow{\text{a.s.}}\mu$. (Chebyshev의 $\frac{\sigma^2}{n\epsilon^2}$은 합이 발산해서 이 방법이 안 된다 — 지수 꼬리가 필요한 이유) → [ch3 보충](ch3%20확률변수의%20수렴.md#8-보충-almost-sure-수렴과-borelcantelli)

</details>

---

## 셀프 테스트

### 백지 증명 체크리스트

뼈대만 보고 전문을 쓸 수 있으면 ☑.

| ☐ | 증명 | 뼈대 | 위치 |
| --- | --- | --- | --- |
| ☐ | Markov (일반) | $t\thinspace I(X\ge t)\le X$ → 기댓값 | [2](#2-markov-부등식) |
| ☐ | Chebyshev | $(X-\mu)^2$에 Markov | [3](#3-chebyshev-부등식) |
| ☐ | Chernoff | $\lambda > 0$, $e^{\lambda X-\lambda t}\ge1$ ⟺ $X\ge t$, Markov | [4.1](#41-chernoff-bound) |
| ☐ | Mills | 대칭 → $s/t\ge1$ 곱하기 → $u=s^2/2$ | [4.2](#42-mills-부등식) |
| ☐ | **Hoeffding's lemma** | (a) $a\le0\le b$ (b) 현, 가중치 (c) E, EX=0 (d) $p,h$, $L(h)$ (e) $L(0)=L'(0)=0$, $L''=u(1-u)\le\frac14$ (f) Taylor | [5.1](#51-hoeffdings-lemma) |
| ☐ | **Hoeffding 부등식** | Chernoff → 독립 곱 → lemma → $\lambda^\star=4\epsilon/\sum(b_i-a_i)^2$ | [5.2](#52-hoeffding의-부등식) |
| ☐ | Rademacher MGF | 급수 더하기 → $(2k)!\ge2^kk!$ | [6.2](#62-rademacher-변수) |
| ☐ | **대칭화** | 조건부 Jensen → 대칭 + $\varepsilon$ → 조건부 Rademacher → $\lvert X-X'\rvert\le b-a$ → 곱 → Chernoff + 합집합 | [6.3](#63-대칭화로-hoeffding약한-판-증명) |
| ☐ | Bernstein (보충) | 급수로 MGF 상계 → $1+x\le e^x$ → Chernoff → $\lambda=\epsilon/(V+M\epsilon)$ | [7.2](#72-bernstein-부등식의-보충-증명) |
| ☐ | 유계형 = $M/3$ | $\mathbb E\lvert Y\rvert^k\le M^{k-2}\sigma^2$, $2\cdot3^{k-2}\le k!$ | [7.3](#73-유계형-bernstein) |
| ☐ | **Bernstein vs Hoeffding** | 분모 비교 → $\sigma^2 < \frac{n(b-a)^2}4-\frac{(b-a)t}3$ → 두 영역 | [7.5](#75-bernstein-vs-hoeffding) |

### 짧은 문답

<details><summary>1. Markov·Chebyshev·Hoeffding·Bernstein의 가정과 꼬리 모양은?</summary>

$X\ge0$ → $1/t$ · 유한 분산 → $1/t^2$ · 독립 + 유계 → $e^{-ct^2}$ · 독립 + 적률조건(sub-exponential) → $e^{-c\min(t^2,t)}$.

</details>
<details><summary>2. 베르누이 예제 (n = 100, ε = 0.2)의 두 숫자는?</summary>

Chebyshev 0.0625, Hoeffding $2e^{-8}\approx0.00067$.

</details>
<details><summary>3. 평균이 0이 아닌 유계 변수에 Hoeffding을 쓰려면?</summary>

$X_i-\mathbb EX_i$에 쓴다. 범위 길이가 $b_i-a_i$로 그대로라 bound도 같다.

</details>
<details><summary>4. 대칭화로 얻은 parameter와 Lemma 1.5의 parameter는?</summary>

대칭화 $b-a$, Lemma 1.5 $(b-a)/2$. 그래서 대칭화 판은 지수가 1/4이다.

</details>
<details><summary>5. Bernstein 적률조건에서 유계형으로 갈 때 M은 어떻게 바뀌나?</summary>

$M\to M/3$, $v_i=\mathop{\mathrm{Var}}\nolimits Y_i$.

</details>
<details><summary>6. Bernstein이 Hoeffding보다 강해지는 t의 필요조건은?</summary>

$t < \frac34 n(b-a)$ (그리고 $\sigma^2 < \frac{n(b-a)^2}4-\frac{(b-a)t}3$).

</details>
<details><summary>7. 확률 1 − δ로 |X̄ − p| 가 얼마 이하인가 (Hoeffding)?</summary>

$\sqrt{\log(2/\delta)/(2n)}$.

</details>

---

## 강의노트 확인

강의노트를 텍스트로 전부 읽고, 수식이 많은 p4–p10은 이미지로 다시 확인했다. 철자 오타(Hoeffinding, Chenoff, Berstein, inquality)는 본문에서 조용히 고쳤다.

| 쪽 | 강의노트 원문 | 노트에 반영한 것 | 근거 | 영향 |
| --- | --- | --- | --- | --- |
| p10 Thm 1.7 | $\bar v=v_1+\cdots+v_n/n$ | ✏️ $\bar v=(v_1+\cdots+v_n)/n$ | 합의 꼴 증명 ($V=n\bar v$) | ★ |
| p8–9 Thm 1.6 증명 | Chernoff 변수와 정리의 임계값을 모두 $t$로 씀 | Chernoff 변수를 $\lambda$로 | 기호 충돌 | · |
| p4 Thm 1.1 | 정리는 $\Pr(X > t)$, 증명의 결론은 $\Pr(X\ge t)$ | 그대로 (≥ 꼴이 더 강해서 둘 다 성립) | – | · |
| p4 Thm 1.1 증명 | 밀도 $f$가 있다고 가정 | 일반 증명 (보충) 추가 | 이산형에도 성립해야 함 | · |
| p10 Remark | "Bernstein이 일반적으로 Hoeffding보다 더 sharp" | 분산 정보가 작을 때만. 최악 분산이면 Hoeffding이 더 강함 | hw1 #2(d) | ★ (개념) |
| 1주차 판 | Lemma 1.5, Hoeffding 증명, Bernstein Remark 없음 | 9/9 갱신판(lecture_ch1) 기준 | 두 판 비교 | · |
