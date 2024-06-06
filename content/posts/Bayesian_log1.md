---
title: "论文手记，变分贝叶斯方法求后验分布"
date: 2024-05-28T00:22:02+08:00
draft: false
---

变分贝叶斯方法（Variational Bayesian Methods）是一种用于近似复杂后验分布的技术。其核心思想是将难以计算的后验分布转化为一个优化问题，通过寻找一个简单的分布族来近似真实的后验分布。以下是使用变分贝叶斯方法求解后验分布的基本步骤：

### 基本步骤

1. **定义后验分布**：
   假设我们有一个概率模型，包含观测数据 \( \mathbf{X} \) 和参数 \( \mathbf{\theta} \)。后验分布 \( p(\mathbf{\theta} | \mathbf{X}) \) 可以通过贝叶斯公式表示为：
   \[
   p(\mathbf{\theta} | \mathbf{X}) = \frac{p(\mathbf{X} | \mathbf{\theta}) p(\mathbf{\theta})}{p(\mathbf{X})}
   \]
   其中， \( p(\mathbf{X} | \mathbf{\theta}) \) 是似然函数， \( p(\mathbf{\theta}) \) 是先验分布， \( p(\mathbf{X}) \) 是边缘似然。

2. **选择变分分布族**：
   选择一个简单的分布族 \( q(\mathbf{\theta} | \mathbf{\lambda}) \) 来近似真实的后验分布 \( p(\mathbf{\theta} | \mathbf{X}) \)，其中 \( \mathbf{\lambda} \) 是变分分布的参数。

3. **变分推断目标**：
   最大化变分下界（Evidence Lower BOund, ELBO），以最小化KL散度 \( D_{KL}(q(\mathbf{\theta} | \mathbf{\lambda}) \| p(\mathbf{\theta} | \mathbf{X})) \)。变分下界定义为：
   \[
   \mathcal{L}(\mathbf{\lambda}) = \mathbb{E}_{q(\mathbf{\theta} | \mathbf{\lambda})} \left[ \log p(\mathbf{X}, \mathbf{\theta}) - \log q(\mathbf{\theta} | \mathbf{\lambda}) \right]
   \]
   由于 \( p(\mathbf{X}) \) 是一个常数，可以通过最大化ELBO来间接最小化KL散度。

4. **优化ELBO**：
   通过梯度下降或其他优化算法对变分参数 \( \mathbf{\lambda} \) 进行优化，找到使ELBO最大的参数。

### 详细过程

假设我们要使用变分贝叶斯方法求解一个具体问题，例如简单的高斯混合模型（GMM）。以下是详细过程：

1. **模型定义**：
   假设观测数据 \( \mathbf{X} = \{x_1, x_2, \ldots, x_N\} \) 来自一个混合高斯模型：
   \[
   p(x_i | \theta) = \sum_{k=1}^K \pi_k \mathcal{N}(x_i | \mu_k, \sigma_k^2)
   \]
   其中， \( \theta = \{\pi_k, \mu_k, \sigma_k^2\} \) 是模型参数， \( \pi_k \) 是混合系数， \( \mu_k \) 和 \( \sigma_k^2 \) 分别是第 \( k \) 个高斯成分的均值和方差。

2. **选择变分分布**：
   选择一个因子化的变分分布族：
   \[
   q(\theta | \lambda) = q(\pi | \lambda_\pi) q(\mu | \lambda_\mu) q(\sigma^2 | \lambda_{\sigma^2})
   \]
   其中， \( \lambda = \{\lambda_\pi, \lambda_\mu, \lambda_{\sigma^2}\} \) 是变分参数。

3. **变分下界（ELBO）**：
   定义变分下界：
   \[
   \mathcal{L}(\lambda) = \mathbb{E}_{q(\theta | \lambda)} \left[ \log p(\mathbf{X}, \theta) - \log q(\theta | \lambda) \right]
   \]
   由于 \( p(\mathbf{X}, \theta) = p(\mathbf{X} | \theta) p(\theta) \)，变分下界可以写为：
   \[
   \mathcal{L}(\lambda) = \mathbb{E}_{q(\theta | \lambda)} \left[ \log p(\mathbf{X} | \theta) + \log p(\theta) - \log q(\theta | \lambda) \right]
   \]

4. **优化**：
   使用梯度下降等优化算法对变分参数 \( \lambda \) 进行优化，最大化ELBO。
   \[
   \lambda^* = \arg\max_\lambda \mathcal{L}(\lambda)
   \]

### 总结

变分贝叶斯方法通过将后验分布的推断问题转化为优化问题，利用简单的变分分布来近似真实的后验分布，从而在计算上更为可行。其核心步骤包括定义后验分布、选择变分分布族、定义并优化ELBO。这种方法在处理大规模数据和复杂模型时尤为有用，是现代贝叶斯推断的重要工具之一。
