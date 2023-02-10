### Project notes

#### Context

- Bought data of now bankrupt insurance company to start your own insurance company
  - In reality you will be out bided by bigger companies at the auction

- Create a premium model for potential clients to earn a profit

- Evaluation will be done using a market simulation where your model will compete against others.

#### Misc

- Insurance companies bring underwriters and inspectors when customers make a claim
- They also have to pay taxes

### Tips

- Do geographic clustering

- Merge the tables (.csv files) into one data frame each for training and testing

- Data imputation using GLM is a good way to model this problem.

- Use SDV to synthetically generate claims data to validate the model.
  - Emulate the original data as close as possible
  - Try to think of what marginal distribution to use for each co-variate

#### Premium formula

These components can be broken down further, however, we are going to use the pure premium method to calculate premiums 
as written in Chapter 8 of Werner & Modlin.

$$ PurePremium = \frac{ExpectedLoss * (1 + LossAllocatedExpenses) + FixedExpensesPerPolicy}{1 - VariableExpensesPerPolicy - ProfitLoading} $$

- $ ExpectedLoss $ the amount of loss the targeted client is predicted to occur.  

- $ LossAllocatedExpenses $ are claim-related expenses that are directly attributable to a specific claim; for
example, fees associated with outside legal counsel hired to defend a claim can be directly
assigned to a specific claim.

- $ FixedExpensesPerPolicy $ day-to-day running expenses for facilitating insurance coverage overall. Commercial rent, server costs, etc. 

- $ VariableExpensesPerPolicy $, expenses which are unrelated to specific policies, and, could be specific to the annual year. E.g. Covid pandemic of 2020-2021 could result in lower insurance claims therefore having a smaller  $ VariableExpensesPerPolicy $ for that annual year. 

- $ ProfitLoading $, similar to a profit margin; you as an executive, get to determine on how much to charge your customers.

Note that you can consolidate the last two variables of interest for the pure premium into one number, ultimately these are the annual business decisions the insurance company has to make. 

#### Modelling expected loss

- Modelling expected loss is the crucial part, this can be done by loss cost model

- An ethical way to do this which also models heterogeneity is to model the frequency and severity separately

- Loss Cost model
  - $$ ExpectedLoss = Y_s * Y_f $$

- Frequency Model
  - Model should be able to predict many zeros
  - The frequency of claims refers to the number of claims can occur for a particular client. We can denote this by $Y_f$. 
  - To model $Y_f$ we should consider distributions that model count data.
    - Poisson 
    - Negative Binomial 
    - Binomial 
    - Zero-inflated Poisson 
  - The most standard approach for modelling such data is through the use of GLM's

- Severity Model
  - The Severity of claims refers to the monetary loss of an insurance claim. We denote this by $Y_s$. 
  - To model $Y_s$ we should consider distributions that model continuos data. 
    - Gaussian (log-link)
    - Gamma (log-link)
    - Log-normal 
    - Variance-Gamma 
    - Generalized Hyperbolic 
  - Again, a GLM framework can be used to model this.

#### Report

- If you go bankrupt, then you will have to write a report explaining:
  - Why you got bankrupt
  - How your model could have been improved

### Lecture notes

$$\mathbb{E}[\textit{loss}] = \mathbb{E}[Y_s Y_f]$$

- Twidie distribution
  - $Y_s$ (severity) and $Y_f$ (frequency) are modelled separately

- In practice, we model them separately

- Geographic clusters can be used along with mixture modelling

- Use different co-variates for $Y_f$ and $Y_s$

- Use different clusters for $Y_f$ and $Y_s$

- Contamination means heterogeneity

- Cluster weights

$$ \hat{z_{ig}} = \pi_g f(X;\theta_g) / \sum_{h} \pi_h f(X;\theta_g) $$

- Expected loss

$$ \mathbb{E}[Y_s Y_f] = \mathbb{E}[Y_s] \mathbb{E}[Y_f] $$

- Soft classification is more reliable for points near the center of two clusters

- Underwriters know their geographic location very well, they are experts in estimating risk
  - They decide what to put on their portfolio

- We can assume $Y_s$ and $Y_f$ are independent, though it might not seem intuitive
  - It is an ethical responsibility to model heterogeneity

- Synthetic Data Generation
  - SDV Python library
  - Gaussian Copula
  - Need to pick marginals correctly