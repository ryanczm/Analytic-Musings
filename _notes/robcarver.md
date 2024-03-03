---
layout: default
title: "Rob Carver"
---

# Single Instrument Basics

* Back-adjustment
    * Stitching together continuous futures price series creates gap on roll dates.
    * Use back-adjustment (Panama canal method) to create continuous series, but trade on original series.
* Risk of Single Contract
    * $\$N \cdot \sigma_{\%y}=\sigma_{\$y}$ where $N$ is notional, $\sigma_{\% y}$ is annualized standard deviation in percentage points. Gives dollar risk of single contract $\sigma_{\$y}$.
* Trend Strength
    * $R = \frac{T}{\sigma_{\$d}}= \frac{T}{P \times \sigma_{\% y} \div 16}$. The raw forecast is the trend divided by the daily dollar risk
    * $S = R \times 10 \times scalar$ where the scaled forecast is multiplied by 10 and the forecast scalar (absolute value of raw forecast).
    * $\hat{S} = cap(S)$ Carver caps the scaled forecast at $\pm 20$
* Position Scaling
    * $N=\frac{\hat{S} \times C \times IDM \times w \times \tau}{10 \times M \times P \times \sigma_{\%}}$
    * The position is the capped forecast times capital times IDM times weight times risk target divided by 10 times notional times percentage risk.
