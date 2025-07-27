---
title: JPEG compression algorithm
date: 19-07-2025
---
JPEG doesn’t save each pixel as-is. Instead, it applies a multi-step process that looks more like intelligent approximation:

## 1. **Block Division**:  
Breaks the image into small blocks (typically $8 \times 8$ pixels).
**Example:** Imagine a simple 16x16 pixel image. This would be divided into four 8x8 blocks. Each block is then processed independently.

## 2. **Frequency Transformation**:  
Applies the **Discrete Cosine Transform (DCT)** to each block.  
The DCT transforms the spatial representation of the image (pixel values) into a frequency representation. This means instead of representing the image as a grid of pixel colors, it's represented as a sum of cosine functions with different frequencies.
-  **Why is this useful?** Most images have a lot of low-frequency information (smooth color changes, gradual shading) and relatively little high-frequency information (sharp edges, fine details). The DCT concentrates the image's energy into a few low-frequency coefficients, which are more important for visual perception.
- **How it works:** The DCT expresses each 8x8 block as a sum of 64 cosine functions, each with a different frequency. The coefficient for each cosine function represents the contribution of that frequency to the overall image block. The top-left coefficient represents the DC component (the average color of the block), while the other coefficients represent increasingly higher frequencies. 

For a block $f(x,y)$, the DCT outputs coefficients $F(u,v)$ using:
$$F(u,v) = \frac{1}{4} C(u)C(v) \sum_{x=0}^{7} \sum_{y=0}^{7} f(x,y) \cos\left[\frac{(2x+1)u\pi}{16}\right] \cos\left[\frac{(2y+1)v\pi}{16}\right]$$
Let's break down this formula:
-   $f(x, y)$: Represents the pixel value at position $(x, y)$ within the 8x8 block.  $x$ and $y$ range from 0 to 7.
-   $F(u, v)$: Represents the DCT coefficient at position $(u, v)$ in the transformed block. $u$ and $v$ also range from 0 to 7, and they represent the horizontal and vertical spatial frequencies, respectively.
-   $C(u)$ and $C(v)$: These are normalization factors:
    -   $C(u) = \frac{1}{\sqrt{2}}$ when $u=0$, otherwise $C(u) = 1$.
    -   $C(v) = \frac{1}{\sqrt{2}}$ when $v=0$, otherwise $C(v) = 1$.
        These factors ensure that the DC component (the average value of the block) is properly scaled.
    -   $\sum_{x=0}^{7} \sum_{y=0}^{7}$: This double summation means we're iterating over all pixels in the 8x8 block.
    -   $\cos\left[\frac{(2x+1)u\pi}{16}\right] \cos\left[\frac{(2y+1)v\pi}{16}\right]$: These are the cosine basis functions. They represent the different frequencies that make up the image block. The formula calculates how much each cosine function contributes to the overall image block.

- **Example:** Imagine an 8x8 block with a smooth gradient from dark to light. After the DCT, the top-left coefficient $F(0,0)$ (the DC component) will have a relatively large value, while the other coefficients will have smaller values. If the block has a sharp edge, some of the higher-frequency coefficients will also have significant values.

## 3. **Quantization**:  
This is the lossy part of JPEG compression, where information is discarded to achieve higher compression ratios. Quantization involves dividing each DCT coefficient by a quantization value and then rounding to the nearest integer.
-   **Why is this useful?** The human eye is more sensitive to low-frequency components than high-frequency components. Quantization takes advantage of this by using smaller quantization values for low-frequency coefficients (preserving them more accurately) and larger quantization values for high-frequency coefficients (discarding them more aggressively). This allows JPEG to discard high-frequency details that are less visually important, resulting in significant compression.
- **How it works:** A quantization table (an 8x8 array of quantization values) is used to determine how much each DCT coefficient is divided by. Standard quantization tables are designed based on psychovisual principles to minimize the perceived distortion. The quality setting in JPEG compression determines the scaling factor applied to the quantization table (lower quality = higher scaling factor = more aggressive quantization).
    $$Q(u,v) = \left\lfloor \frac{F(u,v)}{q(u,v)} \right\rfloor$$
Let's break down this formula:
-   $Q(u, v)$: Represents the quantized DCT coefficient at position $(u, v)$. This is the value after quantization.
-   $F(u, v)$: Represents the DCT coefficient at position $(u, v)$ before quantization.
-   $q(u, v)$: Represents the quantization value at position $(u, v)$ from the quantization table. This value determines how much the corresponding DCT coefficient is divided by.
-   $\left\lfloor \dots \right\rfloor$: This is the floor function, which rounds the result of the division down to the nearest integer. This rounding is what causes the loss of information in the quantization step.

**Example:** Let's say we have a DCT coefficient $F(3, 2) = 25$ and the corresponding quantization value from the quantization table is $q(3, 2) = 10$. Then, the quantized coefficient would be:
$$Q(3, 2) = \left\lfloor \frac{25}{10} \right\rfloor = \left\lfloor 2.5 \right\rfloor = 2$$
Notice that the value has been reduced from 25 to 2, representing a significant reduction in the amount of information stored. The higher the quantization value, the more aggressive the compression and the more information is lost.

## 4. **Entropy Coding**:  
The quantized values are compressed further using **Huffman coding** or **arithmetic coding**, assigning shorter bit patterns to more frequent values.

**Example:** After quantization, many of the DCT coefficients will be zero, especially at higher frequencies. Entropy coding techniques like Huffman coding assign shorter codes to these frequent zero values, further compressing the data.