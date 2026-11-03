# 심수기 Quiz 2 총정리 (11/04)

> **범위 (수업 5개)**: [10/14 오차 분해](../수업정리/10.14%20수업정리.md) · [10/21 UAT](../수업정리/10.21%20수업정리.md) · [10/26 Barron 증명·Rademacher](../수업정리/10.26%20수업정리.md) · [10/28 RNN·LSTM](../수업정리/10.28%20수업정리.md) · [11/2 Attention·Transformer](../수업정리/11.02%20수업정리.md)
> **공지 (교수님)**: 근사이론 등에서 다룬 **해석학의 기본 지식에 대한 증명은 묻지 않는다** (측도, dense, Riesz 표현 정리, Hahn–Banach, Fourier 변환 등). → 정리는 **진술과 뜻**을 알고, 증명은 **확률·미분 계산으로 되는 것**에 집중한다.
> **판서 메모**: 10/28 판서 p12, 2-step RNN BPTT 계산 옆에 빨간 박스 **"Quiz 2 11/04"** (범위 설명은 없음).
> 표시: 🔴 판서로 증명·유도 · 🧮 계산 · 📋 시험 언급 · ⚠️ 헷갈리기 쉬운 것 · ✏️ 자료의 오기를 고친 것. 수치는 모두 스크립트로 검산했다.

## 0. 한 장 흐름

```
[10/14] 오차 분해      R(f̃) − R* ≤ ε_opt + 2·sup_F|R − R̂| + ε_app
          ├─ ε_app (근사)    → [10/21] UAT: 은닉층 1개 sigmoid 망은 C(I_n)에서 dense (존재)
          │                  → [10/26] Barron: f ∈ Γ_C 면 뉴런 N개로 오차 ≤ 4C²/N (차원 무관) ← Maurey 보조정리
          ├─ ε_gen (일반화)  → [10/14] Chebyshev (f 하나 고정): P(|R̂−R| ≥ cσ/√n) ≤ 1/c², PAC
          │                  → [10/26] Rademacher (모임 전체): sup ≤ 2Rad(F) + 1/√(2n), Rad ≤ Q/√n
          │                  → [10/14] Lipschitz 모임은 n ≳ ε^(−d/2): 차원의 저주
          └─ 손실 설계       → [10/14] 0-1 손실 대신 hinge. 단 hinge 최소 ≠ 0-1 최소 (📋 기말 출제 예정)

[10/28] 순서 데이터     RNN h_t = tanh(Wh_{t−1} + Ux_t + b), 모든 시점 같은 파라미터
          ├─ BPTT: 과거 시점으로 ∂h_t/∂h_{t−1} = (1−h_t²)W 를 곱하며 내려간다 (📋 Quiz 2 메모)
          ├─ 긴 곱 → exploding (clipping) / vanishing (더 큰 문제)
          └─ LSTM: ∂C_t/∂C_{t−1} = diag(f_t), f_t ≈ 1 → uninterrupted gradient flow
[11/2]  attention       캡셔닝: 단어마다 이미지 위치에 α (합 1) → z = Σα x
          ├─ softmax(QKᵀ/√d_k)V, √d_k 는 Var[qᵀk] = d_k 때문
          ├─ cross (Q ≠ K,V 출처) / self (Q,K,V 모두 X): permutation equivariant
          ├─ → positional encoding p(t) = (sin ω t, cos ω t, …)
          └─ masked: 미래는 −∞, multi-head, Transformer encoder
```

## 1. 출제 예상 순위

| 순위 | 내용 | 형태 | 근거 | 수업 |
| :---: | --- | --- | --- | --- |
| 1 | **2-step RNN BPTT**: $\partial L/\partial V,c,W,U,b$ 다섯 개 | 🔴🧮 유도·계산 | 📋 바로 옆에 "Quiz 2 11/04", 셋은 D.I.Y. | 10/28 §3 |
| 2 | **scaled dot-product attention**과 $\sqrt{d_k}$ 의 이유 ( $\text{Var}=d_k$ ) | 🔴 짧은 증명 | 판서 2쪽, 빨간 표시 최다 | 11/2 §4 |
| 3 | **Maurey 보조정리** 증명 (확률적 방법, 교차항 0) | 🔴 증명 | 판서 2쪽, 해석학이 아닌 확률 계산 | 10/26 §2 |
| 4 | **오차 분해** 증명 (더하고 빼기, 2배의 sup) | 🔴 증명 (복원) | 10/14 핵심, 10/21·10/26의 출발점 | 10/14 §2 |
| 5 | **LSTM**: 게이트 4개의 역할, $C_t$ 식, $\partial C_t/\partial C_{t-1}=\mathrm{diag}(f_t)$ vs RNN | 🔴 서술·짧은 유도 | 판서 3쪽 | 10/28 §5 |
| 6 | exploding/vanishing의 원인 (Jacobian 곱)과 **gradient clipping** | 서술·계산 | 박스·동그라미 | 10/28 §4 |
| 7 | **positional encoding**: 필요한 이유(permutation equivariance)와 $p(t)$ 계산 | 🧮 | 판서 2쪽 수치 예 | 11/2 §5–6 |
| 8 | **masked self-attention** 행렬, cross vs self | 🧮 서술 | 판서 1쪽 | 11/2 §5, §7 |
| 9 | Chebyshev·PAC 샘플 수, 차원의 저주 $n\gtrsim\varepsilon^{-d/2}$ | 🧮 | 10/14 | 10/14 §3, §5 |
| 10 | hinge loss와 0-1 손실, 반례 | 서술 | 📋 "기말 출제 예정" | 10/14 §6 |
| 11 | 정리 **진술**: UAT, discriminatory, Barron (조건 $c\gt4C_1^2$ ), Rademacher 정의 | 서술 | 정의·박스 | 10/21, 10/26 |
| 12 | 캡셔닝 cross entropy ( $-\log0.8$ ), attention 캡셔닝 shape, inductive bias 정의 | 🧮 서술 | 11/2 판서 | 11/2 §1–3 |

**묻지 않을 것 (공지)**: Riesz 표현 정리, Hahn–Banach(Lemma 3), Lemma 1(sigmoid가 discriminatory)의 증명, Lemma 4·5의 함수해석 부분, Barron Lemma 2의 Fourier 계산. 단, **UAT 증명의 흐름(귀류법)**을 말로 설명하라는 정도는 대비해 둔다.

---

## 2. 공식 한 장

| 주제 | 식 |
| --- | --- |
| 오차 분해 | $R(\tilde f)-R^\ast\le\varepsilon_{opt}+2\sup_{f\in\mathcal F}\lvert R(f)-\hat R(f)\rvert+\varepsilon_{app}$ |
| Chebyshev (f 고정) | $\text{Var}(\hat R(f)-R(f))=\sigma^2/n$ , $P(\lvert\hat R-R\rvert\ge c\sigma/\sqrt n)\le1/c^2$ , 또는 $\le\frac{\sigma^2}{n\varepsilon^2}$ |
| 차원의 저주 | $\lvert\hat f(x)-f^\ast(x)\rvert\le2L\lVert x-x_{nn}\rVert$ , $\mathbb E\lVert x-x_{nn}\rVert^2\propto n^{-2/d}$ → $n\gtrsim\varepsilon^{-d/2}$ |
| hinge | $\max(1-y\thinspace g(x),\thinspace0)$ , 0-1 손실은 $y\thinspace g(x)\le0$ 이면 1 |
| UAT | $\sum_j\alpha_j\sigma(w_j^\top x+\theta_j)$ 들은 $C(I_n)$ 에서 dense |
| discriminatory | $\int\sigma(w^\top x+\theta)\thinspace d\mu=0\ \forall w,\theta$ ⇒ $\mu=0$ |
| Maurey | $\lVert g\rVert\le B$ , $f\in\overline{co}(G)$ ⇒ $\lVert f-\frac1N\sum g_i\rVert^2\le c/N$ ( $c\gt B^2$ ) |
| Barron | $f\in\Gamma_{C_1}$ ⇒ $\frac1{\lvert B_1^d\rvert}\int_{B_1^d}\lvert f-f_N\rvert^2\le c/N$ ( $c\gt4C_1^2$ ), $B=2C_1$ |
| Rademacher | $\text{Rad}_ n(\mathcal F)=\mathbb E[\sup_f\frac1n\sum\xi_if(z_i)]$ , $\sup_f\lvert\frac1n\sum f(x_i)-\mathbb Ef\rvert\le2\text{Rad}+\frac1{\sqrt{2n}}$ |
| RNN | $h_t=\tanh(Wh_{t-1}+Ux_t+b)$ , $Y_t=Vh_t+c$ , $\frac{\partial h_t}{\partial h_{t-1}}=(1-h_t^2)W$ |
| 기울기 곱 | $\frac{\partial\mathcal L}{\partial h_\ell}=\frac{\partial\mathcal L}{\partial h_L}\frac{\partial h_L}{\partial h_{L-1}}\cdots\frac{\partial h_{\ell+1}}{\partial h_\ell}$ |
| clipping ✏️ | $g\leftarrow\min(1,\thinspace v/\lVert g\rVert)\thinspace g$ |
| LSTM | $C_t=f_t\odot C_{t-1}+i_t\odot g_t$ , $h_t=o_t\odot\tanh C_t$ , $\frac{\partial C_t}{\partial C_{t-1}}=\mathrm{diag}(f_t)$ |
| attention | $\text{softmax}(QK^\top/\sqrt{d_k})V$ , $Q=xW_q$ ( $n\times d_k$ ), $A\in\mathbb R^{n\times n}$ , $\text{Var}[q^\top k]=d_k$ |
| equivariance | $\text{Attn}(PX)=P\thinspace\text{Attn}(X)$ |
| positional enc. | $p(t)=(\sin\omega_kt,\cos\omega_kt)_ k$ , $\omega_k=10000^{-2k/d}$ ( $k=0,1,\dots$ ), $z_j=x_j+p_j$ |
| mask | $e_{ij}=q_j^\top k_i/\sqrt{d_k}$ ( $i\le j$ ), $-\infty$ ( $i\gt j$ ) |

---

## 3. 🔴 증명·유도 체크리스트 (뼈대만 보고 백지에)

| ☐ | 무엇 | 뼈대 | 노트 |
| --- | --- | --- | --- |
| ☐ | 2-step BPTT 다섯 기울기 | forward 6줄 → 국소 미분 표 ( $\partial h_2/\partial h_1=(1-h_2^2)W$ ) → $V,c$ 는 시점별 합 → $W,U,b$ 는 **직접 + 시간 경로** | 10/28 §3 |
| ☐ | $\sqrt{d_k}$ 의 이유 | $z=\sum_mq_mk_m$ → 평균 0 → $\text{Var}(q_mk_m)=\mathbb E q_m^2\mathbb Ek_m^2=1$ → 독립 합 $d_k$ → 나누면 1 → softmax가 몰리지 않음 | 11/2 §4 |
| ☐ | Maurey | $f^\ast=\sum\alpha_jh_j$ → i.i.d. $X_i$ ( $P[X_i=h_j]=\alpha_j$ ) → $\mathbb EX=f^\ast$ → 전개 → 교차항 0 → $\frac1N\mathbb E[\lVert X\rVert^2-\lVert f^\ast\rVert^2]\le\frac{B^2}N$ → 실현값 존재 → 삼각부등식 | 10/26 §2 |
| ☐ | 오차 분해 | $R(\tilde f)-R^\ast$ 에 $\hat R(\tilde f)$, $\hat R(\hat f_n)$, $\hat R(f_{\mathcal F})$ 를 더하고 빼 5괄호 → ERM 괄호 $\le0$ → 두 괄호를 sup로 | 10/14 §2 |
| ☐ | Chebyshev 일반화 | $Z_i=\ell_i-R(f)$ i.i.d. 평균 0 → 표본평균 분산 $\sigma^2/n$ → Chebyshev | 10/14 §3 |
| ☐ | Lipschitz 상계 | 최근접 샘플 $x_{nn}$ → 삼각부등식 3항 (가운데 0) → $2L\lVert x-x_{nn}\rVert$ | 10/14 §5 |
| ☐ | LSTM cell 기울기 | $C_t=f\odot C_{t-1}+\cdots$ 를 $C_{t-1}$ 로 미분 → $\mathrm{diag}(f_t)$ vs $(1-h_t^2)W_h$ | 10/28 §5.3 |
| ☐ | permutation equivariance | $Q,K,V\to PQ,PK,PV$ → $\text{softmax}(PAP^\top)=P\thinspace\text{softmax}(A)P^\top$ → $P^\top P=I$ | 11/2 §5 |
| ☐ | Barron 증명의 흐름 (말로) | Fourier로 $f-f(0)$ = 계단 뉴런들의 **기댓값** → Maurey로 $N$ 개 평균, $B=2C_1$ → $4C_1^2/N$ → 계단 → sigmoid | 10/26 §3 |
| ☐ | UAT 증명의 흐름 (말로) | dense 아님 가정 → (Hahn–Banach + Riesz) 모든 뉴런을 0으로 재는 $\mu\ne0$ → discriminatory라 $\mu=0$ → 모순 | 10/21 §4 |

---

## 4. 예상 문제

### 문제 1. 2-step RNN의 BPTT (🔴📋)

$h_t=\tanh(Wh_{t-1}+Ux_t+b)$ , $Y_t=Vh_t+c$ , $L=\frac12(Y_1-Z_1)^2+\frac12(Y_2-Z_2)^2$ , 모든 변수는 실수.

(a) $\frac{\partial L}{\partial V}$ , $\frac{\partial L}{\partial c}$ , $\frac{\partial L}{\partial W}$ , $\frac{\partial L}{\partial U}$ , $\frac{\partial L}{\partial b}$ 를 $h_t$ , $Y_t-Z_t$ , $X_t$ 로 써라.
(b) $h_0=0$, $W=0.5$, $U=1$, $V=2$, $b=c=0$, $X=(1,\thinspace0.5)$, $Z=(1,\thinspace1)$ 일 때 값을 구하라.

<details><summary>풀이</summary>

(a) $r_t=Y_t-Z_t$ 라 하면

$$\frac{\partial L}{\partial V}=r_1h_1+r_2h_2,\qquad\frac{\partial L}{\partial c}=r_1+r_2$$

$$\frac{\partial L}{\partial W}=r_1V(1-h_1^2)h_0+r_2V(1-h_2^2)h_1+r_2V(1-h_2^2)\thinspace W(1-h_1^2)h_0$$

$$\frac{\partial L}{\partial U}=r_1V(1-h_1^2)X_1+r_2V(1-h_2^2)\big[X_2+W(1-h_1^2)X_1\big]$$

$$\frac{\partial L}{\partial b}=r_1V(1-h_1^2)+r_2V(1-h_2^2)\big[1+W(1-h_1^2)\big]$$

핵심: $L_2$ 는 $h_2$ 에 직접, 그리고 $\frac{\partial h_2}{\partial h_1}=(1-h_2^2)W$ 를 거쳐 $h_1$ 쪽으로도 의존한다.

(b) $h_1=\tanh1=0.7616$, $Y_1=1.5232$, $h_2=\tanh(0.5\cdot0.7616+0.5)=0.7068$, $Y_2=1.4136$, $r_1=0.5232$, $r_2=0.4136$, $1-h_1^2=0.4200$, $1-h_2^2=0.5004$

| | $V$ | $c$ | $W$ | $U$ | $b$ |
| --- | --- | --- | --- | --- | --- |
| 값 | 0.6908 | 0.9368 | 0.3153 | 0.7334 | 0.9404 |

( $h_0=0$ 이라 $\partial L/\partial W$ 는 가운데 항 $r_2V(1-h_2^2)h_1$ 만 남는다. 다섯 값 모두 수치 미분과 일치.)
</details>

### 문제 2. Scaled dot-product attention (🔴🧮)

(a) $q,k\in\mathbb R^{d_k}$ 의 성분이 모두 독립인 $N(0,1)$ 일 때 $\mathbb E[q^\top k]$ 와 $\text{Var}[q^\top k]$ 를 구하고, attention 점수를 $\sqrt{d_k}$ 로 나누는 이유를 설명하라.
(b) $Q$ 의 행 $q_1=(1,0)$, $q_2=(0,1)$ , $K$ 의 행 $k_1=(1,1)$, $k_2=(1,-1)$ , $V$ 의 행 $v_1=(1,0)$, $v_2=(0,2)$ , $d_k=2$ 일 때 $\text{softmax}(QK^\top/\sqrt{d_k})V$ 를 구하라.
(c) (b)에 mask를 씌워 query 1이 key 1만 보게 하면 출력의 첫 행은?

<details><summary>풀이</summary>

(a) $\mathbb E[q_mk_m]=\mathbb Eq_m\mathbb Ek_m=0$ → $\mathbb E[q^\top k]=0$ . $\text{Var}(q_mk_m)=\mathbb E[q_m^2]\mathbb E[k_m^2]=1$ , 독립인 $d_k$ 개의 합이라 $\text{Var}[q^\top k]=d_k$ . 차원이 크면 점수의 분산이 커져 softmax가 가장 큰 점수 하나에 몰리고(나머지 가중치·기울기 ≈ 0), $\sqrt{d_k}$ 로 나누면 분산이 차원과 무관하게 1이 된다.

(b) $QK^\top/\sqrt2$ 의 1행 $=(0.707,\thinspace0.707)$ , 2행 $=(0.707,\thinspace-0.707)$ → 행마다 softmax: 1행 $(0.5,\thinspace0.5)$ , 2행 $(0.804,\thinspace0.196)$ → 출력 1행 $0.5v_1+0.5v_2=(0.5,\thinspace1)$ , 2행 $0.804v_1+0.196v_2=(0.804,\thinspace0.391)$ .

(c) $e_{12}=-\infty$ → 1행 가중치 $(1,0)$ → 출력 $v_1=(1,0)$ . 2행은 그대로.
</details>

### 문제 3. Maurey 보조정리 (🔴)

$H$ 가 Hilbert 공간, $G\subset H$ 의 모든 원소가 $\lVert g\rVert\le B$ 이고 $f^\ast=\sum_{j=1}^m\alpha_jh_j$ ( $h_j\in G$ , $\alpha_j\ge0$ , $\sum\alpha_j=1$ ) 이다. 각각 $G$ 에 속하는 $g_1,\dots,g_N$ 이 있어 $\lVert f^\ast-\frac1N\sum_{i=1}^Ng_i\rVert^2\le\frac{B^2}N$ 임을 보여라.

<details><summary>풀이</summary>

1. $X_1,\dots,X_N$ i.i.d., $P[X_i=h_j]=\alpha_j$ → $\mathbb E[X_i]=\sum_j\alpha_jh_j=f^\ast$
2. $\mathbb E\lVert f^\ast-\frac1N\sum X_i\rVert^2=\frac1{N^2}\mathbb E\big[\sum_i\lVert f^\ast-X_i\rVert^2+\sum_{i\ne j}\langle f^\ast-X_i,f^\ast-X_j\rangle\big]$
3. $i\ne j$ : 독립이라 $\mathbb E\langle f^\ast-X_i,f^\ast-X_j\rangle=\langle f^\ast-\mathbb EX_i,f^\ast-\mathbb EX_j\rangle=0$
4. 같은 분포라 $=\frac1N\mathbb E\lVert f^\ast-X_1\rVert^2=\frac1N\big(\mathbb E\lVert X_1\rVert^2-\lVert f^\ast\rVert^2\big)\le\frac{B^2}N$
5. 기댓값이 $B^2/N$ 이하이므로 그 이하인 실현값 $X_i=g_i\in G$ 가 존재한다 ∎

(Barron: $G$ = 계단 뉴런 $\gamma\mathbb 1(\langle a,x\rangle+b)$ , $\lvert\gamma\rvert\le2C_1$ → $B=2C_1$ → $4C_1^2/N$ . 예: $C_1=1.5$ 이면 오차 $\le9/N$ , 0.01 이하가 되려면 $N\ge900$ .)
</details>

### 문제 4. 오차 분해 (🔴)

$f_{\mathcal F}=\arg\min_{\mathcal F}R$ , $\hat f_n=\arg\min_{\mathcal F}\hat R$ , 학습된 $\tilde f\in\mathcal F$ 에 대해 $R(\tilde f)-R^\ast\le\varepsilon_{opt}+2\sup_{f\in\mathcal F}\lvert R(f)-\hat R(f)\rvert+\varepsilon_{app}$ 을 보이고, 세 항이 각각 무엇에 의존하는지 써라.

<details><summary>풀이</summary>

$$R(\tilde f)-R^\ast=[R(\tilde f)-\hat R(\tilde f)]+[\hat R(\tilde f)-\hat R(\hat f_n)]+[\hat R(\hat f_n)-\hat R(f_{\mathcal F})]+[\hat R(f_{\mathcal F})-R(f_{\mathcal F})]+[R(f_{\mathcal F})-R^\ast]$$

셋째 $\le0$ ( $\hat f_n$ 이 $\hat R$ 의 최소), 첫째·넷째 $\le\sup\lvert R-\hat R\rvert$ , 둘째 = $\varepsilon_{opt}$ , 다섯째 = $\varepsilon_{app}$ ∎
근사: 가설 공간만(학습과 무관) → UAT·Barron. 최적화: 샘플과 $\mathcal F$ 가 정해지면 분포와 무관 → GD·SGD. 일반화: 샘플 수 $n$ 과 분포 → Chebyshev·Rademacher.
</details>

### 문제 5. LSTM과 vanishing gradient (🔴🧮)

(a) LSTM의 네 게이트의 역할을 쓰고 $C_t$ , $h_t$ 의 식을 써라.
(b) $f_t=0.9$, $i_t=0.5$, $g_t=0.8$, $o_t=1$, $C_{t-1}=2$ 일 때 $C_t$ , $h_t$ 는?
(c) $\partial C_t/\partial C_{t-1}$ 을 구하고 vanilla RNN의 $\partial h_t/\partial h_{t-1}$ 과 비교하라. $f_t$ 가 계속 0.9이면 50스텝 뒤 cell 경로의 기울기 배율은? 0.99이면?

<details><summary>풀이</summary>

(a) $i$ (input): 새 내용 $g_t$ 를 cell에 얼마나 쓸까, $f$ (forget): 이전 cell $C_{t-1}$ 을 얼마나 남길까, $o$ (output): cell을 $h_t$ 에 얼마나 내보낼까 — 셋은 sigmoid. $g$ : 새로 쓸 내용, tanh. $C_t=f_t\odot C_{t-1}+i_t\odot g_t$ , $h_t=o_t\odot\tanh C_t$ .
(b) $C_t=0.9\cdot2+0.5\cdot0.8=2.2$ , $h_t=\tanh2.2\approx0.976$
(c) $\mathrm{diag}(f_t)$ vs $(1-h_t^2)W_h$ . LSTM은 행렬 $W_h$ 를 반복해 곱하지 않고 forget 값만 원소별로 곱한다. $0.9^{50}\approx0.005$ , $0.99^{50}\approx0.61$ → $f_t\approx1$ 이면 기울기가 거의 그대로 흐른다 (uninterrupted gradient flow). 비교: 스칼라 RNN에서 $(1-h^2)W\approx0.5$ 이면 20스텝 뒤 $0.5^{20}\approx10^{-6}$ .
</details>

### 문제 6. Gradient clipping과 exploding (🧮)

(a) 왜 RNN에서 exploding/vanishing이 생기나?
(b) 임계값 $v$ 의 gradient clipping 식을 쓰고, $g=(3,4)$ 일 때 $v=1$ 과 $v=10$ 의 결과를 구하라.

<details><summary>풀이</summary>

(a) $\frac{\partial\mathcal L}{\partial h_\ell}$ 이 Jacobian $\frac{\partial h_{k+1}}{\partial h_k}$ 의 긴 곱(시퀀스 길이 수백~수천)이라, 대부분 1보다 크면 폭발, 작으면 소멸한다. 같은 $W$ 를 반복해서 곱하는 구조 때문이다.
(b) ✏️ $g\leftarrow\min(1,v/\lVert g\rVert)g$ (슬라이드의 $\max$ 는 오기). $\lVert g\rVert=5$ . $v=1$ : $\frac15(3,4)=(0.6,\thinspace0.8)$ — 방향은 같고 크기만 1. $v=10$ : $5\le10$ 이라 그대로 $(3,4)$ .
</details>

### 문제 7. Positional encoding과 permutation equivariance (🧮)

(a) self-attention이 permutation equivariant임을 보이고, 이 때문에 무엇이 필요한지 설명하라.
(b) $d=4$, 주파수 $\omega=(1,\thinspace0.01)$ , $p(t)=(\sin t,\cos t,\sin0.01t,\cos0.01t)$ 일 때 $p(0)$, $p(1)$, $p(3)$ 을 구하라.

<details><summary>풀이</summary>

(a) 순열 행렬 $P$ 로 입력을 섞으면 $Q,K,V\to PQ,PK,PV$ 이고 $\text{softmax}(PQK^\top P^\top/\sqrt{d_k})PV=P\thinspace\text{softmax}(QK^\top/\sqrt{d_k})P^\top PV=P\cdot\text{Attn}$ . 입력 순서를 바꿔도 출력은 같은 순서로 바뀔 뿐이라 "man bites dog"와 "dog bites man"을 구별하지 못한다 → 위치 벡터를 더하는 positional encoding이 필요하다.
(b) $p(0)=(0,1,0,1)$ , $p(1)\approx(0.841,\thinspace0.540,\thinspace0.010,\thinspace1.000)$ , $p(3)\approx(0.141,\thinspace-0.990,\thinspace0.030,\thinspace1.000)$ . 앞 두 성분(고주파)은 빨리, 뒤 두 성분(저주파)은 천천히 변한다 (이진수의 낮은 자리·높은 자리).
</details>

### 문제 8. 일반화와 차원의 저주 (🧮)

(a) $f$ 를 고정하고 손실의 분산이 $\sigma^2=4$ 일 때, Chebyshev로 "95% 확률로 $\lvert\hat R(f)-R(f)\rvert\lt0.1$ "을 보장하는 샘플 수는?
(b) Lipschitz target, 입력 차원 $d=20$ 에서 $\mathbb E\lVert x-x_{nn}\rVert^2\approx n^{-2/d}\le\varepsilon=0.01$ 이 되려면?
(c) Barron 정리의 속도와 비교하라.

<details><summary>풀이</summary>

(a) $P\le\frac{\sigma^2}{n\varepsilon^2}\le0.05$ → $n\ge\frac4{0.05\cdot0.01}=8000$
(b) $n\gtrsim\varepsilon^{-d/2}=0.01^{-10}=10^{20}$
(c) Barron: 근사 오차 $\le c/N$ 으로 **차원 $d$ 가 없다**. 함수 모임을 Barron class처럼 좁히면 차원의 저주를 피한다. (일반화 쪽은 Rademacher로 $O(1/\sqrt n)$ , 가중치 크기 $\lVert f\rVert\le Q$ 가 좌우.)
</details>

### 문제 9. Hinge loss (📋 기말 예정)

0-1 손실을 학습에 쓰지 않는 이유와 hinge loss의 두 효과를 쓰고, "hinge 리스크 최소 모델 ≠ 0-1 리스크 최소 모델"인 예를 들어라.

<details><summary>풀이</summary>

0-1 손실은 불연속이고 기울기가 거의 0이라 GD를 쓸 수 없다. hinge $\max(1-yg(x),0)$ 는 ① 크게 틀릴수록 벌점이 크고 ② 아슬아슬하게 맞아도( $0\lt yg\lt1$ ) 벌점을 줘 더 확실히 맞추게 한다.
예: $P(x_1)=0.99$, $P(x_2)=0.01$ , 둘 다 레이블 $+1$ . GA: $g=0.1$ (둘 다) → hinge 0.9, 0-1 위험 0. GB: $g(x_1)=1$, $g(x_2)=-0.5$ → hinge 0.015, 0-1 위험 0.01. hinge는 GB, 0-1은 GA를 고른다.
</details>

### 문제 10. 짧은 답

<details><summary>(a) discriminatory의 정의</summary>모든 w, θ에서 ∫σ(w<sup>T</sup>x + θ)dμ = 0이면 μ = 0</details>
<details><summary>(b) UAT가 보장하지 않는 것</summary>필요한 뉴런 수, 그 가중치를 GD로 찾을 수 있는지(opt), 데이터로 배울 수 있는지(gen)</details>
<details><summary>(c) Rademacher complexity의 정의와 뜻</summary>E[sup<sub>f</sub> (1/n)Σξ<sub>i</sub>f(z<sub>i</sub>)], ξ<sub>i</sub> = ±1 반반. 함수 모임이 무작위 부호에 얼마나 맞출 수 있나(복잡도). 함수가 하나뿐이면 0</details>
<details><summary>(d) cross-attention과 self-attention</summary>cross: Q는 decoder, K·V는 입력 X. self: Q, K, V 모두 X</details>
<details><summary>(e) masked self-attention에서 query j는 어디까지 보나</summary>key 위치 0, …, j. i &gt; j는 −∞ → softmax 후 0 (미래 단어를 미리 보면 정답을 베끼는 것)</details>
<details><summary>(f) inductive bias의 정의와 RNN의 inductive bias</summary>보지 못한 입력의 출력을 예측하려고 학습자가 쓰는 가정. RNN: 1D 사슬 구조 + first-order Markov assumption</details>
<details><summary>(g) attention 캡셔닝의 α<sup>(t)</sup>와 z<sup>(t)</sup></summary>α<sup>(t)</sup> = softmax(f<sub>att</sub>(X, h<sub>t−1</sub>)) ∈ ℝ<sup>14×14</sup>, 합 1, f<sub>att</sub> = MLP. z<sup>(t)</sup> = Σα<sub>ij</sub>X<sub>ij</sub> ∈ ℝ<sup>512</sup></details>
<details><summary>(h) 캡셔닝 학습: 정답 확률이 0.8이면 그 시점 손실은</summary>−log 0.8 ≈ 0.223 (one-hot이라 정답 단어의 −log 확률만 남음)</details>
<details><summary>(i) SENet의 channel attention은 합이 1이어야 하나</summary>아니다. sigmoid라 채널마다 (0,1). 공간 soft attention은 softmax라 합 1</details>

---

## 5. ⚠️ 자주 틀리는 것 (자료의 오기 포함)

- ⚠️ **$W$ 와 $U$ 의 역할**이 자료마다 다르다: 슬라이드 $h_t=\sigma(Uh_{t-1}+Wx_t+b)$ , 판서 $h_t=\tanh(Wh_{t-1}+Ux_t+b)$ . 답안에 식을 먼저 써서 정의할 것.
- ✏️ BPTT 판서의 $\partial L/\partial W$ 셋째 항 $(1-h_0^2)$ → **$(1-h_2^2)$** .
- ⚠️ $\partial h_2/\partial W=(1-h_2^2)h_1$ 은 $h_1$ 을 고정한 **직접 미분**. 시간 경로 $\frac{\partial h_2}{\partial h_1}\frac{\partial h_1}{\partial W}$ 를 빠뜨리지 말 것.
- ✏️ clipping은 $\max$ 가 아니라 **$\min(1,v/\lVert g\rVert)$** .
- ✏️ forget gate가 곱해지는 것은 $C_t$ 가 아니라 **$C_{t-1}$** .
- ✏️ Barron 조건은 $c\gt4C_1$ (10/26 p1), $4C_1^3$ (10/21)이 아니라 **$c\gt4C_1^2$** (증명 결론).
- ⚠️ Chebyshev 임계값은 $c\sigma/\sqrt n$ (요약의 $\sqrt{c/n}\sigma$ 는 오기), 차원의 저주는 $\varepsilon^{-d/2}$ .
- ⚠️ softmax 축: $QK^\top$ (행 = query)이면 **행마다**. 슬라이드의 $e_{i,j}$ ( $i$ = key)는 그 전치라 열 방향. mask도 판서 $E$ 에서는 대각 **아래**, $QK^\top$ 에서는 대각 **위**가 $-\infty$ .
- ⚠️ 분모는 $\sqrt{d_k}$ (key 차원). 슬라이드 일부의 $\sqrt d$, $\sqrt D$ 는 $d_k=d$ 일 때만 같다.
- ⚠️ positional encoding은 판서·원 논문대로 **더하기** $x_j+p_j$ (슬라이드 "concatenate"). $\omega_k=10000^{-2k/d}$ 는 $k=0$ 부터 ( $\omega=1$ 이 첫 주파수).
- ⚠️ single-query attention은 permutation **invariant**, self-attention은 **equivariant**.
- ⚠️ 11/2 판서의 "self-attention 슬라이드 위 캡셔닝 예"(Q = 문장, K·V = 이미지)는 **cross-attention**이다.
