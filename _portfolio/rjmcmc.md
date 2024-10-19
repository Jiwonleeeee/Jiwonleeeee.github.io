---
title: "Bayesian Estimation of Physical Activity from Wearable Sensor Measurements"
collection: portfolio
date: "2024-10-19"
output: pdf_document
location: Cincinnati
permalink: "/portfolio/rjmcmc"
---
R codes used for this project are posted [here](https://github.com/Jiwonleeeee/BayesActigraph).


This research focused on filtering active states from unlabeled wearable sensor measurements. I show you the result directly so that you can understand the motivation better.
<br/><img src='/images/display.png'>
The left panel shows the raw signal data from a wearable device called `Actigraph'. It measures the acceleration of a wearer's body movement and is used for physical activity analysis. The problem arises when we do not have any labels for this data, i.e., we do not know what a wearer was doing at a give period of time. The right panel is what we can get after applying our method. It is desirable that the measurements in the active states (red) are higher than the ones in the resting states (blue).


Current methods are using somewhat heuristic-based algorithms, for example, the simplest one is using a threshold like this figure:
<br/><img src='/images/threshold.png'>

If we look at the raw data closely, however, you will notice that some measurements within the active states are as low as the measurements in the resting states. This is natural as each measurement was recorded every minute. One possible approach to handle this is to use feature extraction from a particularly specified window. However, this requires to determine the window size and the features to use (e.g. mean or variance). Our goal is to minimize the practitioners decision and to provide the direct tool to use without specifying things. 


We designed an algorithm that does not require any specifications before running. The only thing it needs is the raw signal data directly collected from the device.


I will briefly describe the modeling procedure. Each active state and resting state are defined using the parameters \\(a_1,\ldots,a_B\\) and \\(r_1,\ldots,r_B\\). Since we need to define intervals, we treat an interval starting with \\(a_b\\) as an active state interval and with \\(r_b\\) as a resting state interval, as in this figure:
<br/><img src='/images/statelabel.png'>
To identify the active states is equal to find the location of these intervals \\(a_b,r_b\\) for \\(b=1,\ldots,B\\). The important factor to be considered is that **we need an inference not only about the locations but also the numbers of these intervals**, i.e., we do not have information about \\(B\\) as well. This is where the technique named **reversible jump MCMC** comes in. It allows this varying dimension of the parameter space during the inference procedure. 








