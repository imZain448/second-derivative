---
title: What's Entropy - Expanded explanation
date: 19-07-2025
---

Entropy is a measure of **unpredictability**, **uncertainty**, or **surprise** inherent in a source of information. In simpler terms, it tells you how much "randomness" there is in your data. A high entropy value indicates a lot of unpredictability, while a low entropy value indicates that the data is more predictable and contains more patterns.

Think of it this way:

*   **Low Entropy:** Imagine a coin that always lands on heads. There's no surprise, no uncertainty. You know exactly what the outcome will be every time.
*   **High Entropy:** Now imagine a fair coin. There's a 50/50 chance of heads or tails. Each flip is unpredictable, and there's more "surprise" in the outcome.

Mathematically, entropy $H(X)$ of a random variable $X$ with possible outcomes $\{x_1, x_2, ..., x_n\}$ is defined as:

$$H(X) = - \sum_{i=1}^{n} p(x_i) \log_2 p(x_i)$$

Let's break down this formula piece by piece:
*   $X$: Represents a random variable. A random variable is a variable whose value is a numerical outcome of a random phenomenon. In the context of data, this could be a symbol, a pixel value, a word, etc.
*   $\{x_1, x_2, ..., x_n\}$: Represents the set of all possible outcomes or symbols that the random variable $X$ can take. For example, if $X$ represents a coin flip, then the possible outcomes are $\{heads, tails\}$. If $X$ represents a byte of data, then the possible outcomes are $\{0, 1, 2, ..., 255\}$.
*   $p(x_i)$: Represents the probability of observing the outcome $x_i$. Probability is a measure of the likelihood that an event will occur. It is quantified as a number between 0 and 1, where 0 indicates impossibility and 1 indicates certainty.
*   $\log_2$: Represents the base-2 logarithm. The logarithm tells you what power you need to raise the base (in this case, 2) to get a certain number. In information theory, the base-2 logarithm is used because we're often dealing with bits (0s and 1s).
*   $- \sum_{i=1}^{n}$: This is a summation symbol, which means we're adding up a series of terms. The index $i$ goes from 1 to $n$, where $n$ is the number of possible outcomes. The negative sign ensures that the entropy value is always non-negative.

>**In essence, the formula calculates the weighted average of the "surprise" associated with each possible outcome. The "surprise" is measured by the base-2 logarithm of the probability of that outcome.**

**Examples to illustrate entropy:**
1.  **Coin Flip:**
    *   **Unfair Coin (Low Entropy):** Suppose you have a biased coin that lands on heads 90% of the time and tails 10% of the time.
        *   $p(heads) = 0.9$
        *   $p(tails) = 0.1$
        *   $H(X) = - (0.9 * \log_2(0.9) + 0.1 * \log_2(0.1)) \approx 0.469$ bits
    *   **Fair Coin (High Entropy):** Suppose you have a fair coin that lands on heads 50% of the time and tails 50% of the time.
        *   $p(heads) = 0.5$
        *   $p(tails) = 0.5$
        *   $H(X) = - (0.5 * \log_2(0.5) + 0.5 * \log_2(0.5)) = 1$ bit

    Notice that the fair coin has higher entropy (1 bit) than the unfair coin (0.469 bits). This is because the outcome of the fair coin is more unpredictable.

2.  **Text Strings:**
    *   **Repetitive String (Low Entropy):** Consider the string "AAAAAAA". There's only one symbol (A), and it occurs with probability 1.
        *   $p(A) = 1$
        *   $H(X) = - (1 * \log_2(1)) = 0$ bits
    *   **More Varied String (Higher Entropy):** Consider the string "ABCDEFG". Each symbol occurs with equal probability (1/7).
        *   $p(A) = p(B) = p(C) = p(D) = p(E) = p(F) = p(G) = 1/7$
        *   $H(X) = - 7 * (1/7 * \log_2(1/7)) = \log_2(7) \approx 2.81$ bits
    *   **English Text (Medium Entropy):** English text has some predictability. Some letters are more common than others (e.g., "e" is more common than "z"). Also, certain letter combinations are more likely (e.g., "th" is more common than "zx"). Therefore, English text has an entropy value somewhere between the two extremes.

3.  **Image Data:**

    *   **Solid Color Image (Low Entropy):** An image that is entirely blue will have very low entropy because all the pixel values are the same.
    *   **Random Noise Image (High Entropy):** An image that looks like random static will have high entropy because each pixel value is essentially random.
    *   **Typical Photograph (Medium Entropy):** A typical photograph will have medium entropy because it contains some patterns and structures, but also some degree of randomness.

**Key Takeaways:**
*   Entropy is a fundamental concept in information theory that quantifies the amount of uncertainty or randomness in data.
*   The higher the entropy, the more difficult it is to compress the data without losing information.
*   Shannon's Source Coding Theorem tells us that we can't compress data below its entropy limit (on average) without using lossy compression techniques.

Now, let's go back to the original question:

*   If all symbols are equally likely: entropy is **maximum**. This is because each outcome is equally surprising.
*   If one symbol dominates: entropy is **low**. This is because the outcome is highly predictable.