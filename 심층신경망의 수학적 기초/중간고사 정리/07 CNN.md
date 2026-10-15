# 07 CNN — 합성곱 층 · 출력 크기 · Pooling · Equivariance · AlexNet에서 ResNet까지

> **수업** 9/28 (CNN 기초) · 10/7 (CNN 구조) → 날짜별 원본: [09.28 CNN 기초](../수업정리/09.28%20수업정리%202%20-%20CNN%20기초.md) · [10.07](../수업정리/10.07%20수업정리.md)
> **이 장의 시험 포인트** (두 회차 모두 필기 없음) 🧮 출력 크기·파라미터 수·합성곱 손계산 · equivariance 정의 · 깊은 망의 최적화 문제와 residual connection · 1×1 conv bottleneck
> **연습** [연습문제 2](../문제풀이/연습문제%202.md) 문제 7–10
> 표시: 🔴 수업에서 판서로 증명한 것 · 🧮 숫자로 풀어 본 예제 · 📝 Problem Set 1에 나온 것 · ⚠️ 헷갈리기 쉬운 것 · (보충) 수업 밖 설명

## 한눈에

- FC 층은 파라미터가 너무 많고, 이미지를 한 줄로 펴서 **공간 구조**를 버리며, 이동에 대한 성질을 담지 못한다.
- **합성곱 층 = 지역 연결 + 가중치 공유**: 작은 필터를 모든 위치에 똑같이 적용한다 → 파라미터가 적고, 이동에 **equivariant**하다.
- **출력 크기** $N'=\dfrac{N-K+2P}S+1$, 출력 채널 수 = 필터 개수. **파라미터** = $(K\cdot K\cdot C+1)\times D$
- 구조의 흐름: AlexNet → ZFNet → VGG(작은 필터로 깊게) → GoogLeNet(1×1 conv로 싸게) → **ResNet**(깊은 망을 학습되게: $F(x)+x$ )
- 깊은 plain 망은 **training error도 높다** → 과적합이 아니라 **최적화 문제**.

| 기호 | 뜻 |
| --- | --- |
| $N$ | 입력의 가로(세로) 크기 |
| $K$ (또는 $F$ ) | 필터 크기 |
| $S$, $P$ | stride, padding |
| $C$ | 입력 채널 수 |
| $D$ | 필터 개수 = 출력 채널 수 |

---

## 7.1 왜 CNN인가

| FC 층 | |
| --- | --- |
| 장점 | 단순하다. 이론상 매우 일반적이다 (충분히 큰 MLP는 어떤 함수든 배울 수 있다) |
| 단점 | **파라미터가 너무 많다.** shift equivariance/invariance를 담지 못해 **inductive bias가 나쁘다** |

- 이미지는 차원이 매우 크고 변형(위치, 모양)이 많다. FC는 이미지를 **한 줄로 펴서(flatten)** 넣으므로 공간 정보를 못 쓴다.
- 비전의 많은 과제는 **이동에 equivariant/invariant**하다: 고양이를 옮겨도 여전히 고양이 (위치만 바뀐다).
- 픽셀은 이웃끼리 의존한다 → 은닉 뉴런을 **작은 입력 패치에만** 연결한다 (locally connected).

**정의 (equivariant vs invariant)** 변환들의 모임 $G$의 모든 $g$에 대해

$$\text{equivariant: }f(g\cdot x)=g\cdot f(x)\qquad\qquad\text{invariant: }f(g\cdot x)=f(x)$$

- $f$ = 합성곱, $g$ = 이동이면 **equivariant**: 물체를 옮기면 feature map도 같이 옮겨진다.
- invariant(출력이 아예 안 바뀜)는 분류 결과에 해당하고, pooling·global average pooling이 그쪽을 돕는다.

## 7.2 합성곱 층

**2D 합성곱 = 창을 옮겨 가며 가중 평균** 10×10 이미지(값 0 또는 90)에 3×3 **평균 필터**를 한 칸씩 옮기며 적용하면, 창 안에 90이 하나면 $90/9=10$, 셋이면 30 … → 경계가 **흐려진다**. 출력은 $10-3+1=8$ 이라 8×8. 필터가 5×5면 더 넓게 평균해 더 흐려지고 출력은 6×6. 합성곱 층은 이 **필터 값을 학습**한다.

- 입력 32×32×3을 펴지 않고 **공간 구조를 유지**한다.
- 필터 5×5×3 — **필터는 항상 입력의 깊이 전체를 덮는다**.
- 필터를 이미지 위에서 밀며 **내적**: 한 위치의 출력 = $w^\top x+b$ ( $5\cdot5\cdot3=75$차원 내적 + bias)
- 모든 위치에서 계산한 결과가 **activation map** 한 장 (28×28×1). 필터가 $D$개면 $D$장 → 28×28×D
- 쌓기: 32×32×3 → CONV, ReLU (5×5×3 필터 6개) → 28×28×6 → CONV, ReLU (5×5×**6** 필터 10개) → 24×24×10 → …

🧮 **파라미터 수**
- 필터 하나: $5\times5\times3+1=76$ (FC면 $3072\times$ 출력 수)
- 5×5×3 필터 6개: $6\times(75+1)=$ **456**
- 둘째 층 5×5×6 필터 10개: $10\times(150+1)=1510$
- (보충) 같은 28×28×6 출력을 FC로 만들면 $3072\times4704\approx1.4\times10^7$개

### 출력 크기

$$N'=\frac{N-K+2P}{S}+1,\qquad C'=D$$

- 나누어떨어지지 않으면 내림: $\lfloor(N+2P-K)/S\rfloor+1$
- **same padding** (크기 유지): $S=1$, 홀수 $K$일 때 $P=\frac{K-1}2$ ( $K=3$ → 1, $K=5$ → 2, $K=7$ → 3)

🧮 **출력 크기 예제**

| 입력 | 설정 | 계산 | 출력 |
| --- | --- | --- | --- |
| 7×7 | $K=3$, $S=1$ | $(7-3)/1+1$ | 5×5 |
| 7×7 | $K=3$, $S=2$ | $(7-3)/2+1$ | 3×3 |
| 7×7 | $K=3$, $S=3$ | $(7-3)/3+1=2.33$ | 맞지 않는다 |
| 7×7 | $K=3$, $S=1$, $P=1$ | $(7-3+2)/1+1$ | 7×7 |
| 52×52×128 | $K=3$, $S=1$, $P=1$, 필터 256 | $(52-3+2)/1+1$ | 52×52×256 |
| 52×52×128 | $K=4$, $S=2$, $P=1$, 필터 256 | $(52-4+2)/2+1$ | 26×26×256 |

### 🧮 합성곱 손계산

**예제 1** 5×5 이미지, 3×3 필터, stride 1 — 창 안의 원소별 곱을 더한다.

$$\text{Image}=\begin{pmatrix}2&4&9&1&4\cr2&1&4&4&6\cr1&1&2&9&2\cr7&3&5&1&3\cr2&3&4&8&5\end{pmatrix},\qquad\text{Filter}=\begin{pmatrix}1&2&3\cr-4&7&4\cr2&-5&1\end{pmatrix}\qquad\Longrightarrow\qquad\begin{pmatrix}51&66&20\cr31&49&101\cr15&53&-2\end{pmatrix}$$

- 왼쪽 위: $2\cdot1+4\cdot2+9\cdot3+2\cdot(-4)+1\cdot7+4\cdot4+1\cdot2+1\cdot(-5)+2\cdot1=51$
- zero padding 1을 주면 출력이 5×5가 되고(첫 값 21), 가운데 3×3은 위 결과와 같다.
- 필터 값이 **gradient descent로 학습하는 가중치**다.
- (보충) 필터를 뒤집지 않고 곱한다 (수학의 convolution은 뒤집는다 — 딥러닝은 이 cross-correlation을 convolution이라 부른다).

**예제 2** 4×4 입력, 3×3 필터 두 개 → feature 두 장 (각 2×2)

$$\text{Input}=\begin{pmatrix}1&0&3&1\cr0&-1&2&0\cr2&0&4&2\cr0&3&0&1\end{pmatrix},\ \ \text{F}_ 1=\begin{pmatrix}1&1&0\cr0&1&-1\cr0&0&0\end{pmatrix},\ \ \text{F}_ 2=\begin{pmatrix}-1&1&0\cr0&-1&0\cr-1&1&0\end{pmatrix}\ \Longrightarrow\ \begin{pmatrix}-2&5\cr-5&3\end{pmatrix},\ \begin{pmatrix}-2&5\cr2&-4\end{pmatrix}$$

⚠️ 슬라이드는 Feature 2의 오른쪽 아래를 −5로 적었지만 계산하면 **−4**다.

### 1×1 convolution

- 56×56×64에 1×1 필터 32개 → 56×56×32. 필터 하나는 1×1×64이고 64차원 내적을 한다.
- 각 픽셀의 $C$차원 벡터를 $K$차원으로 보내는 **픽셀별 임베딩**이다 ( $K\lt C$면 차원 축소). 모든 픽셀이 같은 가중치를 쓰므로, 전체를 펴서 넣는 FC와는 다르다.

### Pooling

- 표현을 작게 만든다(downsampling). **채널마다 따로** 적용한다: 224×224×64 → 112×112×64
- 학습할 **파라미터가 없다**.

🧮 2×2 창, stride 2

$$\begin{pmatrix}5&10&6&7\cr4&0&4&3\cr2&4&3&4\cr0&8&1&6\end{pmatrix}\ \longrightarrow\ \text{average: }\begin{pmatrix}4.75&5\cr3.5&3.5\end{pmatrix},\qquad\text{max: }\begin{pmatrix}10&7\cr8&6\end{pmatrix}$$

- **Global average pooling**: 채널마다 전체 평균 하나만 남긴다 (6×6×3 → 숫자 3개).

### FC 층과 비교

| | FC 층 | 합성곱 층 |
| --- | --- | --- |
| 연결 | 모든 노드와 연결 | **지역적으로만** (이웃 크기 = kernel 크기) |
| 가중치 | 연결마다 따로 | **모든 위치가 공유** |

합성곱 층은 대부분의 가중치를 0으로 두고(지역 연결) 남은 가중치를 묶은(공유) FC 층이다.

### (보충) 합성곱이 이동에 equivariant함을 보이기 — 경계는 무시

- 합성곱 $(w\star x)[i,j]=\sum_{a,b}w[a,b]\thinspace x[i+a,\thinspace j+b]$, 이동 $(T_{p,q}x)[i,j]=x[i-p,\thinspace j-q]$

$$(w\star T_{p,q}x)[i,j]=\sum_{a,b}w[a,b]\thinspace x[i+a-p,\thinspace j+b-q]=(w\star x)[i-p,\thinspace j-q]=\big(T_{p,q}(w\star x)\big)[i,j]$$

- 핵심은 **모든 위치가 같은 $w$를 쓴다**는 것(가중치 공유). ReLU는 원소별이라 이동과 순서를 바꿔도 같다 → CONV–ReLU를 쌓아도 equivariant.

## 7.3 CNN 구조

**CNN 분류기의 뼈대** INPUT → [CONV + ReLU → POOL]을 반복 (**feature learning**) → FLATTEN → FC → SOFTMAX (**classification**). CNN의 층 종류는 합성곱 층, pooling 층, FC 층 세 가지다.
- 역사: **LeNet-5** (LeCun, Bottou, Bengio, Haffner 1998, 문서 인식) — 합성곱 → subsampling → FC 구조의 원조

### ImageNet top-5 오류의 흐름

| 연도 | 모델 | 오류 | 층 | 한 줄 |
| --- | --- | --- | --- | --- |
| 2010–11 | (딥러닝 이전) | 28.2%, 25.8% | shallow | |
| 2012 | **AlexNet** | 16.4% | 8 | 비전에서 딥러닝 시대를 연 첫 CNN |
| 2013 | **ZFNet** | 11.7% | 8 | AlexNet의 숫자를 더 잘 조정 |
| 2014 | **VGGNet** | 7.3% | 19 | 작은 필터로 더 깊게 |
| 2014 | **GoogLeNet** | 6.7% | 22 | 더 깊고 훨씬 효율적 (FC 층 없음) |
| 2015 | **ResNet** | 3.6% | 152 | 깊이의 혁명 |

### AlexNet — 합성곱 5층 + FC 3층

🧮 **층별 출력 크기** (공식 $(N-K+2P)/S+1$ )

| 층 | 설정 | 출력 |
| --- | --- | --- |
| Input | | 227×227×3 |
| Conv1 + ReLU | 96개, 11×11, $S=4$, $P=0$ | 55×55×96 — $(227-11)/4+1$ |
| MaxPool1 | 3×3, $S=2$ | 27×27×96 — $(55-3)/2+1$ |
| Conv2 + ReLU | 256개, 5×5, $S=1$, $P=2$ | 27×27×256 |
| MaxPool2 | 3×3, $S=2$ | 13×13×256 |
| Conv3, 4, 5 + ReLU | 384, 384, 256개, 3×3, $S=1$, $P=1$ | 13×13×256 |
| MaxPool3 | 3×3, $S=2$ | 6×6×256 |
| FC6, FC7, FC8 | 9216 → 4096 → 4096 → 1000 | $6\cdot6\cdot256=9216$ |

(Norm1, Norm2는 크기를 바꾸지 않는다.)

**특징 네 가지**
1. **ReLU를 쓴 첫 CNN** — sigmoid·tanh의 포화 기울기 → 느린 학습을 피함
2. **첫 층의 큰 stride(4)** — feature 크기를 줄여 계산 절약 (약 3G GPU 메모리)
3. **Normalization layer** (LRN) — 지금은 쓰지 않음
4. **FC 층에 dropout 0.5** — 과적합 방지. "FC layers are extremely expensive!"

(보충) 가중치를 세면 합성곱 5층은 약 3.7M, FC 3층은 약 58.6M — **FC가 약 94%**다. FC6 하나( $9216\times4096\approx3.77\times10^7$ )가 합성곱 전체의 10배다.

### ZFNet · VGGNet

- **ZFNet**: AlexNet에서 CONV1을 11×11 stride 4 → **7×7 stride 2**, CONV3–5의 필터 수를 384, 384, 256 → **512, 1024, 512**로. 16.4% → 11.7%
- **VGGNet**: **작은 합성곱 필터로 더 깊게**. (보충) 3×3을 두 번 쌓으면 5×5 영역을 보면서 가중치는 $18C^2\lt25C^2$, 비선형도 한 번 더.

### GoogLeNet과 inception module

- 22층, **FC 층 없음**, AlexNet보다 파라미터 12배 적음, 6.7%
- **Inception module**: 1×1, 3×3, 5×5 합성곱과 3×3 max pooling을 **나란히** 쓰고, 결과를 **깊이 방향으로 이어 붙인다**. 여러 receptive field의 정보를 모은다. 공간 크기는 same padding으로 모두 28×28로 맞춘다.

🧮 **입력 28×28×256일 때**

| | Naive | 1×1 bottleneck (64채널) 삽입 |
| --- | --- | --- |
| 1×1 가지 | 128채널, $1\cdot1\cdot256\cdot128=32{,}768$ | 같음 |
| 3×3 가지 | 192채널, $3\cdot3\cdot256\cdot192=442{,}368$ | 1×1(64) → 3×3: $16{,}384+110{,}592$ |
| 5×5 가지 | 96채널, $5\cdot5\cdot256\cdot96=614{,}400$ | 1×1(64) → 5×5: $16{,}384+153{,}600$ |
| pooling 가지 | 256채널 (그대로), 0 | pool → 1×1(64): $16{,}384$ |
| **출력 채널** | $128+192+96+256=672$ | $128+192+96+64=480$ |
| **가중치** | **약 1090k** | **약 330k** (적힌 항을 다 더하면 346,112) |

- Naive의 문제: pooling 가지가 채널을 그대로 내보내 module마다 채널이 늘기만 한다 → 윗층 파라미터와 메모리 증가
- 해결: 큰 필터 **앞에** 1×1 conv로 채널을 줄인다 → 큰 필터가 256채널이 아니라 64채널을 받는다.

### 깊이와 성능, ResNet

**관찰** CIFAR-10에서 56층 plain 망이 20층보다 test error가 높다. 과적합일까? → **training error도 높다** → **과적합이 아니라 최적화 문제다**.

**왜 더 깊은 망이 더 나쁜가**
- 이론상 깊은 망은 학습 데이터를 얕은 망보다 **적어도 같게** 맞출 수 있어야 한다. (보충) 얕은 망을 복사하고 나머지 층을 **항등함수**로 두면 같은 함수가 되기 때문이다.
- 하지만 큰 망은 최적화가 훨씬 어렵다:
  - **Gradient vanishing**: $\frac{\partial J}{\partial h^{(l)}}=\frac{\partial J}{\partial h^{(L)}}\frac{\partial h^{(L)}}{\partial h^{(L-1)}}\cdots\frac{\partial h^{(l+1)}}{\partial h^{(l)}}$ — 작은 인자를 많이 곱하면 아래층 기울기가 0에 가까워진다.
  - **Covariate shift**: 아래층의 작은 변화가 깊은 층에서 큰 변화가 된다.

**Residual connection** — shortcut을 더해 **항등함수를 쉽게** 만든다.

| | Plain | Residual block |
| --- | --- | --- |
| 배우는 것 | $H(x)$ 전체 | 잔차 $F(x)$, 출력 $F(x)+x$ (그다음 ReLU) |

- $F(x)=0$ (가중치 0)이면 **항등함수** — "깊은 망 ⊇ 얕은 망"을 쉽게 만든다.
- **기울기가 identity 경로로 우회한다.** (보충) $y=x+F(x)$ → $\frac{\partial y}{\partial x}=I+\frac{\partial F}{\partial x}$: 곱의 각 인자에 $I$가 들어가 기울기가 사라지지 않는다. 04장 gate 패턴으로는 덧셈 노드가 upstream을 그대로 넘기는 것이다.

| Residual block | 구성 | 가중치 (보충) |
| --- | --- | --- |
| Plain (64채널) | 3×3, 64 → relu → 3×3, 64 | $2\cdot9\cdot64\cdot64=73{,}728$ |
| Bottleneck (256채널) | 1×1, 64 → 3×3, 64 → 1×1, 256 (256 → 64 → 256) | $16{,}384+36{,}864+16{,}384=69{,}632$ |

256채널에서 3×3을 그대로 두 번 하면 1,179,648 → bottleneck은 약 17분의 1. inception과 같은 1×1 요령이다.

**결과** plain 망은 깊을수록(20 → 56층) 오류가 커지고, residual 망은 깊을수록(20 → 110층) 오류가 준다.

### Saliency map

- 이미지의 **어느 부분이 예측에 가장 크게 기여하는지** 본다: 클래스 점수 $S_c(I)$를 **입력 픽셀**로 미분

$$\frac{\partial S_c(I)}{\partial I}=\frac{\partial F^{(L)}}{\partial F^{(L-1)}}\frac{\partial F^{(L-1)}}{\partial F^{(L-2)}}\cdots\frac{\partial F^{(1)}}{\partial I}$$

- 학습할 때의 역전파와 계산은 같고 **미분하는 대상만 다르다** (학습: 손실을 가중치로 미분 / saliency: 점수를 입력으로 미분). 역전파 한 번이면 이미지 크기의 기울기가 나온다.

---

## 자주 틀리는 것

- ⚠️ 필터 깊이는 입력 채널 수와 **같다** — 파라미터 셀 때 $C$를 빼먹지 않는다. bias(+1)도 필터마다 하나.
- ⚠️ 출력 크기에서 padding은 **양쪽**이라 $2P$.
- ⚠️ pooling은 채널 수를 바꾸지 않고 파라미터가 0이다.
- ⚠️ 합성곱은 이동에 **equivariant**다. "translation invariant"라는 슬라이드 표현은 "필터가 어느 위치에서나 같다"는 뜻이다.
- ⚠️ 깊은 plain 망의 성능 저하를 과적합이라 쓰면 틀린다 → **training error도 높다**는 근거를 쓴다.
- ⚠️ AlexNet 입력은 227 (그림의 224로 계산하면 나누어떨어지지 않는다).

## 셀프 체크

<details><summary>Q1. 64×64×3에 7×7 필터 16개, stride 2, padding 3이면 출력 크기와 파라미터 수는?</summary>⌊(64 − 7 + 6)/2⌋ + 1 = 32 → 32×32×16. 파라미터 16 × (7·7·3 + 1) = 2368</details>
<details><summary>Q2. FC와 합성곱 층의 차이 두 가지는?</summary>지역 연결, 가중치 공유</details>
<details><summary>Q3. equivariant와 invariant의 식은?</summary>f(g·x) = g·f(x) / f(g·x) = f(x)</details>
<details><summary>Q4. 입력 28×28×256에 5×5 conv 96개를 바로 쓸 때와 1×1 conv 64개를 먼저 거칠 때의 가중치는?</summary>614,400 vs 16,384 + 153,600 = 169,984</details>
<details><summary>Q5. 56층 plain 망이 나쁜 것이 과적합이 아닌 근거는?</summary>training error도 20층보다 높다.</details>
<details><summary>Q6. residual block이 항등함수를 만들려면?</summary>F(x) = 0 → 출력 x</details>
