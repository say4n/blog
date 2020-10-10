---
layout: post
title: "How does the Perceptron Learning Algorithm Work?"
---

The perceptron learning algorithm can be breifly stated as follows.

For $$x_i \in \mathcal{X}$$, where $$\mathcal{X}$$ is the domain set,
for $$y_i \in \mathcal{Y}$$, where $$\mathcal{y}$$ is the target set and
for $$h \in \mathcal{H}$$, where $$\mathcal{H}$$ is the hypothesis class.

1. Initialize the weight vector $$w^t$$ to a zero vector.
2. Find a mistake $$(x_i, y_i)$$ such that $$h(x_i) = sgn({w^t}^\top x_i) \ne y_i$$.
3. Update the weight vector such that $$w^{t+1} \leftarrow w^{t} + y_i x_i$$.
4. Repeat from step 2 untill convergence.

Now, why does this work? I am glad you asked!
