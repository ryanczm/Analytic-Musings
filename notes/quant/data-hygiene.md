---
layout: post
title: Data Hygiene
category: quant
---

# Overview

Just some notes on data hygiene, or how to analyse data and plot before doing any modelling work. Just my opinions/taste.

# Good Books

Regression & Other Stories by Gelman, Hill, Vehtari, especially the portion on exploring data

# Charting Hygiene

Line/curve plots, scatterplots and histograms are majority of all you need. But mostly lines and scatters.

For line/time series plots, the line thickness and markers matter. For a high frequency time series, you want a thin line to see detail. For a lower frequency time series, use markers like X or triangles with the line. Especially if the intervals are irregular.

For scatterplots, if you have a category to subplot or stratify on, always plot BOTH the facetted subplots and a single scatterplot (stratify with color or markers). Both are always better than one as it lets you isolate out the individual scatter patterns AND also see how they all fit together.

Bar charts are overrated except for a histogram - except for a specific use case of stacked barcharts for generation mixes. 