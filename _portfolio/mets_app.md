---
title: "Possible application of RJMCMC to baseball data"
collection: portfolio
date: "2024-10-30"
output: pdf_document
location: Cincinnati
permalink: "/portfolio/mets_app"
---

Let's consider this hypothetical data about Mets winning and losing scenario. 
<br/><img src='/images/ideascratch.png'>
Possible \\(X_i\\) are:
1. Difference between the starter's historical stats and the stats on that game. If this discrepancy is large (in a negative way), then that game would likely be harder than expected.
2. Batters performance metrics ()
3. The number of errors
3. Home/away
4. Runs made
5. Other stats like stealing bases

For the simplest setting to model this would be using a logistic regression with \\(\beta_1,\ldots,\beta_p\\):

$$
\begin{align}
a &= b + c \\
d &= e + f
\end{align}
$$



$$
\displaylines{
\nabla \cdot E= \frac{\rho}{\epsilon_0} \\\
\nabla \cdot B=0 \\\
\nabla \times E= -\partial_tB \\\
\nabla \times B  = \mu_0 \left(J + \varepsilon_0 \partial_t E \right)
}
$$

