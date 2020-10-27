---
layout: post
title: "How many NaNs is too many?"
---

Every person who has ever trained a machine/deep learning model has at one point in their lives come across a NaN (acronym for **N**ot **a** **N**umber). But, did you know that the internal representation of one NaN may be different from another?

Well hold on, let's take a ride!

Computers don't understand the decimal number system, so, everything needs to be represented in terms of binary numbers. One such representation of floating point numbers is the [IEEE-754](https://en.wikipedia.org/wiki/IEEE_754) standard.

In this standard, a decimal number is represented by a $$1$$-bit sign ($$S$$), a $$w$$-bit biased exponent ($$E$$) and a $$t = (p-1)$$-bit trailing significand $$T=d_1d_2...d_t$$. $$d_0$$ is implicitly encoded in the biased exponent. $$p$$ is the precision of the significand.

For a $$k$$-bit ($$k > 128$$) IEEE-754 representation of a floating point number, we have

1. $$k$$ is a multiple of 32.
2. Precision, $$p$$ is $$k - \lfloor 4 \times \log_2 (k) \rceil + 13$$.
3. Maximum exponent, $$\text{emax}$$ is $$2 ^ {k - p - 1}$$.
4. Bias is $$\text{emax}$$.
5. Width of exponent field, $$w$$ is $$\lfloor 4 \times \log_2 (k) \rceil - 13$$

The bit representation of a 32-bit floating point number is

{:refdef: style="text-align: center;"}
![By Vectorization: Stannered - Own work based on: Float example.PNG, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=3357169](/images/float32.svg "By Vectorization: Stannered - Own work based on: Float example.PNG, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=3357169")
{:refdef}

The steps for the conversion from the IEEE-754 representation to the decimal systems are

1. The sign bit is $$0$$, so, the sign of the number is $$(-1)^0 = 1$$.
2. The exponent is $$01111100_2 = 124_{10}$$.
3. The fraction part is $$0.01000000000000000000000_2 = 1 \times 2^{-2}$$.

Putting it all together, the decimal value of the number is obtained as

$$
(-1)^\text{\colorbox{#c3fbfd}{sign bit}} \times (1 + \colorbox{#feacac}{\text{fraction}}) \times 2 ^ {\colorbox{#9ffeab}{\text{exponent}} - \text{bias}}
$$

For a 32-bit floating point number, the bias is 127.
Thus, we get $$1 \times (1+\frac{1}{2^2}) \times \frac{1}{2^3} = \frac{5}{2^5}= 0.15625$$.

Similarly, a 64-bit floating point number is represented as

{:refdef: style="text-align: center;"}
![By Codekaizen - Own work, CC BY-SA 4.0, https://commons.wikimedia.org/w/index.php?curid=3595583](/images/float64.svg "By Codekaizen - Own work, CC BY-SA 4.0, https://commons.wikimedia.org/w/index.php?curid=3595583")
{:refdef}

The representation $$r$$ and the value $$v$$ of the floating point datum are inferred from the constituent fields of the floating point representation as follows.

- If $$ E = 2^w -1 $$ and $$ T \ne 0 $$ then $$v$$ is NaN and $$r$$ is either qNaN or sNaN.
- If $$ E = 2^w -1 $$ and $$ T = 0 $$ then $$r =  v = (-1)^S \times \infty$$.
- Signed floating point numbers are represented by all other $$ E \in [0, 2^w - 2] $$.
- Note that zeros can be signed!

### Types of NaN

IEEE-754 defines two types of NaN, quiet NaN (qNaN) and signalling NaN (sNaN).

Quiet NaNs are implementation specific and can offer diagnostic information from the underlying data for a particular implementation.
The first bit of the trailing significand ($$d_1$$) is set to $$1$$ for a quiet NaN.

Signalling NaNs are reserved operands that are used to signal the invalid operation exception.
The first bit of the trailing significand ($$d_1$$) is set to $$0$$ for a quiet NaN.
Of the remaining $$t-1$$ bits, at least 1 should be non-zero to distinguish the NaN from infinity.

### How many NaNs are there

We know that for a number to be NaN, it must have its exponent $$E$$ set to $$ 2^w -1 $$ and $$ T \ne 0 $$.
So, for a $$k$$-bit representation, we have

$$ \text{Total number of NaNs} = 2 ^ {k - w} - 2 $$

$$ \text{Number of quiet NaNs} = 2 ^ {k - (w + 1)} $$

$$ \text{Number of signalling NaNs} = 2 ^ {k - w} - 2 ^ {k - (w + 1)} - 2 $$

The trend of the fraction of NaNs as compared to number of valid floating point representations is as follows

{:refdef: style="text-align: center;"}
![](/images/nanpercent.svg)
{:refdef}

**Note**: *The y-axis of the graph uses log scale.*
