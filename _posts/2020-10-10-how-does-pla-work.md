---
layout: post
title: "How does the Perceptron Learning Algorithm work?"
---

## Perceptron Model

The [perceptron](https://doi.org/10.1037/h0042519) is a binary classification algorithm with target set
$$\mathcal{Y} = \{-1, +1\}$$, domain set $$\mathcal{X} \in \mathbb{R}^{d \times m}$$,
where $$m$$ is the number of data points in the domain set.

For $$x \in \mathcal{X}$$, a weighted score is computed and the predicted output
is $$+1$$ if $$ \sum_{i=1}^{d} {w_i x_i} > \theta$$ otherwise, $$-1$$. Here,
$$x \in \mathbb{R}^{d \times 1}$$ and $$w \in \mathbb{R}^{d \times 1}$$.

Thus, $$h(x) = sgn(\sum_{i=1}^{d} {w_i x_i} - \theta)$$. Now let $$x_0 = -\theta$$ be
a dummy feature and $$w_0 = 1$$ be its corresponding weight, we can rewrite $$h(x)$$ as:

$$
\begin{aligned}
h(x) = sgn(\sum_{\textcolor{orange}{i=0}}^{d} {w_i x_i}) &= sgn(w^\top x)\\
&= sgn(w \cdot x)
\end{aligned}
$$

**Note**: Here $$x_i$$ is the i-th element of the vector $$x$$.

*For the model to work, ie. for $$h(x)$$ to produce the correct label, when $$y= +1$$,
we want $$w \cdot x > 0$$, ie. the angle between $$w$$ and $$x$$ should lie in $$[0, \pi/2]$$.
Similarly, when $$y = -1$$, we want $$w \cdot x < 0$$, ie. the angle between $$w$$
and $$x$$ should lie in $$[\pi/2, \pi]$$.*

## Perceptron Learning Algorithm

The perceptron learning algorithm can be breifly stated as follows.

For $$x_i \in \mathcal{X}$$, for $$y_i \in \mathcal{Y}$$ and
for $$h \in \mathcal{H}$$, where $$\mathcal{H}$$ is the hypothesis class.

1. Initialize the weight vector $$w^t$$ to a zero vector ($$t=0$$).
2. Find a mistake $$(x_i, y_i)$$ such that $$h(x_i) = sgn({w^t}^\top x_i) \ne y_i$$.
3. Update the weight vector such that $$w^{t+1} \leftarrow w^{t} + y_i x_i$$.
4. Repeat from step 2 untill convergence.

**Note**: Here $$x_i$$, $$y_i$$ are the i-th data points from the training data set
and $$w^t$$ is the weight vector at iteration $$t$$.
Also, $$x_i \in \mathbb{R}^{d \times 1}$$, $$y_i \in \mathbb{R}^{1 \times 1}$$ and
$$w \in \mathbb{R}^{d \times 1}$$.

Now, why does this work? I am glad you asked!

Let us assume that $$(x_i, y_i)$$ was a misclassification. We have
$$w^{t+1} \leftarrow w^{t} + y_i x_i$$.

$$
\begin{aligned}
w^{t+1} \cdot x_i &= {w^{t+1}}^\top x_i \\
&= ({w^{t} + y_i x_i})^\top x_i \\
&= {w^{t}}^\top x_i + y_i x_i^\top x_i \\
&= {w^{t}}^\top x_i + y_i \|x_i\|^2 \text{\hspace{2em}(1)}
\end{aligned}
$$

Now, let $$\alpha^{t+1}$$ be the angle between the weight vector $$w^{t+1}$$ and the input
vector $$x_i$$.

$$
\begin{aligned}
cos(\alpha^{t+1}) &= \frac{w^{t+1} \cdot x_i}{\|w^{t+1}\|\|x_i\|}\\
&\propto w^{t+1} \cdot x_i
\end{aligned}
$$

From Equation $$(1)$$, we get

$$
cos(\alpha^{t+1}) \propto cos(\alpha^t) + y_i \|x_i\|^2
$$

or,

$$
\begin{gathered}
cos(\alpha^{t+1}) < cos(\alpha^t) &\text{if } y_i = -1\\
cos(\alpha^{t+1}) > cos(\alpha^t) &\text{if } y_i = +1
\end{gathered}
$$

Thus, when $$y_i = -1$$, $$cos(\alpha^{t+1}) < cos(\alpha^t) \implies \alpha^{t+1} > \alpha^t$$.
The angle between a negative data point and the weight vector increases.

Whereas, when $$y_i = 1$$, $$cos(\alpha^{t+1}) > cos(\alpha^t) \implies \alpha^{t+1} < \alpha^t$$.
The angle between a positive data point and the weight vector decreases.

We can hereby conclude that the weight vector $$w^{t+1}$$ is more aligned (tending towards $$0$$ rad) to positive
data points and less aligned (tending towards $$\pi$$ rad) to negative data points than the weight vector $$w^t$$.
This is the intended behaviour!
