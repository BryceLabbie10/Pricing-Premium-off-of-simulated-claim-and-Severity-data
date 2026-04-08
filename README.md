# Pricing-Premium-off-of-simulated-claim-and-Severity-data
This project's goal was to improve R studio skills by simulating a data set and doing an actuarial analysis of it. It includes simulating data sets using poisson and lognormal Distributions, fitting lognormal and normal distributions to data sets, calculating VaR and TVaR, and estimating total premium.

Data Simulation:

The first step of this project was to simulate a data set to use. A poisson distribution with lambda=2 was used to generate a data set for the number of claims and a lognormal distribution with meanlog=8 and sdlog=1 was used to simulate the severity. The following code was used to simulate a data set of total severity.

A data set of number of claims per policy holder was created using claims<- rpois(10000,2). Then a dataset of the severity of each the claims was created using individual_severity <- rlnorm(n=claims,meanlog=8,sdlog=2). 

Model Fitting:

The "fitdistplus" package was used for the fitdist() formula.

Claims were then fitted to a poisson distribution which resulted in a lambda=2.0003. The individual_severity data set was then fitted to a lognormal,gamma, and weibull distribution. 
  The lognormal resulted in an aic of 162526.1 and a 99th percentile of   
