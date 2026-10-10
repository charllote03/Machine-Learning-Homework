# 奇异值分解、贝叶斯过程、交叉熵知识点笔记

# 奇异值分解
### 1、奇异值分解定理
若 A 为一 m×n 实矩阵，$A \in \mathbb{R}^{m \times n}$，则 A 的奇异值分解存在

$$ A=U \Sigma V^\top $$

其中 U 是 m 阶正交矩阵，$V$ 是 n 阶正交矩阵，$\Sigma$ 是 m×n 矩形对角矩阵，其对角线元素非负，且按降序排列。

---

### 2、奇异值分解过程
给定 $m \times n$ 矩阵 $A$，矩阵奇异值分解可分为以下几个过程：

**第一步：求 $V$**

求 $A^\top A$ 的特征值和特征向量，将求得的特征向量单位化，得到单位特征向量 $v_i$，即为正交矩阵 $V$ 的列：

$$ V=\begin{bmatrix}v_1&v_2&\ldots&v_n\end{bmatrix} $$

**第二步：求 $\Sigma$**

求得的特征值平方根为奇异值 $\sigma_i$，将奇异值由大到小排列作为对角线元素，构成矩形对角矩阵 $\Sigma$：

$$ \Sigma=\mathrm{diag}(\sigma_1,\sigma_2,\ldots,\sigma_r) $$

$r$ 为矩阵 $A$ 的秩。

**第三步：求 $U_1$**

对 $A$ 的前 $r$ 个正奇异值，令

$$ u_j=\frac{1}{\sigma_j}Av_j $$

得到

$$ U_1=\begin{bmatrix}u_1&u_2&\ldots&u_r\end{bmatrix} $$

**第四步：求 $U_2$**

求 $A^\top$ 的零空间的一组标准正交基 $\{u_{r+1},u_{r+2},\ldots,u_m\}$，令

$$ U_2=\begin{bmatrix}u_{r+1}&u_{r+2}&\ldots&u_m\end{bmatrix} $$

并令

$$ U=\begin{bmatrix}U_1&U_2\end{bmatrix} $$

**第五步：得到奇异值分解**

$$ A=U \Sigma V^\top $$

---

### 3、实例计算
求矩阵

$$ A=\begin{bmatrix}1&2\\2&1\\0&0\end{bmatrix} $$

的奇异值分解。

解：先求对称矩阵

$$ A^\top A= \begin{bmatrix}1&2&0\\2&1&0\end{bmatrix} \begin{bmatrix}1&2\\2&1\\0&0\end{bmatrix} = \begin{bmatrix}5&4\\4&5\end{bmatrix} $$

求 $A^\top A$ 的特征值：

$$ \det(A^\top A-\lambda I)= \begin{vmatrix}5-\lambda&4\\4&5-\lambda\end{vmatrix} =(5-\lambda)^2-16=0 $$

解得

$$ \lambda_1=9,\qquad \lambda_2=1 $$

对应 $\lambda_1=9$ 的单位特征向量为

$$ v_1=\begin{bmatrix}\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}\end{bmatrix} $$

对应 $\lambda_2=1$ 的单位特征向量为

$$ v_2=\begin{bmatrix}\frac{1}{\sqrt{2}}\\-\frac{1}{\sqrt{2}}\end{bmatrix} $$

因此正交矩阵

$$ V=\begin{bmatrix}\frac{1}{\sqrt{2}}&\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}&-\frac{1}{\sqrt{2}}\end{bmatrix} $$

奇异值为

$$ \sigma_1=\sqrt{\lambda_1}=3,\qquad \sigma_2=\sqrt{\lambda_2}=1 $$

构造矩形对角矩阵

$$ \Sigma=\begin{bmatrix}3&0\\0&1\\0&0\end{bmatrix} $$

基于 $A$ 的正奇异值计算得到向量

$$ u_1=\frac{1}{\sigma_1}Av_1 =\frac{1}{3} \begin{bmatrix}1&2\\2&1\\0&0\end{bmatrix} \begin{bmatrix}\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}\end{bmatrix} = \begin{bmatrix}\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}\\0\end{bmatrix} $$

$$ u_2=\frac{1}{\sigma_2}Av_2 =\frac{1}{1} \begin{bmatrix}1&2\\2&1\\0&0\end{bmatrix} \begin{bmatrix}\frac{1}{\sqrt{2}}\\-\frac{1}{\sqrt{2}}\end{bmatrix} = \begin{bmatrix}-\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}\\0\end{bmatrix} $$

求 $A^\top$ 的零空间的一组标准正交基。因为

$$ A^\top= \begin{bmatrix}1&2&0\\2&1&0\end{bmatrix} $$

解 $A^\top u=0$，可得前两个分量为 $0$，因此可取

$$ u_3=\begin{bmatrix}0\\0\\1\end{bmatrix} $$

构造正交矩阵

$$ U=\begin{bmatrix} \frac{1}{\sqrt{2}}&-\frac{1}{\sqrt{2}}&0\\ \frac{1}{\sqrt{2}}&\frac{1}{\sqrt{2}}&0\\ 0&0&1 \end{bmatrix} $$

综上，矩阵 $A$ 的奇异值分解为

$$ A=U\Sigma V^\top = \begin{bmatrix} \frac{1}{\sqrt{2}}&-\frac{1}{\sqrt{2}}&0\\ \frac{1}{\sqrt{2}}&\frac{1}{\sqrt{2}}&0\\ 0&0&1 \end{bmatrix} \begin{bmatrix}3&0\\0&1\\0&0\end{bmatrix} \begin{bmatrix} \frac{1}{\sqrt{2}}&\frac{1}{\sqrt{2}}\\ \frac{1}{\sqrt{2}}&-\frac{1}{\sqrt{2}} \end{bmatrix} $$

---

# 贝叶斯过程
### 1、贝叶斯定理

$$ P(\theta \mid D)=\frac{P(D \mid \theta)P(\theta)}{P(D)} $$

其中：

$P(\theta)$：**先验分布**，观测数据之前，对未知参数 $\theta$ 的主观认识。

$P(D\mid\theta)$：**似然**，给定 $\theta$ 下，数据 $D$ 出现的概率。

$P(D)$：**边际似然**，数据本身出现的概率。

$P(\theta\mid D)$：**后验分布**，观测到数据后更新得到的认识。

> 贝叶斯的过程性体现在：前一步算出的后验，可以当作当前的先验，从而形成不断吸收新证据、持续更新的迭代闭环。

---

### 2、项目案例：基于振动信号的旋转机械故障预警
#### 应用背景
在旋转机械故障预警项目中，系统通过振动传感器采集轴承、齿轮箱等部件的运行信号，并由模型输出一个故障置信度 $s$。如果只简单设定阈值，例如 $s>0.5$ 就报警，会忽略两类错误的代价差异。

漏报故障可能导致设备停机、产线中断甚至结构失效；误报只需要安排人工巡检复核。因此，需要利用贝叶斯过程对置信度进行代价敏感更新。

#### 数学模型建立
设真实状态为：
- $F$：设备存在故障；
- $N$：设备正常。

设先验概率为

$$ P(F)=\pi,\qquad P(N)=1-\pi $$

设某次模型输出或传感器证据为 $e$，其似然为

$$ P(e\mid F),\qquad P(e\mid N) $$

根据贝叶斯定理，后验概率为

$$ P(F\mid e)=\frac{P(e\mid F)P(F)}{P(e\mid F)P(F)+P(e\mid N)P(N)} $$

$$ P(N\mid e)=1-P(F\mid e) $$

定义代价矩阵：

| 真实情况 | 判为故障 $a_F$ | 判为正常 $a_N$ |
| ---- | ---- | ---- |
| 真实故障 $F$ | $0$ | $\lambda_{miss}$ |
| 正常 $N$ | $\lambda_{false}$ | $0$ |

其中 $\lambda_{miss}$ 表示漏报故障的代价，$\lambda_{false}$ 表示误报正常的代价。

若采取动作 $a_F$，即判为故障，其期望代价为

$$ C(a_F\mid e)=P(N\mid e)\lambda_{false} $$

若采取动作 $a_N$，即判为正常，其期望代价为

$$ C(a_N\mid e)=P(F\mid e)\lambda_{miss} $$

当

$$ C(a_F\mid e)<C(a_N\mid e) $$

即

$$ P(N\mid e)\lambda_{false}<P(F\mid e)\lambda_{miss} $$

时，应判定为故障。整理得

$$ \frac{P(F\mid e)}{P(N\mid e)}>\frac{\lambda_{false}}{\lambda_{miss}} $$

也就是说，当后验概率之比超过代价比时，就判定为故障。

#### 单次贝叶斯更新计算
设该批次设备真实故障率为 $3\%$，即

$$ P(F)=0.03,\qquad P(N)=0.97 $$

某次模型输出置信度落在高分段 $s_0$，经验估计：

$$ P(s_0\mid F)=0.80,\qquad P(s_0\mid N)=0.20 $$

则后验概率为

$$ \begin{aligned} P(F\mid s_0) &=\frac{P(s_0\mid F)P(F)}{P(s_0\mid F)P(F)+P(s_0\mid N)P(N)}\\ &=\frac{0.80\times 0.03}{0.80\times 0.03+0.20\times 0.97}\\ &=\frac{0.024}{0.218}\\ &\approx 0.1101 \end{aligned} $$

于是

$$ P(N\mid s_0)\approx 0.8899 $$

后验概率之比为

$$ \frac{P(F\mid s_0)}{P(N\mid s_0)} = \frac{0.1101}{0.8899} \approx 0.1237 $$

设漏检代价和误检代价分别为

$$ \lambda_{miss}=20,\qquad \lambda_{false}=2 $$

代价比为

$$ \frac{\lambda_{false}}{\lambda_{miss}} = \frac{2}{20} = 0.1 $$

因为

$$ 0.1237>0.1 $$

所以应判定为故障。

#### 贝叶斯过程迭代
上述过程只使用了一次贝叶斯定理。在实际项目中，可以将上一轮计算出的后验当作下一轮先验，形成贝叶斯过程。

第一轮后验为

$$ P(F\mid s_0)\approx 0.1101,\qquad P(N\mid s_0)\approx 0.8899 $$

第二轮观测到新证据 $e_2$，其似然为

$$ P(e_2\mid F)=0.70,\qquad P(e_2\mid N)=0.10 $$

将第一轮后验作为第二轮先验，则

$$ \begin{aligned} P(F\mid e_2,s_0) &=\frac{P(e_2\mid F)P(F\mid s_0)} {P(e_2\mid F)P(F\mid s_0)+P(e_2\mid N)P(N\mid s_0)}\\ &=\frac{0.70\times 0.1101} {0.0771+0.0890}\\ &\approx 0.4641 \end{aligned} $$

此时后验概率之比为

$$ \frac{P(F\mid e_2,s_0)}{P(N\mid e_2,s_0)} = \frac{0.4641}{0.5359} \approx 0.866 $$

仍然大于代价比 $0.1$，因此继续判定为故障。

第三轮观测到新证据 $e_3$，其似然为

$$ P(e_3\mid F)=0.60,\qquad P(e_3\mid N)=0.05 $$

继续将上一轮后验作为先验：

$$ \begin{aligned} P(F\mid e_3,e_2,s_0) &=\frac{P(e_3\mid F)P(F\mid e_2,s_0)} {P(e_3\mid F)P(F\mid e_2,s_0)+P(e_3\mid N)P(N\mid e_2,s_0)}\\ &=\frac{0.60\times 0.4641} {0.2785+0.0268}\\ &\approx 0.9122 \end{aligned} $$

后验概率之比为

$$ \frac{P(F\mid e_3,e_2,s_0)}{P(N\mid e_3,e_2,s_0)} = \frac{0.9122}{0.0878} \approx 10.39 $$

远大于代价比 $0.1$，因此系统给出高置信度故障预警。

通过上述过程，设备故障概率从初始先验 $0.03$ 逐步更新到约 $0.9122$，体现了贝叶斯过程不断吸收新证据、持续修正认识的迭代特性。

---

# 交叉熵
### 1、定义
交叉熵定义为：用模型分布 $Q$ 去编码或描述真实分布 $P$ 时，所付出的平均编码代价。

$$ H(p,q)=-\sum_{x}p(x)\log q(x) $$

其中：

$p$：真实标签的概率分布。

$q$：模型预测出来的概率分布。

---

### 2、原理推导
**信息量**：事件 $x$ 发生所携带的信息量可以定义为

$$ I(x)=-\log p(x) $$

概率越小的事件，一旦发生，其信息量越大。

**信息熵**：信息熵是随机变量 $X$ 的平均信息量或期望信息量：

$$ H(X)=E[I(X)] =\sum_{x\in X}p(x)I(x) =-\sum_{x\in X}p(x)\log p(x) $$

信息熵度量了一个随机变量的不确定性。熵越大，不确定性越大。

**交叉熵**：如果用模型分布 $q(x)$ 去编码真实分布 $p(x)$，则平均编码长度为

$$ H(p,q)=-\sum_{x}p(x)\log q(x) $$

**KL散度**：KL散度表示由于使用次优编码策略而导致的额外平均编码长度，即信息损失：

$$ \begin{aligned} D_{KL}(p\|q) &=\left(-\sum_{x}p(x)\log q(x)\right) -\left(-\sum_{x}p(x)\log p(x)\right)\\ &=\sum_{x}p(x)\left(\log p(x)-\log q(x)\right)\\ &=\sum_{x}p(x)\log\frac{p(x)}{q(x)} \end{aligned} $$

因此，交叉熵、信息熵、KL散度三者关系为

$$ D_{KL}(p\|q)=H(p,q)-H(p) $$

即

$$ H(p,q)=H(p)+D_{KL}(p\|q) $$

> 交叉熵多出来的部分正是因模型分布 $q(x)$ 偏离真实分布 $p(x)$ 而多付的代价，即 KL 散度。$q(x)$ 越接近 $p(x)$，KL 散度越小，交叉熵越小。这是把交叉熵作为损失函数来优化的根本原理。

---

### 3、应用案例与推导
以三分类手写数字识别为例，类别为数字 $0$、数字 $1$、数字 $2$。输入一张真实数字为 $0$ 的图片。

真实分布采用 One-hot 编码：

$$ p=[1,0,0] $$

模型 A 的预测为：

$$ q_A=[0.85,0.10,0.05] $$

模型 B 的预测为：

$$ q_B=[0.20,0.50,0.30] $$

使用自然对数 $\ln$ 计算。

#### 真实分布的信息熵

$$ H(p)=-(1\cdot \ln 1+0\cdot \ln 0+0\cdot \ln 0)=0 $$

因为真实标签是确定性的，所以信息熵为 $0$。

#### 模型 A 的交叉熵损失

$$ \begin{aligned} H(p,q_A) &=-\sum_i p_i\ln q_{A,i}\\ &=-(1\cdot \ln 0.85+0\cdot \ln 0.10+0\cdot \ln 0.05)\\ &=-\ln 0.85\\ &\approx 0.1625 \end{aligned} $$

#### 模型 B 的交叉熵损失

$$ \begin{aligned} H(p,q_B) &=-\sum_i p_i\ln q_{B,i}\\ &=-(1\cdot \ln 0.20+0\cdot \ln 0.50+0\cdot \ln 0.30)\\ &=-\ln 0.20\\ &\approx 1.6094 \end{aligned} $$

#### KL 散度验证
由于真实分布的信息熵 $H(p)=0$，所以

$$ D_{KL}(p\|q_A)=H(p,q_A)-H(p)\approx 0.1625 $$

$$ D_{KL}(p\|q_B)=H(p,q_B)-H(p)\approx 1.6094 $$

#### 分析
模型 A 对真实类别数字 $0$ 的预测概率为 $0.85$，更接近真实标签 $1$，因此交叉熵损失约为 $0.1625$。

模型 B 对真实类别数字 $0$ 的预测概率只有 $0.20$，偏离真实分布较远，因此交叉熵损失约为 $1.6094$。

在训练过程中，反向传播算法会根据交叉熵损失调整模型参数，使模型预测分布 $q$ 越来越接近真实分布 $p$。因此，交叉熵越小，说明模型对真实类别的预测越准确。
