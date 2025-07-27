---
title: The Last bit - Understanding the Limit of Compression
overview: "Have you ever wondered how a single photo taken on your phone takes up just a few hundred kilobytes, while the raw version would be several megabytes? Or how we stream entire movies without downloading gigabytes of data first? The answer lies in compression — a clever way of removing patterns and redundancies from data.But what happens when data can't be compressed anymore? Does it mean it's truly random? Or perfectly encoded? In this article, we explore how compression works, what it reveals about the data it touches, and why there's a fundamental limit to how much you can shrink information. Along the way, we’ll meet JPEGs, Shannon's theorems, forward error correction, and a surprising truth: sometimes, the inability to compress something tells you more than you’d expect."
date: 19-07-2025
---
![[DEEP LEARNING SERIES-PART1.gif]]
>First in our deep learning series: We explore compression, the art of removing redundancy, and why it's essential for understanding how machines learn
## 1. A World Without JPEGs and MPEGs

Imagine taking a single photo and seeing it consume **6 megabytes** of storage. Now record just **one minute of full HD video** — you’re looking at over **10 gigabytes**. This is what data looks like _before_ compression.

Let’s break that down:
### 📷 Raw Image Size (1080p)
- Resolution: $1920 \times 1080 = 2,073,600$ pixels
- RGB color: 3 bytes per pixel
- Total size: $2,073,600 \times 3 = 6,220,800 \text{ bytes} \approx 6 \text{ MB}$
### 🎥 Raw Video Size (1 minute at 30 fps)
- 30 frames per second × 60 seconds = 1800 frames
- Each frame ≈ 6 MB
- Total size: $6 \text{ MB} \times 1800 = 10.8 \text{ GB}$
So, without compression, one minute of HD video would require nearly **11 gigabytes** — more than many smartphones could have stored just a decade ago. And that’s without even talking about audio!

### Why JPEG and MPEG Matter

This is where formats like **JPEG** (for images) and **MPEG** (for videos) come in. They make storage and streaming possible by removing **redundancy** — the repeated or predictable parts of data that don’t need to be stored in full.


### How JPEG Compresses an Image
JPEG doesn’t save every pixel exactly as it is. Instead, it looks for patterns and removes unnecessary details — especially the ones your eyes won’t even notice. Here's how it works in simple terms:

1. **Break the image into small blocks** — usually 8×8 pixels each.
2. **Find patterns** in each block — smooth color areas, edges, or sharp transitions.
3. **Keep the important stuff** — like broad shapes and color transitions.
4. **Throw away tiny details** that the human eye is unlikely to notice.
5. **Store what's left** in a compact format using efficient encoding techniques.

That’s why a raw image that takes 6 MB can shrink down to just 300 KB — and still look almost the same to the human eye.

> 🧠 Curious how it works under the hood?  
> Check out the **Appendix: JPEG Compression Explained with Math** → [[JPEG compression algorithm | link]]

### The Bigger Picture

JPEG achieves compression by answering one simple question:

> What can we throw away without anyone noticing?

The result is often a 10× to 20× reduction in size — from 6 MB down to a few hundred kilobytes for a typical image — while keeping it visually intact.

But this kind of magic only works **if there’s something to remove** — repeated patterns, predictable structures, visual tricks. Which brings us to the heart of the matter:

**What if the data has no pattern at all? Can we still compress it?**

To answer that, we need to meet the mind who first asked — and answered — this question: **Claude Shannon**.

---
## 2. Shannon’s Theorem — What It Tells Us About Compression

After seeing how JPEG cleverly removes visual redundancies, a natural question arises:

> How much can we compress something — in theory — before we hit a wall?

That question was answered over 75 years ago by Claude Shannon, the founder of information theory.

### The Source Coding Theorem

In 1948, Shannon introduced the **Source Coding Theorem**, which sets the fundamental limit of how efficiently any lossless compression algorithm can work.

The key idea is this:

> You can’t compress data — on average — below its entropy.

Or put differently:

> Entropy is the theoretical best-case size of your data.


### What Is Entropy?

Entropy is a measure of **unpredictability** or **surprise** in your data. If your data is full of repeated or predictable values, entropy is low. If your data is random and hard to predict, entropy is high.

Mathematically, entropy $H(X)$ of a random variable $X$ with possible outcomes $\{x_1, x_2, ..., x_n\}$ is defined as:

$$H(X) = - \sum_{i=1}^{n} p(x_i) \log_2 p(x_i)$$

where $p(x_i)$ is the probability of seeing symbol $x_i$.

- If all symbols are equally likely: entropy is **maximum**.
- If one symbol dominates: entropy is **low**.

For example:
- A string like `"aaaaaaa"` has **low entropy** — it’s very predictable.
- A string like `"qmwzueo"` has **higher entropy** — much less predictable.

> Want to dive deeper into this formula and what it really means?  
> Check out the **Appendix: Entropy Explained** → [[Entropy Explained | link]]

### What This Means for Compression

Let’s say you’re sending a message made of symbols A, B, and C. If A occurs 90% of the time, and B and C just 5% each, you can assign **shorter codes** to A and **longer codes** to B and C. This is how **Huffman coding** works — by matching code length to symbol probability.

Shannon’s theorem guarantees that:

- You can compress the data down to around **$H(X)$ bits per symbol**, but **not lower** (on average) using **lossless** methods.

> For example, if a message has entropy 1.5 bits/symbol, the best compression will average around 1.5 bits per symbol. You might get lucky with one file, but over many, you can't beat that average.


### Why This Matters

- JPEG works well because most images **have structure** — patterns, smooth color transitions, redundant information.
- But Shannon tells us: **if there’s no structure, there’s no room to compress.**

That’s the boundary he drew for us. If your data looks random enough, **no amount of clever encoding will make it smaller** without losing something.
And that takes us to the next question:

> **What does entropy really measure? And how is it different from information?**

Let’s go there next.


---
## 3. What Are the Other Compression Techniques?

JPEG is just one type of compression — and a very specific one, designed for images. But data comes in all forms: text, numbers, logs, videos, code, DNA sequences. Naturally, we’ve built different algorithms to handle different kinds of structure.

But behind every compression algorithm is one goal:

> **Find the patterns, then store them in a smarter way.**

Let’s look at some of the most common types of compression — and what kinds of patterns they’re good at spotting.


### 🔁 1. Run-Length Encoding (RLE)

**Good for**: Repetitive data like `"aaaaaaa"` or long sequences of the same pixel.

**How it works**: Instead of storing each repeated character or value individually, RLE stores the value once, along with the number of times it repeats (the "run length"). This is extremely simple and effective when you have long runs of identical data.

Example:
- `"aaaaaabbbb"` → `a6b4` (meaning six 'a's followed by four 'b's)

Simple, but powerful when there are lots of repetitions.

### 📚 2. Dictionary-Based Compression (LZ77 / LZW / DEFLATE)

**Good for**: Text files, logs, documents, code — anything with repeated phrases or sequences.

**How it works**: These algorithms build a "dictionary" of frequently occurring patterns within the data. Instead of repeatedly storing those patterns, they store a reference (a pointer or index) to the dictionary entry. This is particularly effective for text, where words and phrases are often repeated. DEFLATE is a combination of LZ77 and Huffman coding.

Used in: ZIP files, PNG images, GIFs, gzip compression.

### 🌳 3. Huffman Coding

**Good for**: Compressing symbols based on their frequency.

**How it works**: Huffman coding assigns shorter bit codes to symbols that appear more frequently and longer codes to symbols that appear less frequently. This is based on the idea that you can save space by using fewer bits to represent common symbols. The codes are designed in such a way that no code is a prefix of another, ensuring unambiguous decoding.

Example:  
If 'e' occurs often, it might be encoded as `1`, while 'z' might become `11110`.

Often used as the final step in other algorithms.

### 📈 4. Arithmetic Coding

**Good for**: Achieving close-to-optimal compression for known symbol probabilities.

**How it works**: Instead of assigning a specific code to each symbol, arithmetic coding represents the entire message as a single fraction between 0 and 1. The length of the interval representing the message is determined by the probabilities of the symbols. It's more complex than Huffman coding but can achieve better compression, especially when symbol probabilities are highly skewed.

Used in some video codecs, image formats, and specialized scientific applications.


### 🧠 5. Transform-Based Compression (like JPEG and MPEG)

**Good for**: Images, audio, and video — data where **perceptual quality** matters more than exact recovery.

**How it works**: [[The Last bit - Understanding the Limit of Compression#How JPEG Compresses an Image | Already explained in section 1]]
Lossy, but efficient.

For a detailed explanation of these techniques look at [[Compression Techniques - Detailed | here]]
### ⚠️ 6. Lossless vs. Lossy Compression

| Type     | Can Recover Original? | Used For               |
| -------- | --------------------- | ---------------------- |
| Lossless | ✅ Yes                 | Text, code, logs, data |
| Lossy    | ❌ No (approximate)    | Images, audio, video   |

Lossless compression preserves every bit.  
Lossy compression sacrifices precision for space — as long as it still _looks or sounds_ right.


Now that we’ve seen **how compression works** and **where it works best**, it’s time to dig deeper.

Let’s go beyond the algorithms and ask a tougher question:

> **What’s the relationship between entropy and information?**

Are they the same thing? Or is one hiding inside the other?

Let’s find out.

## 4. Entropy vs Information

At this point, we know entropy tells us how _unpredictable_ or _surprising_ data is. But here’s where things get subtle:

> **Does higher entropy always mean more information?**

Not quite.

Let’s unpack the difference.

### Entropy Measures Surprise — Not Meaning

Entropy is purely statistical. It doesn’t care whether your data makes sense — it only measures how likely each symbol is.

Let’s look at two examples:
- 🅰️ A: `"qmwzueo"`
	- Looks random.
	- High entropy.
	- But may just be noise.
-  🅱️ B: `"I love you"`
	- Predictable letters.
	- Low entropy.
	- But emotionally meaningful.

**Conclusion**:
- **Entropy** tells you how _compressed_ something can be.
- **Information** tells you how _meaningful_ something is — and that depends on the context, not just the probabilities.
### Shannon’s Definition of Information
Even Shannon used the terms carefully:
- **Entropy** is about possible symbol arrangements.
- **Information** is what you get **when entropy is reduced** — when you learn something you didn’t already know.
So:
- If someone sends you a message with _very low entropy_, it's probably boring or redundant.
- If it has _high entropy_, it might be useful — or it might just be gibberish.

### Structure vs Surprise
Here’s another way to think about it:

| Data Example           | Entropy     | Meaningful?     |
| ---------------------- | ----------- | --------------- |
| `"aaaaaa"`             | Low         | No              |
| `"adfkla"`             | High        | Probably not    |
| `"The Earth is round"` | Medium–High | Yes             |
| Encrypted message      | Maximum     | Yes, but hidden |
| Random noise           | Maximum     | No              |
So the ideal sweet spot is data that’s **structured but non-trivial**.  
That’s where **true information** lives — not too repetitive, not too random.


### Can Information Be Compressed?

Yes — and in fact, **that’s the whole point of compression**.
Compression reveals **hidden structure**. If you can compress something, it means there’s _redundancy_ — something predictable — which implies that **the actual information content is smaller than the raw size**.
But if your data is already pure information — nothing repeated, nothing predictable — you won’t be able to shrink it any further. You’ve hit the **information core**.

### TL;DR: Entropy vs Information

| Concept     | What it Measures             | Units           | Depends On               |
| ----------- | ---------------------------- | --------------- | ------------------------ |
| Entropy     | Statistical unpredictability | Bits            | Symbol probabilities     |
| Information | Meaningful content learned   | Bits (relative) | Context, prior knowledge |


> Next, let’s ask the deeper question:  
> **What does it mean when something resists compression altogether?**  
> Is it pure information — or pure randomness?

That’s where we find the true limit of compression.
Let’s go there next.
## 5. Compressibility

We’ve talked about entropy, and we’ve talked about information. But there’s something even more tangible that sits right in between:

> **Compressibility.**

It’s what we observe in practice. And it tells us something powerful about the data we’re dealing with.

### What Is Compressibility, Really?

Compressibility is the **degree to which you can reduce the size of data without losing anything important**.

If your data compresses well, it means:
- There’s **redundancy**.
- There are **patterns**.
- Something about it is **predictable**.

If your data refuses to compress, it means:
- There’s **very little redundancy**.
- It may be **already compressed**, **encrypted**, or **random**.

So while entropy is the _theoretical limit_, compressibility is how **close you get to that limit** in practice.

### Why This Matters

Compressibility isn’t just a technical convenience — it’s a **diagnostic tool**.

Let’s say you have a file, and you try to compress it with a state-of-the-art algorithm like `zstd`, `gzip`, or `lzma` — and it **barely shrinks**.

That tells you something:

- Either the file is already optimally compressed.
- Or it’s full of high-entropy content — like noise or encrypted data.
- Or there really is no structure left to exploit.

In other words:

> **If your data can’t be compressed, you’ve either reached the truth — or the chaos.**
### Compression as a Lens

Compression doesn’t just reduce size. It reveals:
- Structure in text (like grammar and repeated phrases)
- Patterns in images (like textures and gradients)
- Predictability in time series (like seasonal trends)
- Even logic in code (like recurring function calls)

The more compressible something is, the more structured it is — and the more **room there is to learn, optimize, or summarize.**

That’s why compressibility is so useful in fields like:

- **Machine learning**: pre-processing and redundancy removal
- **Genomics**: finding repeated DNA segments
- **Security**: testing for encryption or obfuscation
- **Data forensics**: checking file tampering or hidden content

### A Quick Litmus Test

Here’s how different kinds of data typically fare when compressed:

| Data Type                            | Compressible? | Why                             |
| ------------------------------------ | ------------- | ------------------------------- |
| Plain text (logs, books)             | ✅ Very        | Highly redundant and structured |
| Raw images (BMP)                     | ✅             | Contains spatial redundancy     |
| Already compressed files (ZIP, JPEG) | ❌ Barely      | Already close to entropy limit  |
| Encrypted files                      | ❌             | Designed to look random         |
| White noise                          | ❌             | Truly random, no pattern        |

### The Boundary Line

There’s a fine line between **highly informative** and **utterly chaotic** — and compressibility helps draw it.

- If your data compresses: you’ve found structure.
- If it doesn’t: either it’s encrypted, random, or already distilled.

This leads us naturally to the final wall:

> **What if something is so random, so unpredictable, that it contains no compressible structure at all?**

Welcome to the realm of **true randomness** — the final limit of compression.

Let’s explore that in the next section.

---
## 6. Information Redefined in Terms of Compression

We’ve come a long way — from JPEGs to Shannon, from entropy to structure, and finally to the edge where data resists compression altogether.

Now we’re ready to ask one last question:

> **What is information, really?**

Not just statistically. But conceptually.

### When Compression Ends, Information Begins

Let’s flip everything we’ve learned:
- Compression removes what’s **redundant**.
- What remains is what’s **essential**.
- That essence — the part that can’t be reduced any further — is what we call **information**.

So here’s a powerful way to redefine it:

> **Information is what survives compression.**

It’s the irreducible residue — the thing that still needs to be said _after_ everything predictable has been stripped away.

### Kolmogorov Complexity: The Deepest Cut
In theoretical computer science, this idea is formalized as **Kolmogorov Complexity**.

> It defines the information content of a string as the length of the **shortest possible program** that can produce it.

So:
- A string like `"aaaaaaa"` has low Kolmogorov complexity — it can be generated with a short loop.
- A truly random string like `"xb47kpz9..."` has high complexity — there's no shortcut; the shortest program is just “print this exact string.”

The more compressible a string is, the **lower** its complexity.  
The more random it is, the **higher** its complexity.

This flips everything:

- Structure becomes a **shortcut**.
- Redundancy becomes **evidence of order**.
- And compression becomes a tool for measuring **depth**.

### Meaning Through Compression

In real life, we care about _meaning_. But in data, meaning often comes hand-in-hand with **structure**. When structure exists, we can:

- Learn from it
- Compress it
- Transmit it efficiently
- Build systems around it

That’s why compression is more than an engineering trick — it’s a **window into the soul of information**.

### Final Thought: Compression as Epistemology

Think about this:

- Science compresses nature into laws.
- History compresses events into stories.
- Language compresses ideas into words.
- AI compresses data into latent representations.

> Compression isn’t just about making things smaller —  
> it’s how we **understand** the world.

And the closer we get to perfect compression, the closer we are to **truth**.

### Summary Takeaway

> **To compress is to understand. To resist compression is to either be noise — or essence.**

🧭 **Why This Matters for What’s Coming Next**

This chapter wasn’t just about compression — it was about uncovering structure, and learning to measure what’s truly essential in data.

In the world of deep learning and language generation, those same questions come back in a new form:
- How do we model structure in language?
- How do we predict what comes next?
- What does it mean for a machine to understand?

As we move forward, we’ll build on this foundation — carrying with us the idea that to learn is, in many ways, to compress.

## References

*   Shannon, C. E. (1948). A Mathematical Theory of Communication. *Bell System Technical Journal, 27*(3), 379-423 [link](https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf).
*   Cover, T. M., & Thomas, J. A. (2006). *Elements of Information Theory* (2nd ed.). Wiley-Interscience [link](https://onlinelibrary.wiley.com/doi/book/10.1002/047174882X).
*   Sayood, K. (2017). *Introduction to Data Compression* (5th ed.). Morgan Kaufmann [link](https://students.aiu.edu/submissions/profiles/resources/onlineBook/E3B9W5_data%20compression%20computer%20information%20technology.pdf).
*   Li, M., & Vitányi, P. (2008). *An Introduction to Kolmogorov Complexity and Its Applications* (3rd ed.). Springer [link](https://link.springer.com/book/10.1007/978-0-387-49820-1).