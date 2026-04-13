# Pricing-Premium-off-of-simulated-claim-and-Severity-data
This project's goal was to improve R studio skills by simulating a data set and doing an actuarial analysis of it. It includes simulating data sets using poisson and lognormal Distributions, fitting lognormal and normal distributions to data sets, calculating VaR and TVaR, and estimating total premium.

Data Simulation:

The first step of this project was to simulate a data set to use. A poisson distribution with lambda=2 was used to generate a data set for the number of claims and a lognormal distribution with meanlog=8 and sdlog=2 was used to simulate the severity. The following code was used to simulate a data set of total severity.

A data set of number of claims per policy holder was created using claims<- rpois(10000,2). Then a dataset of the severity of each the claims was created using individual_severity <- rlnorm(n=claims,meanlog=8,sdlog=2). 

Model Fitting:

The "fitdistplus" package was used for the fitdist() formula.

  Claims were then fitted to a poisson distribution which resulted in a lambda=2.0003. The individual_severity data set was then fitted to a lognormal,gamma, and weibull distribution. 
  The lognormal resulted in an aic of 162526.1 and a 99th percentile of 30726.4. The Gamma resulted in an aic of 165780.2 and a 99th percentile of 29805.05. The weibull resulted in  an aic of 163911.8 and a 99th percentile of 22697.58. The 99th percentile of the actual data set was 30888.97.
    QQ plots were made for each of the 3 fits. These can be found in the "Figures" file. The QQ of the gamma and weibull fits suggest heavy underestimation of the extreme tail values. The lognormal had some variation in the upper quantiles but no clear trend of over or underestimation. 
    With the lowest aic and the more acurate and conservative quantile values, the lognormal fit was chosen for further analysis. 


Total Severity Modeling:

  A monte carlo simulation was used to create a data set of the total severity of claims using the replicate function, the claim model, and the individual severity model. 
  total_severities <- replicate(30000,{N<-rpois(1,fit_claims$estimate[1]*10000);if(N>0){sum(rlnorm(N,meanlog=fit_severity_lnorm$estimate[1],sdlog=fit_severity_lnorm$estimate[2]))} else{0}})
  The code would simulate a claim amount then sum that many simulated severity amounts. This was replicated 30,000 times. A density histogram was plotted to visual a good possible distibution to fit. The histogram can be found in "Figures."  Based on the shape, I decided to test a normal and lognormal fit. Based on the central limit theorem, the total severities is expected to approach a normal distribution as the samples increase. However, the central limit theorem can unaccuratley model the tails, which are vital for the final analysis of the data. So, a lognormal fit was also tested.
  The normal fit had an aic of 307333.2 and a 99th percentile of 101119258. The lognormal fit has an aic of 307330.5 and a 99th percentile of 101148393. Both the QQ plots had extremely little variation. The 99th percentile of the data set was 101179227. Both of the models underestimated the upper quantiles but the lognormal was much closer. Based on this the lognormal was chosen to model the total severities. 

Estimation of total Premiums:
The following equation was used to estimate a simple total premium*

  Premium=(Expected_Total_Severity+TVaR)*1.3

The tail value at risk was determined at a 99% confidence and the 30% adjustment was added to account for operating costs. The 99th percentile of the lognormal fit was 101148393 so, using the integrate function, the expected value given the the severity exceeds 101148393. This resulted in a TVaR value of 101148393. 
Using the formula for the expected value of a lognormal distributions, the expected total severity was calculated as 98464617.
Then the total premium was calculated as 260,065,267.

*A very simplified premium equation was used since the main objective of this project was to use distribution fits and Monte Carlo Simulations
