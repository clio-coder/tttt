# Problem Set 1 풀이 학습노트

> 원본: `Problem Set1.pdf` (M1407.0012, Prof. Youngjoon Hong, 4문제, 제출 기한 없음)
> 이 노트의 목적: ① 문제마다 **어느 수업 내용으로 푸는지** ② **풀이 순서(레시피)** ③ 완전한 풀이 ④ 채점에서 빠지기 쉬운 부분을 정리한다. 같은 유형의 새 문제는 [연습문제 1](연습문제%201.md)에 있다.
> 표시: ✔ 채점 포인트(반드시 써야 하는 줄) · ✗ 자주 하는 실수 · (보충) 수업에 없던 설명

---

## 0. 한눈에 보기

| 문제 | 묻는 것 | 쓰는 수업 도구 | 핵심 한 줄 |
| --- | --- | --- | --- |
| P1 | Jensen–Shannon divergence의 비음수성·비퇴화성·대칭성 | **KL ≥ 0과 등호 조건** ([09.07 §9](../수업정리/09.07%20수업정리.md)) | $m$이 확률분포임을 보이고 KL 정리를 두 번 쓴다 |
| P2 | 정규분포 평균의 MAP = ridge, 축소 계수, bias–variance | **MAP = loss + regularization** ([09.07 §6](../수업정리/09.07%20수업정리.md)), **ridge** ([09.09 §5](../수업정리/09.09%20수업정리.md)) | $-\log$ 사후분포 → 상수 버리고 2배 → 미분 = 0 |
| P3 | 점 2개짜리 hard-margin SVM의 쌍대 문제를 끝까지 풀기 | **SVM 라그랑주·쌍대· $b$ 공식** ([09.14 §7.5–7.7](../수업정리/09.14%20수업정리.md)) | $w=\sum\alpha_iy_ix_i$, $\sum\alpha_iy_i=0$ → 쌍대 최대화 → support vector로 $b$ |
| P4 | max–min 부등식 증명과 반례 | **SVM 쌍대의 "min max ≥ max min"** ([09.14 §7.7](../수업정리/09.14%20수업정리.md)) | 두 변수를 모두 고정한 사슬 부등식에서 한쪽씩 max·min |

### 출제 패턴 — 이 문제지에서 보이는 것

| 패턴 | PS1에서 | 시험 대비 전략 |
| --- | --- | --- |
| **수업 정리를 새 대상에 적용** | P1: KL ≥ 0 → JS divergence | 🔴 정리마다 "이걸 어디에 또 쓸 수 있나"를 생각해 둔다 (예: KL ≥ 0 → 상호정보량 ≥ 0) |
| **수업 유도를 구체적인 모델로** | P2: "MAP = ridge"를 1차원 평균 추정으로 | 일반 유도를 가장 작은 예($d=1$, 점 2개)로 직접 계산해 본다 |
| **필기 없이 넘어간 슬라이드도 나온다** | P3·P4: 9/14 p37–40 (SVM 쌍대, $b$ 계산, min–max 부등식) — **필기가 없던 쪽** | 슬라이드에서 **식만 주고 증명하지 않은 주장**을 직접 증명해 본다 → §5 목록 |
| **수업에 없던 도구는 힌트와 함께** | P2-3: bias–variance 분해 (Hint 제공) | 힌트의 "더하고 빼기" 같은 요령을 도구 상자에 모아 둔다 → §6 |

---

## 1. Problem 1 — Jensen–Shannon divergence

**문제** 확률분포 $p,q$와 $m=\frac12(p+q)$에 대해 $D_{JS}(p\Vert q)=\frac12D_{KL}(p\Vert m)+\frac12D_{KL}(q\Vert m)$일 때 (i) $D_{JS}\ge0$ (ii) $D_{JS}=0\iff p=q$ (iii) $D_{JS}(p\Vert q)=D_{JS}(q\Vert p)$를 보여라.

**쓰는 도구** Theorem 1 (9/7): $KL(p\Vert q)\ge0$, 등호 $\iff p=q$. **단, 두 인자가 모두 확률분포여야 한다.**

**풀이 순서**
1. $m$이 확률분포임을 먼저 보인다 (KL 정리를 쓸 자격).
2. (i) 각 항에 KL ≥ 0.
3. (ii) "음이 아닌 두 수의 합 = 0 ⇔ 둘 다 0" + KL 등호 조건. **양방향 모두.**
4. (iii) $m$이 $p,q$에 대칭.

**풀이** (이산 분포로 쓴다. 연속 분포는 합을 적분으로 바꾸면 같다 — 9/7 슬라이드 "similar way for continuous")

0. **$m$은 확률분포다.** $m(x)=\frac12(p(x)+q(x))\ge0$이고 $\sum_xm(x)=\frac12(1+1)=1$.
   (보충) 또 $p(x)>0$이면 $m(x)\ge\frac12p(x)>0$이므로 $KL(p\Vert m)$에 $b\log\frac b0=\infty$ 항이 생기지 않는다. $KL(q\Vert m)$도 같다. 따라서 $D_{JS}$는 **항상 유한**하다 ($KL(p\Vert q)$와 다른 점).
1. **(i)** $p,m$과 $q,m$이 모두 확률분포이므로 Theorem 1에서 $KL(p\Vert m)\ge0$, $KL(q\Vert m)\ge0$. 따라서 $D_{JS}(p\Vert q)=\frac12KL(p\Vert m)+\frac12KL(q\Vert m)\ge0$.
2. **(ii) (⇐)** $p=q$면 $m=\frac12(p+p)=p$이므로 $KL(p\Vert m)=KL(p\Vert p)=0$, $KL(q\Vert m)=KL(p\Vert p)=0$ → $D_{JS}=0$.
   **(⇒)** $D_{JS}=0$이면 (i)에서 두 항이 모두 0 이상이므로 $KL(p\Vert m)=0$이고 $KL(q\Vert m)=0$. Theorem 1의 등호 조건에서 $p=m$이고 $q=m$. 따라서 $p=q$.
   (또는 $p=m=\frac12(p+q)$에서 바로 $p=q$.)
3. **(iii)** $m=\frac12(p+q)=\frac12(q+p)$이므로 $m$은 $p,q$의 순서와 무관하다. 따라서
   $$D_{JS}(q\Vert p)=\tfrac12KL(q\Vert m)+\tfrac12KL(p\Vert m)=D_{JS}(p\Vert q)$$

✔ $m$이 확률분포라는 줄 · ✔ (ii)의 양방향 · ✔ "두 항이 모두 0 이상이므로 둘 다 0"
✗ $m$이 분포인지 확인하지 않고 KL 정리를 씀 · ✗ (⇒)만 쓰고 (⇐) 생략 · ✗ KL이 대칭이라고 착각 (KL은 비대칭 — 9/7 p33)

**더 생각해 볼 것** $D_{JS}=H(m)-\frac12(H(p)+H(q))$, $D_{JS}\le\log2$, KL 삼각부등식 반례 → [연습문제 1 · 문제 1](연습문제%201.md)

---

## 2. Problem 2 — 정규분포 평균의 MAP, 축소, bias–variance

**문제** $X_1,\dots,X_n$ 독립, $X_i\sim N(\theta,1)$, prior $\theta\sim N(0,\tau^2)$.
1. MAP를 유도하고 ridge 문제 $\arg\min_\theta\sum(X_i-\theta)^2+\lambda\theta^2$, $\lambda=1/\tau^2$의 해와 같음을 보여라.
2. $\hat\theta_{MAP}=a\bar X$로 쓰고 축소 계수 $a$를 구하라.
3. $\mathbb E[(\hat\theta-\theta)^2]=(\mathbb E\hat\theta-\theta)^2+\text{Var}(\hat\theta)$를 증명하고 $\hat\theta_{MAP}$의 bias, variance, MSE를 구하라.

**쓰는 도구** 9/7 MAP 판서: $\hat\theta_{MAP}=\arg\min[-\log p(x\mid\theta)-\log p(\theta)]$, 가우시안 prior → $\frac{1}{2\tau^2}\theta^2$ · 9/9 가우시안 오차 → 제곱합 · 9/9 ridge

**풀이 순서 (MAP 레시피)**
① 사후분포 ∝ 가능도 × prior ② $-\log$ ③ $\theta$와 무관한 상수 버리기 ④ 양수배(여기선 2배) ⑤ 미분 = 0, 2계도함수 > 0 확인

**풀이**

**1.** 베이즈 정리로 $p(\theta\mid X)\propto p(X\mid\theta)\thinspace p(\theta)$ (분모 $p(X)$는 $\theta$와 무관).
$$p(X\mid\theta)=\prod_{i=1}^n\frac1{\sqrt{2\pi}}e^{-(X_i-\theta)^2/2},\qquad p(\theta)=\frac1{\sqrt{2\pi\tau^2}}e^{-\theta^2/(2\tau^2)}$$
$$-\log p(\theta\mid X)=\frac12\sum_{i=1}^n(X_i-\theta)^2+\frac{\theta^2}{2\tau^2}+C\quad(C\text{는 }\theta\text{와 무관})$$
$\hat\theta_{MAP}=\arg\max p(\theta\mid X)=\arg\min[-\log p(\theta\mid X)]$이고, 상수를 버리고 2를 곱해도 최소점은 그대로이므로
$$\hat\theta_{MAP}=\arg\min_\theta\sum_{i=1}^n(X_i-\theta)^2+\frac{1}{\tau^2}\theta^2\quad\Rightarrow\quad\lambda=\frac1{\tau^2}$$
- 9/7 판서 "data fitting loss + regularization"의 1차원 버전이다. $\frac{1}{\tau^2}$이 클수록(prior가 좁을수록) 정규화가 강하다.

**2.** $J(\theta)=\sum(X_i-\theta)^2+\lambda\theta^2$
$$J'(\theta)=-2\sum_{i=1}^n(X_i-\theta)+2\lambda\theta=-2n\bar X+2(n+\lambda)\theta=0\ \Rightarrow\ \hat\theta_{MAP}=\frac{n}{n+\lambda}\bar X$$
$J''(\theta)=2(n+\lambda)>0$이므로 최소점이다. $\lambda=1/\tau^2$를 넣으면
$$\boxed{a=\frac{n}{n+1/\tau^2}=\frac{n\tau^2}{n\tau^2+1}}\in(0,1)$$
- **해석:** MLE $\bar X$를 prior 평균 0 쪽으로 **줄인다(shrinkage)**. $n\to\infty$ 또는 $\tau^2\to\infty$(prior가 평평)면 $a\to1$ (MLE), $\tau^2\to0$이면 $a\to0$.
- 9/9 ridge 해 $(\lambda I+X^\top X)^{-1}X^\top y$에서 설계행렬이 $\mathbf 1_n$(1만 있는 열)인 경우다: $X^\top X=n$, $X^\top y=n\bar X$ (보충).

**3. 분해 증명** (Hint: $\hat\theta-\theta=(\hat\theta-\mathbb E\hat\theta)+(\mathbb E\hat\theta-\theta){}$)

$$\begin{aligned}\mathbb E[(\hat\theta-\theta)^2]&=\mathbb E[(\hat\theta-\mathbb E\hat\theta)^2]+2(\mathbb E\hat\theta-\theta)\thinspace\mathbb E[\hat\theta-\mathbb E\hat\theta]+(\mathbb E\hat\theta-\theta)^2\cr&=\text{Var}(\hat\theta)+0+(\mathbb E\hat\theta-\theta)^2\end{aligned}$$

가운데 항: $\mathbb E\hat\theta-\theta$는 상수(확률변수 아님)라서 밖으로 나오고, $\mathbb E[\hat\theta-\mathbb E\hat\theta]=0$.

**$\hat\theta_{MAP}$에 적용.** $\mathbb E[\bar X]=\theta$, $\text{Var}(\bar X)=\frac1{n^2}\sum\text{Var}(X_i)=\frac1n$ (∵ 독립)

| 양 | 계산 | 결과 |
| --- | --- | --- |
| Bias | $\mathbb E[a\bar X]-\theta=(a-1)\theta$ | $-\dfrac{\theta}{n\tau^2+1}$ |
| Variance | $a^2\thinspace\text{Var}(\bar X)=a^2/n$ | $\dfrac{n\tau^4}{(n\tau^2+1)^2}$ |
| MSE | Bias² + Var | $\dfrac{\theta^2+n\tau^4}{(n\tau^2+1)^2}$ |

- (보충) MLE $\bar X$는 bias 0, MSE $=1/n$. MAP가 더 나은 조건: $\dfrac{\theta^2+n\tau^4}{(n\tau^2+1)^2}<\dfrac1n\iff\theta^2<2\tau^2+\dfrac1n$. **prior가 참값을 크게 벗어나지 않으면 bias를 조금 내주고 variance를 크게 줄여 이긴다.**

✔ 분모 $p(X)$를 버리는 이유 · ✔ "상수 버리고 2배해도 argmin 불변" · ✔ 2계 조건 · ✔ 분해에서 가운데 항이 0인 이유 · ✔ $\text{Var}(\bar X)=1/n$의 근거(독립)
✗ $\lambda=1/(2\tau^2)$로 적음 (2배를 안 함) · ✗ $\text{Var}(a\bar X)=a\thinspace\text{Var}(\bar X)$ (제곱 빠뜨림) · ✗ bias 부호

**더 생각해 볼 것** 다차원 ridge, 최적 $\lambda^\ast=\sigma^2/\beta^2$ → [연습문제 1 · 문제 4](연습문제%201.md)

---

## 3. Problem 3 — 점 2개짜리 SVM 쌍대 문제

**문제** $x_1=(0,0),\thinspace y_1=-1$; $x_2=(2,2),\thinspace y_2=+1$. primal $\min\frac12\lVert w\rVert^2$ s.t. $y_i(w^\top x_i+b)\ge1$.
1. 라그랑주 함수 → $w,b$에 대해 최소화 → $\alpha_1,\alpha_2$에 대한 쌍대 → 최적 승수
2. $w,b$ 복원 3. 초평면과 마진

**쓰는 도구** 9/14 p38–40 (필기 없던 쪽): $L_p(w,b,\alpha)=\frac12w^\top w+\sum_i\alpha_i(1-y_i(x_i^\top w+b))$, $\nabla_wL_p=0\Rightarrow w=\sum\alpha_iy_ix_i$, $\partial_bL_p=0\Rightarrow\sum\alpha_iy_i=0$, 쌍대 $\max\sum\alpha_i-\frac12\sum\sum\alpha_i\alpha_jy_iy_jx_i^\top x_j$, support vector에서 $b=y_i-x_i^\top w$

**풀이 순서 (SVM 쌍대 레시피 6단계)**
① 라그랑주 함수 ② $\nabla_w=0$, $\partial_b=0$ ③ 대입해서 $g(\alpha)=\sum\alpha_i-\frac12\lVert\sum\alpha_iy_ix_i\rVert^2$ ④ 제약($\alpha_i\ge0$, $\sum\alpha_iy_i=0$) 아래 최대화 ⑤ $w=\sum\alpha_iy_ix_i$, $\alpha_s>0$인 점에서 $b=y_s-x_s^\top w$ ⑥ **검산**: 모든 점에서 $y_i(w^\top x_i+b)\ge1$, primal 값 = 쌍대 값

**풀이**

**① 라그랑주 함수** ($\alpha_1,\alpha_2\ge0$)
$$L(w,b,\alpha)=\frac12\lVert w\rVert^2+\alpha_1\big(1-y_1(w^\top x_1+b)\big)+\alpha_2\big(1-y_2(w^\top x_2+b)\big)=\frac12\lVert w\rVert^2+\alpha_1(1+b)+\alpha_2\big(1-2w_1-2w_2-b\big)$$
($x_1=0$이라 $w^\top x_1=0$, $y_1=-1$)

**② $w,b$에 대해 최소화**
$$\nabla_wL=w-\sum_i\alpha_iy_ix_i=0\Rightarrow w=-\alpha_1(0,0)+\alpha_2(2,2)=(2\alpha_2,\thinspace2\alpha_2)$$
$$\frac{\partial L}{\partial b}=-\sum_i\alpha_iy_i=\alpha_1-\alpha_2=0\Rightarrow\alpha_1=\alpha_2$$

**③ 쌍대 함수** 대입하면 $\sum\alpha_iy_ix_i^\top w=w^\top w$이고 $b$ 항은 $b\sum\alpha_iy_i=0$으로 사라져서
$$g(\alpha)=\sum_i\alpha_i-\frac12\sum_i\sum_j\alpha_i\alpha_jy_iy_jx_i^\top x_j,\qquad x_1^\top x_1=x_1^\top x_2=0,\ x_2^\top x_2=8$$
$$g(\alpha)=\alpha_1+\alpha_2-\frac12\cdot8\alpha_2^2=\alpha_1+\alpha_2-4\alpha_2^2$$

**④ 최대화** 제약 $\alpha_1=\alpha_2=:\alpha\ge0$ → $g=2\alpha-4\alpha^2$ (오목). $g'=2-8\alpha=0$ → $\boxed{\alpha_1=\alpha_2=\tfrac14}$, 쌍대 최적값 $g^\ast=\frac12-\frac14=\frac14$.

**⑤ $w,b$ 복원**
$$w=(2\alpha_2,2\alpha_2)=\big(\tfrac12,\tfrac12\big)$$
$\alpha_1,\alpha_2>0$이므로 둘 다 support vector. $b=y_2-x_2^\top w=1-(1+1)=-1$. (검산: $b=y_1-x_1^\top w=-1-0=-1$ ✓)

**⑥ 초평면과 마진**
- 초평면: $\frac12x_{(1)}+\frac12x_{(2)}-1=0\iff x_{(1)}+x_{(2)}=2$ (첫째·둘째 좌표)
- 검산: $y_1(w^\top x_1+b)=(-1)(-1)=1$ ✓, $y_2(w^\top x_2+b)=1\cdot(2-1)=1$ ✓
- primal 값 $\frac12\lVert w\rVert^2=\frac12\cdot\frac12=\frac14=g^\ast$ ✓ (등호 성립 — 슬라이드 "equality holds in certain conditions"의 예)
- **마진** (9/14 정의: 결정 평면과 가장 가까운 점 사이 거리) $=\dfrac{1}{\lVert w\rVert}=\dfrac{1}{1/\sqrt2}=\sqrt2$
  - 기하 검산: 두 점을 잇는 선분의 수직이등분선이 $x_{(1)}+x_{(2)}=2$이고, 두 점 사이 거리 $2\sqrt2$의 절반이 $\sqrt2$ ✓
  - (보충) $H_+$와 $H_-$ 사이 폭을 마진이라 부르는 책도 있다: $2/\lVert w\rVert=2\sqrt2$. 답안에는 **어느 정의인지 한 줄** 적는다.

✔ $w=\sum\alpha_iy_ix_i$, $\sum\alpha_iy_i=0$ 두 식 · ✔ 쌍대의 제약 $\alpha\ge0$ · ✔ $b$를 support vector에서 구함 · ✔ 검산
✗ $\partial_b$ 조건을 빠뜨려 $\alpha_1$을 정하지 못함 · ✗ $y_i$ 부호 실수 · ✗ 마진을 $1/\lVert w\rVert^2$로 씀

**더 생각해 볼 것** 점이 3개이고 하나는 support vector가 아닐 때 ($\alpha=0$이 되는 이유) → [연습문제 1 · 문제 6](연습문제%201.md)

---

## 4. Problem 4 — max–min 부등식

**문제** 공집합이 아닌 $X,Y$와 $f:X\times Y\to\mathbb R$에 대해 $\max_{x\in X}\min_{y\in Y}f(x,y)\le\min_{y\in Y}\max_{x\in X}f(x,y)$를 증명하고, 등호가 성립하지 않는 반례를 들어라.

**쓰는 도구** 9/14 p37: $\min_{w,b}\max_{\alpha\ge0}L\ \ge\ \max_{\alpha\ge0}\min_{w,b}L$ — 슬라이드는 이 부등식을 증명 없이 썼다. ($x\leftrightarrow\alpha$, $y\leftrightarrow(w,b)$로 보면 같은 부등식)

**풀이 순서** ① 두 변수를 모두 **고정** ② "min ≤ 값 ≤ max" 사슬 ③ 왼쪽은 $x'$만, 오른쪽은 $y'$만의 함수 → 한쪽씩 max, min

**증명** (문제대로 max·min이 존재한다고 둔다)
1. 임의의 $x'\in X$, $y'\in Y$를 고정하면
   $$\min_{y\in Y}f(x',y)\ \le\ f(x',y')\ \le\ \max_{x\in X}f(x,y')$$
   (첫 부등호: 최솟값은 $y=y'$에서의 값 이하. 둘째: 최댓값은 $x=x'$에서의 값 이상.)
2. 왼쪽 끝은 $x'$만의 함수이고 오른쪽 끝은 $y'$만의 함수다. 모든 $x'$에 대해 성립하므로 왼쪽에 $\max_{x'}$를 취해도 된다:
   $$\max_{x'\in X}\min_{y\in Y}f(x',y)\ \le\ \max_{x\in X}f(x,y')\qquad\forall y'\in Y$$
3. 이제 왼쪽은 상수이고 모든 $y'$에 대해 성립하므로 오른쪽에 $\min_{y'}$를 취해도 된다:
   $$\max_{x\in X}\min_{y\in Y}f(x,y)\ \le\ \min_{y'\in Y}\max_{x\in X}f(x,y')\qquad\blacksquare$$

(보충) max·min이 존재하지 않으면 sup·inf로 바꿔 같은 증명이 된다.

**반례 1 (이산)** $X=Y=\lbrace0,1\rbrace$, $f(x,y)=1$ ($x=y$), $0$ ($x\ne y$)
- $\max_x\min_yf$: $x=0$이면 $\min(f(0,0),f(0,1))=\min(1,0)=0$, $x=1$도 0 → **0**
- $\min_y\max_xf$: $y=0$이면 $\max(1,0)=1$, $y=1$도 1 → **1**
- $0<1$: 등호 불성립. (최대화하는 쪽은 상대가 맞춰 주지 않길 바라고, 최소화하는 쪽은 맞추길 바라는 "짝 맞추기 게임")

**반례 2 (연속)** $X=Y=[0,1]$, $f(x,y)=(x-y)^2$
- $\max_x\min_y(x-y)^2=\max_x0=0$ ($y=x$로 고르면 됨)
- $\min_y\max_x(x-y)^2=\min_y\max\lbrace y^2,(1-y)^2\rbrace=\frac14$ ($y=\frac12$)
- $0<\frac14$

✔ 두 변수를 고정한 사슬 부등식 · ✔ "왼쪽은 $x'$만, 오른쪽은 $y'$만의 함수"라는 말 · ✔ 반례의 두 값을 실제로 계산
✗ $\max_x$와 $\min_y$를 한꺼번에 취함 (순서가 핵심) · ✗ 반례에서 한쪽 값만 계산

**더 생각해 볼 것** 등호가 성립하는 조건(saddle point), SVM의 약한 쌍대성 직접 증명 → [연습문제 1 · 문제 7](연습문제%201.md)

---

## 5. 슬라이드에서 "증명 없이 주장만 한 것" — 다음 출제 후보

PS1 P3·P4가 9/14의 **필기 없던 슬라이드**에서 나왔다. 같은 성격의 후보를 모았다. 앞의 두 개는 PS1에 이미 나왔다.

| 주장 | 위치 | 연습문제 |
| --- | --- | --- |
| SVM 쌍대 유도와 $b=y_i-x_i^\top w$ | 9/14 p38–40 | ✅ PS1 P3 · 연습 6 |
| $\min\max\ge\max\min$, "equality holds in certain conditions" | 9/14 p37 | ✅ PS1 P4 · 연습 7 |
| KL은 삼각부등식을 만족하지 않는다 | 9/7 p33 | 연습 1(e) |
| $I(X;Y)=H(X)-H(X\mid Y)$ ("Similarly") | 9/9 p3 | 연습 2 |
| Beta prior와 posterior가 모두 Beta | 9/7 p25 | 연습 3 |
| mini-batch는 "variance가 줄어서" 더 안정적 | 9/21 W4M2 p5 | 연습 8 |
| Xavier는 zero-centered 활성화를 가정, ReLU면 붕괴 | 9/21 W4M2 p25 | 연습 9 |
| He 초기화 슬라이드의 mean·std 값 | 9/23 W4W1 p1 | 연습 9(e) |
| $\eta<2/\beta$면 감소 (얼마나 빡빡한 조건인가) | 9/23 W4W2 p10 | 연습 10 |
| softmax 기울기 $\partial p_k/\partial w_{mn}=(\delta_{mk}p_k-p_kp_m)x_{in}$ | 9/14 p22 | – (직접 유도해 볼 것) |
| BN: $\gamma=\sigma$, $\beta=\mu$면 항등함수 / 테스트 시 affine으로 fuse | 9/23 W4W1 p14, p20 | – (직접 유도해 볼 것) |
| Descent Lemma ③ $\beta$-smooth ⇒ $\nabla^2f\preceq\beta I$ | 9/23 W4W2 p9 | 9/28 수업 |

---

## 6. 풀이 도구 상자

| 도구 | 언제 | 형태 |
| --- | --- | --- |
| **정리 적용 전 전제 확인** | KL ≥ 0, Jensen 등을 새 대상에 쓸 때 | "두 인자가 확률분포다", "함수가 볼록이다"를 한 줄 쓴다 |
| **음이 아닌 것들의 합 = 0** | "⇔" 증명 | 각 항 ≥ 0 → 합 = 0이면 각 항 = 0 |
| **iff는 양방향** | 등호 조건 | (⇐)는 대개 대입 한 줄이지만 반드시 쓴다 |
| **MAP 레시피** | prior가 있는 추정 | 사후 ∝ 가능도 × prior → $-\log$ → 상수 버림 → 양수배 → 미분 = 0 → 2계 확인 |
| **더하고 빼기** | bias–variance, 분산 분해 | $\hat\theta-\theta=(\hat\theta-\mathbb E\hat\theta)+(\mathbb E\hat\theta-\theta)$ → 교차항 0 |
| **독립 + 평균 0 → 교차항 0** | 분산 계산 (Xavier·He·mini-batch) | $\mathbb E[u_iu_j]=\mathbb E[u_i]\mathbb E[u_j]=0$ ($i\ne j$) |
| **SVM 쌍대 레시피** | 작은 데이터 SVM | 라그랑주 → $\nabla_w,\partial_b$ → $g(\alpha)$ → 제약 하 최대화(내부해가 음수면 경계) → $w$, $b$ → 검산 |
| **사슬 부등식 후 한쪽씩** | max–min, 약한 쌍대성 | 모두 고정 → $\min\le f\le\max$ → 한 변수씩 max·min |
| **작은 반례** | "항상 성립하나?" | $\lbrace0,1\rbrace$ 위 표, 베르누이 분포, $(x-y)^2$ on $[0,1]^2$ |
