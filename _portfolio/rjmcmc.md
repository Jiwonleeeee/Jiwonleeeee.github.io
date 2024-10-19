---
title: "Bayesian Estimation of Physical Activity from Wearable Sensor Measurements"
collection: portfolio
date: "2024-10-19"
output: pdf_document
location: Cincinnati
permalink: "/portfolio/rjmcmc"
---
R codes used for this project are posted [here](https://github.com/Jiwonleeeee/BayesActigraph).

This research focused on filtering active states from unlabeled wearable sensor measurements. I show you the result directly so that you can better understand the motivation.
<br/><img src='/images/display.png'>
The left panel shows the raw signal data from a wearable device called `Actigraph`. It measures the acceleration of a wearer's body movements and is used for physical activity analysis. The problem arises when we do not have any labels for this data, i.e., we do not know what the wearer was doing at a given period of time. The right panel shows the result after applying our method. It is desirable that the measurements in the active states (red) are higher than those in the resting states (blue).

Current methods use somewhat heuristic-based algorithms. For example, the simplest one involves using a threshold, as shown in this figure:
<br/><img src='/images/threshold.png'>

However, if we look closely at the raw data, we notice that some measurements within the active states are as low as those in the resting states. This is natural, as each measurement was recorded every minute. One possible approach to handle this is to extract features from a specifically defined window. However, this requires determining the window size and which features to use (e.g., mean or variance). Our goal is to minimize the practitioner's decisions and provide a tool that can be used directly, without specifying these parameters.

We designed an algorithm that does not require any prior specifications before running. **The only thing it needs is the raw signal data directly collected from the device.**

I will briefly describe the modeling procedure. Each active state and resting state is defined using the parameters \\(a_1,\ldots,a_B\\) and \\(r_1,\ldots,r_B\\). Since we need to define intervals, we treat an interval starting with \\(a_b\\) as an active state interval and one starting with \\(r_b\\) as a resting state interval, as shown in this figure:
<br/><img src='/images/statelabel.png'>
Identifying the active states is equivalent to finding the location of these intervals \\((a_b,r_b]\\) for \\(b=1,\ldots,B\\). The important factor to consider is that **we need to infer not only the locations but also the number of these intervals**, i.e., we do not have information about \\(B\\) either. This is where the technique named **reversible jump MCMC** comes in. It allows for a varying dimension of the parameter space during the inference procedure.

# What information can help decide the location of the intervals?
We assume the likelihood functions for each active and resting state are multivariate normal and Poisson, respectively. For each possible move, we compare the change in likelihood and accept the move if the change is significant enough. We run this algorithm multiple times until it converges. The move types are: update, birth, and death.
<br/><img src='/images/movetype.png'>

This is illustrated in this video. You can see that for extreme measurements, the state is barely changed, but for ambiguous ones, there is more flexibility:

<iframe width="560" height="315" 
src="https://www.youtube.com/embed/b6m08gw1AYU" 
title="YouTube video player" frameborder="0" 
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
allowfullscreen></iframe>

This method is highly flexible and can be applied to data from different wearers. For example, current metrics mostly focus on high-functioning adults, but the subjects in the data we used are individuals with cerebral palsy, and some can barely walk. The practical use of this algorithm allows practitioners to apply it to raw data, collect measurements from the active states, and derive their own metrics based on them.
