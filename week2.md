
# Coursera: Statistical Inference: Week 2: Sample Statistics, Distributions


## Expected Value, Variance, standard deviation, standard error 

variance is a measure of "spread"

    Var(X) = E[(X-u)^2] = E[X^2] - E[X]^2
    sd = sqrt(Var(X))

    E[X^2] = x.SUM [ x^2 * p(x) ]
    where x is an outcome of the random variable X


Var(X) for a binomial distribution

    E[X] = 0 * (1-p) + 1 * p = p
    E[X^2] = 0^2 * (1-p) + 1^2 * p = p = E[X]
    Var(X) = E[X^2] - E[X]^2 = p - p^2 = p(1-p) <--- well known formula


Variance:

    sd^2 = i.SUM[ (Xi - u)^2 ]
          --------------------
                n

Sample variance:

    S^2 = (i-1).SUM [ (Xi - X')^2 ] 
            ------------------------
                   n - 1                <-- for bias in low levels of n


* X is a random variable
* E[X] the expected value of the random variable X
    * i.e the long-run average of multiple outcomes
* X' is the **sample mean**
    * WHICH IS ALSO a random variable
    * i.e it has a distribution, expected value, etc. 
* E[X'] is the expected value of the random variable X'
    * i.e. the long-run average of multiple samples
    * i.e. expected value of the sampling distribution of the mean 
* E[X'] = expected value (mean) of sample-means distribution
    * the mean (expected value) from multiple samples
    * the mean of the...
        * distribution of sample means, X'
* u = mean of the population
* E[X'] = u 
    * i.e. the mean of the sampling distribution of the mean approximates the population mean

* Var(X') = variance of sample-means distribution

.

    Var(X') = Var(X) / n
            = sd^2 / n
            = sd / sqrt(n)


* sd^2 = variance of population
* sd = std dev of population
* SE = standard error
    * Note: std dev of a statistic, e.g Var(X') (actually sqrt(Var(X')), is called a standard error
* SE of the mean 
    * se =  sd / sqrt(n) 
    * sd = stddev of underlying population
    * se = the std dev of the sample-means distribution


### Summary:

* The sample variance estimates the population variance
* The distribution of sample variance is centered at what it estimates
    * i.e the population variance ("unbiased")
    * gets more concentrated with more data
* The Variance of the sample meanS, Var(X')
    * is the population variance, sd^2, divided by n
    * Var(X') = sd^2  / n
                    


## Normal distribution

    E[X] = u
    Var(X) = sd^2

Standard normal distr (Z-scale)

    u=0
    sd=1 
    
    Z = X-u/sd ~ N(u=0,sd=1) (normal dist with u=0, sd=1)
    X = u + sd*Z ~ N(u,sd^2)
    
    68% fall within +/- 1 sd
    95% within +/- 2 sd
    99% within +/- 3 sd
    
    -1.28 sd = 10th quantile 
    1.28 sd = 90th quantile
    1.645 sd = 95th
    1.96 sd = 97.5th
    2.33 sd = 99th


R Example:

    # probability of the random value being 93 OR LESS
    pnorm(93,mean=100,sd=10)    

    # probability of the random value being 93 OR MORE
    1-pnorm(93,mean=100,sd=10) 

    # standard normal (u=0,sd=1) probability of RV being -0.7 sd OR LESS
    pnorm(-0.7) 
    
    # the RV value of the 95th percentile (95% of RVs are LESS)
    qnorm(0.95, mean=100, sd=10) 


## Bernoulli distribution

* binary outcome, 0 or 1
* p(1) = p
* p(0) = 1-p
* P(X=x) = p^x * (1-p)^(1-x)
* mean = E[X] = = p
* Var(X) = p * (1-p)


## Binomial random variable

* sum of bernoulli trials
* P(X=x) = (n C x) * p^x * (1-p)^(n-x)
    * (n C x) - number of different ways to choose n out of x, WITH REPLACEMENT
    * p^x : "hits"
    * (1-p)^(n-x): "misses"

.

    (n C x) =   n!      <--- number of ways to choose n
              --------
               x!(n-x)! <--- number of ways to choose x * number of ways to chose "NOT x"


R Example: 

    choose(n,x)

    # probability of x OR FEWER hits in n trials
    pbinom(x, prob=p, size=n) 

    # probability of MORE THAN x hits (lower.tail=F) in n trials
    pbinom(x, prob=p, size=n, lower.tail=F) 

    # probability of X OR MORE hits in n trials (lower.tail=F) 
    pbinom(x-1, prob=p, size=n, lower.tail=F) 



## Poisson distribution

* for modeling counts
* modeling event-time or "survival" data
* modeling contingency tables
* approximating binomials when n is LARGE and p is small

.

    # x takes on non-negative values
    P(X=x;lambda) = lambda^x * e^-lambda
                   --------------------
                        x!

    mean = lambda
    variance = lambda


* X ~ Poisson(lambda * t)
    * lambda = E[X/t] is the expected COUNT per unit of time
    * t is the total monitoring time


R Example: 

    # probability of x OR FEWER occurrences at rate lambda
    # if lambda / t, then multiply by total t (i.e 3.5/hour * 3 hours)
    ppois(x, lambda= lambda/t * t)          



## Exponential distribution

* For modeling the time between events in Poisson processes; 
* i.e. a process in which events occur continuously and independently at a constant avg rate.

.

    P(X=x,lambda) = lambda * e^-lambda*x
    
    E[X] = 1/lambda
    Var(X) = 1/lambda^2
    sd = 1/lambda
    
    E[X^n] = n! / lambda^n
    
    "memoryless"




