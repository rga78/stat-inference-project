

# Coursera: Statistical Inference: Week 4: Power, Bootstrapping


## Power 

* "Power" = the probability of correctly rejecting the null hypothesis when it is in fact false
* b (beta) = probability of type 2 error (false miss)
* power = 1 - b
    * i.e, the probability of NOT getting a false miss
    * i.e, the probability of getting a HIT (assuming one actually exists)
* a (alpha) = probability of type 1 error (false hit)
* assume the expected value of the test sample avg, ua
* then plug ua into the equations
* basically you have a normal centered around u, and a normal centered around ua.
* draw a vertical line at a=0.05 in first normal. 
* compute area from second normal at same line to get power
* useful for planning a study 
    * i.e what n do i need for a power of 90%?

.

    1-b = P(X' > u0 + z(1-a) * sd / sqrt(n); u = ua)
    1-b = P(X' > u0 + z-quantile * se; assuming u = ua)

For gaussian data:

    1-b = pnorm(u0 + z-quantile * se, mean = ua, sd=se, lower.tail=F) // P(X' > u0 + z-quantile * se)

...where:

    X' ~ N(ua,sd^2/n)

    Power= (ua-u0) / sd / sqrt(n) = (ua-u0) / se ???


### Effect Size

Effect Size is the "size of the effect", or the magnitude of the difference
between the means of the two samples being compared.

    Effect Size = ua-u0 / sd
    # the diff in means, in sd units


### Calculating n based on desired power

* under H0, X' is N(u0,se)
* under Ha, X' is N(ua,se)
* we reject H0 if 
    * X' >= z(0.95) * se
    * which under H0 has prob=0.05 
    * P( X' >= z(0.95) * se )
* under Ha it has prob:
    * P( X'-(ua-u0) / se >= (ua-u0)/ se + z(0.95))
    * P(Z >= (ua-u0) / se + z(0.95))
* desired power= 0.8
    * z(0.2) = (ua-u0) / se + z(0.95)


R Example: 

    power.t.test
    power.t.test(n=16, delta=0.5, sd=1, type="one.sample", alt="one.sided")
    power.t.test(n=16, delta=2, sd=4, type="one.sample", alt="one.sided")
    power.t.test(n=16, delta=100, sd=200, type="one.sample", alt="one.sided")

    # delta = ua-u0
    # All result in the same power: 0.604
    # because power depends on effect size = delta / sd



## Multiple Comparisons 

* Every time you test for p values...
* there's a small probability of getting it wrong 
    * (that's what the p value indicates)
* Doing multiple tests **increases the chance** that one of those tests will have it wrong
* type I error (false positive) = V
* type II error (false negative) = T
* False positive rate - rate at which type 1 errors are called "significant": E[V/?]
* Family wise error rate FWER - probability of at least one type 1 error: P(V>=1)
* False discovery rate = rate at which claims of significance are false: E[V/R]
* R = number of times you claimed significance


R Example: 

    # TODO
    # Controls FWER
    sum(p.adjust(pValues,method="bonferroni") < 0.05)
    # = 0 (no true positives in data)


    # Controls FDR
    sum(p.adjust(pValues, method="BH") < 0.05)
    # =0



## Bootstrapping

* Sample observed data w/ replacement 
    * i.e, take samples from your sample, WITH REPLACEMENT!
    * (without replacement, you've merely recreated the observed data)
* Approximates sampling distribution
    * as far as the observed data is able to approx the true pop dist
* Use this sampling dist to calculate...
    * confidence intervals
    * standard error
    * etc of the statistic

R example:

    x <- some vector
    n <- length(x)
    B <- 10000 # of "resamples"
    resamples <- matrix( sample(x, n*B, replace=T), rows=B, cols=n)
    medians <- apply(resamples, 1, median)
    sd(medians) // standard error of median statistic
    quantile(medians, c(0.025, 0.975)) // 95% conf interval
    
    g <- ggplot(data.frame(medians=medians), aes(x=medians))
    g <- g + geom_histogram(color="black", fill="lightblue", binwidth=0.05)
    // plots the ESTIMATE of the sample-medians distribution (i.e sampling distribution of the median)



## Group comparisons - Permutations

* Comparing two groups
* trying to determine if the groups differ significantly
* randomly assign results to the two groups 
    * i.e break the connection between each result and its group
    * i.e mix up all the results across the two groups - permutations
* repeat for many simulated permutations
* see how many permutations produced as significant a result as the original data
    * to reject or accept the null hypothesis



