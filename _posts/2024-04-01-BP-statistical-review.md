---
image: "/./assets/images/BPStatreview/cover.png"
layout: post
title: "Understanding Crude & Product Trade Flows: Visualising the BP Statistical Review of World Energy"
category: commods
excerpt: "Every year since 1952, BP publishes a report called the BP Statistical Review of World Energy. In it contains historic data on world energy markets: facts & figures on glboal energy markets from 1960 to 2022. I visualise and analyze the data via charts. My aim is to gain an intuitive geographic feel of global crude and product flows. Who are the biggest exporters? Importers? Producers? Consumers? Where are the sweet/sour/light/heavy crude regions? What are the (inferred) refinery slates across regions? Where does oil flow from country to country? After much wrangling, I produced charts how I see fit to describe the data. For each category, we will understand the sequence of events leading up to the story told in the chart."
---

Every year since 1952, BP publishes a report called the _BP Statistical Review of World Energy_. As of 2022, the custodian is now the Energy Institute, which has taken over BP. In it contains historic data on world energy markets: a facts and figures of the yearly production & consumption of energy and various commodities, from 1960/70 to 2022.

At my past role at General Index, I never had a chance to get a feel of markets, flows and players for oil. What is the geographical distribution of crude, product and gas? Hence, I decided to take the data to produce some charts. From there, I could analyze selected ones, understanding the _who_, _what_, _where_ and _why_ of energy.

My goal is to gain _intuitive geographical feel_ of the global energy markets, focusing on crude, product and natural gas. Who are the biggest exporters? Importers? Producers? Consumers? Where and why are the sweet/sour/light/heavy crude regions? What are the (inferred) refinery slates across regions? Where and from who does oil and gas flow from country to country? 

The data is found [here](). After much wrangling, I produced charts how I see fit to best explain the data. For each category, we will try and understand what the sequence of events leading up to the story told in the chart. We'll build up with simple, boring stuff to more exciting, interesting stuff.

## Table of Contents


<!-- TOC -->
- [Table of Contents](#table-of-contents)
- [Energy Consumption (EJ Y)](#energy-consumption-ej-y)
- [Oil Reserves (BN BBL)](#oil-reserves-bn-bbl)
- [Oil Production (K BBL D)](#oil-production-k-bbl-d)
- [Oil and Product Consumption (K BBL D)](#oil-and-product-consumption-k-bbl-d)
- [Capacity (K BBL D)](#capacity-k-bbl-d)
- [Crude and Product Trade Flows (MN TN)](#crude-and-product-trade-flows-mn-tn)
- [The Platts Periodic Table and Refinery Specs](#the-platts-periodic-table-and-refinery-specs)
- [Conclusion](#conclusion)

<!-- /TOC -->

## Energy Consumption (EJ Y)

One ExaJoule, or $10^{18}J$ is equivalent to $174M$ barrels of oil. In 2022, world energy consumption was $604$ ExaJoules. Our first chart tells the story of energy consumption by region from 1960 to 2022. The main story is that APAC has outstripped the growth rates of all other regions which have remained fairly stagnant. 

<center>
<img src="{{ site.imageurl }}/BPStatReview/1.png" style="width:100%;"/>
</center>

The main driver has been China's growth and a smaller portion of that, India's growth. What happened in 2000? According to the [Economic History of China](), the post AFC China entered the WTO in 2001. As China opened up, it began to fuel export-led growth and rapid industrialization. Taking a look at the top 10 countries:


<!-- <center>
<img src="{{ site.imageurl }}/BPStatReview/2.png" style="width:100%;"/>
</center> -->


<center>
<img src="{{ site.imageurl }}/BPStatReview/3.png" style="width:100%;"/>
</center>

Lastly, we can break down consuption by oil, gas, coal, nuclear, hydro and other renewables. We see that APAC consumes most of energy from coal, then crude. This implies China has a ton of coal plants to generate power as compared to natural gas - probably implying coal is cheaper.

<center>
<img src="{{ site.imageurl }}/BPStatReview/4.png" style="width:100%;"/>
</center>
<center>
<img src="{{ site.imageurl }}/BPStatReview/4A.png" style="width:100%;"/>
</center>



## Oil Reserves (BN BBL)


<center>
<img src="{{ site.imageurl }}/BPStatReview/5.png" style="width:100%;"/>
</center>
Interesting or major discoveries were: Gharais field, Saudi Arabia, 1985. Orinico belt, Venezuela 2006. Athabasca oil sands, Canada 1997. Anyway, moving on to more exciting stuff.
<center>
<img src="{{ site.imageurl }}/BPStatReview/6.png" style="width:100%;"/>
</center>

## Oil Production (K BBL D)

<center>
<img src="{{ site.imageurl }}/BPStatReview/7.png" style="width:100%;"/>
</center>

While the Middle East produces the most crude by far, the story of this chart is clearly the 2010 Shale revolution in the US (brown line). As the story goes, it was technological advancements via fracking and horizontal drilling that let the US access previously inaccessible shale reserves like Eagle Ford shale in the Permian Basin in the Gulf Coast.
<center>
<img src="{{ site.imageurl }}/BPStatReview/8.png" style="width:100%;"/>
</center>

I was pretty surprised to learn that China produced more crude than Iran and further solidifies the power of the US as the top producer. Clearly, a country might have reserves, but economic, technological and political factors determine the ability to access and extract them.

<center>
<img src="{{ site.imageurl }}/BPStatReview/9.png" style="width:100%;"/>
</center>
<center>
<img src="{{ site.imageurl }}/BPStatReview/10.png" style="width:100%;"/>
</center>


## Oil and Product Consumption (K BBL D)
<center>
<img src="{{ site.imageurl }}/BPStatReview/11.png" style="width:100%;"/>
</center>


We can see that consumption of liquids is dominated globally by two countries, the US and China. Interestingly, the data means the US produces and consumes the most crude in the world, speaking to it's geopolitical and economic power.
<center>
<img src="{{ site.imageurl }}/BPStatReview/12.png" style="width:100%;"/>
</center>

We can further breakdown consumption of product by category to reveal regional demand preferences.
<center>
<img src="{{ site.imageurl }}/BPStatReview/13.png" style="width:100%;"/>
</center>
<center>
<img src="{{ site.imageurl }}/BPStatReview/14.png" style="width:100%;"/>
</center>
<center>
<img src="{{ site.imageurl }}/BPStatReview/15.png" style="width:100%;"/>
</center>
APAC is big on petchems, and diesel, presumably due to industrialization. NA has huge gasoline appetite, due to the heavy use of gasoline cars. Europe is huge on diesel. When I investigated, I realized this was due to policymaking in the 90s where European governments subsidized diesel, leading to an adoption of diesel cars over gasoline.

## Capacity (K BBL D)

Lastly, we can examine refining capacity. No surprise to see US and China dominate.

<center>
<img src="{{ site.imageurl }}/BPStatReview/16.png" style="width:100%;"/>
</center>
In Asia, China dominates by a large margin followed by India. There is relatively less refining capacity in the Middle East, implying that these countries prefer to focus on producing. In Europe, Germany, Italy and Spain are the top players. Singapore's heavy investment into refienry despite it's small size clearly shows. Okay now on to the interesting stuff!

## Crude and Product Trade Flows (MN TN)

So, we've gotten an idea of energy consumption, crude production, consumption and refining capacity. None of these tell us specifics yet. I wrangled the trade flows data for 2022 and produced a heatmap. There's alot to be deduced from it. We can look at the export-import flow of each country in detail.

We sort by region and then countries. The colormap is chosen to reflect dark for high flow and light for low flow. To look at where a country exports to, pick a _row_. To look at where it imports crude from, look at a _single column_.

<center>
<img src="{{ site.imageurl }}/BPStatReview/17.png" style="width:100%;"/>
</center>

Analyzing the trade flows, we can see the patterns emerge:

* Exports
    * Heavy sours from Canada (oil sands), Mexico, South America into the US.
    * US exports (presumably light sweet) to neighbours but majority to Europe, and other Asia Pacific (Japan?). Smaller amounts to China and India (presumably distance related?)
    * South American crudes are to US, Europe and China.
    * Russian and Middle East crude, flow to Europe and APAC. However, with the Ukraine war, exports have reduced since the previous yesterday.
    * North and West Africa flows to Europe and APAC
* Imports
    * Heavier, sourer crudes from Canada and South America into US.
    * Europe imports from all regions.
    * APAC (China and India) mostly import from the ME, CIS and some from South America.

<center>
<img src="{{ site.imageurl }}/BPStatReview/18.png" style="width:100%;"/>
</center>


Putting it together, we can see the rough geographic structure of crude flows. The BP map provides more color. We can see the export hubs of Russia and ME and the import hubs of APAC and Europe. The US is special in that it both imports and exports heavily. I sought to understand why this structure was so, so I analyzed the crude characteristics per region, but first, let's look at the product flows:
<center>
<img src="{{ site.imageurl }}/BPStatReview/19.png" style="width:100%;"/>
</center>


Unfortunately, the data in the report does not show flows by product, so we have to guess:

* Excess gasoline from European refineries who process light sweet flows to meet US consumption while US refineries in turn ship excess diesel to meet heavy demand in Europe due to processing heavier crudes.
* Strong refining capacity means the US imports heavier crudes from SA and ships the product back to them.
* Other notable product flows are Russian product to Europe and Europe product to Africa (?)
* On the import side, we can notice Singapore being a regional hub in Southeast Asia for refined products, and India being a strong exporter of product.


But what accounts for these flows? 

## The Platts Periodic Table and Refinery Specs

I soon realized the key was to understand the refinery diets and crude characteristics of each region. Platts provides a [_periodic table_]() of crude grades worldwide. I wanted to visualise this: so I manually scraped the data. Red and blue lights are API gravity thresholds for light (>34) and medium (>25). Here is the __interactive plot__:

{% include html_assets/platts.html %}

* African crudes are medium-light and sweet with some sour outliers. We should expect flows into Europe.
* Asian crudes are medium and sweet.
* CIS crudes are light and sweet except for the benchmark crude Urals which is medium sour.
* EU crudes are light and sweet.
* ME crudes are medium sour.
* SA crudes are sour and heavy.
* NA crudes are light and sweet with the exception of Maya and West Canada Select.


Referencing the crude trade flows, it's clear US exports it's light sweet crude from the shale fields to Europe while taking in heavy Canadian and South American/Mexican crudes. The [EIA FAQ]() states that as of Jan 1 2023, there were 129 operable refineries in the US. Only 7 were built after 2010.

Since refineries are complex and difficult to re-configure, processing light and sweet shale oil domestically would mean operating at lower capacities since the distillation columns lighter sections would fill up faster, leaving the heavier sections, and further on, the cracking/coking units are then underutilized. 

At least, this is my understanding from reading _Leffler_ and _Gary_'s nontechnical refining books. 

## Conclusion

I still have parsed the natural gas data and plotted charts, but we'll leave that for another time. To conclude, this post visualises the BP Statistical Review for World Energy charts and the Platts Periodic table of crudes. I've got a decent geographic feel of how things flow at a high level. 

This will be a foundational project to start off for commodities/oil & gas. I've planned two more successive projects: __analyzing a recent market view from a purely discretionary perspective (off an oil analyst's Twitter thread)__ and __building a balance for a product and doing some forecasting/analytics__. This project was done first to give me that basic understanding of oil & gas flows as pre-requisite. See you in the next post!