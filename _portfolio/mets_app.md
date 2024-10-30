---
title: "Possible application of RJMCMC to baseball data"
collection: portfolio
date: "2024-10-30"
output: pdf_document
location: Cincinnati
permalink: "/portfolio/mets_app"
---

Let's consider this hypothetical data about Mets winning and losing scenarios. 
<br/><img src='/images/ideascratch.png'>
Possible \\(X_i\\) are:
1. Difference between the starter's historical stats and the stats in that game. If this discrepancy is large (in a negative way), then that game would likely be harder than expected.
2. Batters' performance metrics
3. The number of errors
3. Home/away 
4. Runs scored
5. Other stats like stolen bases

For the simplest setting, modeling this would involve using logistic regression with \\(\beta_1,\ldots,\beta_p\\):

$$
\begin{align}
y_i &\sim \text{Ber}(\pi_i) \\
\pi_i &= \text{logic}\left(\beta_0 + \beta_1 X_{i1} + \ldots + \beta_p X_{ip} \right)
\end{align}
$$

What if there a suspicious point where the effect of some \\(X_{ij}\\) might change, i.e., \\(\beta_j\\) should change before and after this point. Let \\(\boldmath{\beta^t} = \beta_1^t,\ldots, \beta_p^t\\), then the idea is to use a different set of \\(\boldmath{\beta^t}) for each possible \\(t\\) as shown in the figure below:
<br/><img src='/images/beforedeath.png'>
The parameters are \\(A,B, \boldmath{\beta^1}, \boldmath{\beta^2}, \boldmath{\beta^3}\\). Let's assume this is the initial state, and consider what happens if the model tries to combine the last two sections. Like the usual MCMC using the Metropolis-Hastings (MH) scheme, we need to propose possible values for the parameters for this move; i.e., we want to see if the scenario below makes more sense:
<br/><img src='/images/afterdeath.png'>
Let \\(\theta=(A,B, \boldmath{\beta^1}, \boldmath{\beta^2}, \boldmath{\beta^3})\\) be a set of the parameters at a current iteration \\(s\\), and \\(\hat{\theta}=(A, \boldmath{\beta^1}, \boldmath{\beta'^2})\\) be the proposed parameter corresponding the above scenario at \\(s+1\\). 

The basic idea is similar to the original MH step, i.e., we are going to evaluate the acceptance probability for this particular proposal. The proposal ratio needs adjustment, and additional terms are needed to account for the difference in dimension between \\(\hat{\theta}\\) and \\(\theta\\), but the procedure remains the same.
<br/><img src='/images/accprob.png'>


This approach is similar to using random slopes or including a time effect in the model but has some advantages:

1. Whether we use different slopes or not is determined in a fully data-driven way.
2. The number of coefficients does not have to be the same for each period; i.e., we can include an additional covariate if needed. For example, during the COVID season, we might want to use information about whether main players contracted COVID.

Its extension to possible predictions for a future season remains unsolved yet, this approach can help us understand dynamics in the historical data, potentially leading to a better understanding of what influenced the Mets' wins.























