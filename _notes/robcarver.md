---
layout: default
title: "Rob Carver"
---

# Basic Directional Strategies

* Back-adjustment
    * Stitching together continuous futures price series creates gap on roll dates.
    * Use back-adjustment (Panama canal method) to create continuous series, but trade on original series.
* Risk of Single Contract
    * $ P \cdot M \cdot \sigma_{\%}=\sigma_{p}$. Dollar risk is the notional times percentage risk
    * Percentage risk is tricky.
    * Carver defines it as $\sigma_{\%}=0.3(10Y \space avg)+0.7(\sigma_{\%})$
    * The larger weight is on a 10Y average of annualized risk. The shorter weight is a EWMA32 of returns.
* Trend Strength
    * $R = \frac{T}{\sigma_{p}}= \frac{T}{P \times \sigma_{\%} \div 16}$. The raw forecast is the trend divided by the daily dollar risk
    * $S = R \times 10 \times scalar$ where the scaled forecast is multiplied by 10 and the forecast scalar (absolute value of raw forecast).
    * $\hat{S} = cap(S)$ Carver caps the scaled forecast at $\pm 20$
* Position Scaling
    * $N=\frac{\hat{S} \times C \times IDM \times w \times \tau}{10 \times M \times P \times \sigma_{\%}}$
    * The position is the capped forecast times capital times IDM times weight times risk target divided by 10 times notional times percentage risk.
