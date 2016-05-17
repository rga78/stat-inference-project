
# Coursera: Statistical Inference: Week 1: Probability


## Probability Basics

* 0 < p < 1
* p, 1-p
* P(A a B) = P(A) * P(B) 
    * ONLY IF A and B are INDEPENDENT
* P(A u B) = P(A) + P(B) - P(A a B) 
    * individual probabilites minus joint probability
* P(A u B) = P(A) + P(B) 
    * A and B mutually exclusive, P(A a B) = 0

Probability underlies the formal process of statistical inference,
using sample statistics (median, mean, sd of the sample) as estimates for 
overall population statistics.


### Probability Mass Function / Probability Density Function

* Probability Mass Function
    * for discrete random variables 
    * probability assigned to every possible outcome
* Probability Density Function
    * for continuous random variables
    * probability assigned to a range of outcomes



### Various

* **bounded** random variable: max and min values
* **unbounded** random variable: no min or max
* **binomial** 
    * canonical PMF for flipping a coin
* **poisson** 
    * canonical PMF for dealing with counts (unbounded)
* **bernouli:**
    * p(x) = t^x (1 - t)^(1-x) for x=0,1
    * where t is the probability of one of the two outcomes


R Example:

    pbeta() // beta is a pdf. p is for probability
    qbeta() // q is for quantile
    qunif()




## Conditional probability

P(A|B) = the probability of A occuring GIVEN THAT B has occurred

    P(A | B) = P(A a B) 
               --------
                 P(B)

             = P(A)P(B)     # if A and B are independent
               -------
                 P(B) 

             = P(A)         # as expected, if A and B are independent



### Bayes' rule:

P(B|A) = the probability of B occurring GIVEN THAT A has occurred.
Equal to the probabilty of A occurring, given B has occurred, 
times the probability of B occurring,
all divided by the sum of the above probability, 
plus the probabiliyt of A occurring given that B's complement has occurred,
times the probabiliby of B's complement occurring.

So, in other words, there's a probability that A will occur given that B has
occurred and a probability that A will occur given that B's complement has occurred,
and these two probabilities are modulated w/r/t the probability that B or B's
complement will occur at all.  The portion of that total probability that represents 
the probability A will occur given that B has occurred, is equal to the
probability B will occur given A has occurred.


    P(B | A) =       P(A | B) * P(B)
               -----------------------------
               P(A|B) P(B) + P(A | Bc) P(Bc)


* Bc = B's complement?


### Sensitivity, Specificity, Predictive Value

* \+ = positive test result 
* \- = negative test result
* D = subject has condition
* Dc = subject does NOT have condition
* **Sensitivity** = P(+ | D)
    * probability of a positive test given the subject has the condition
* **Specificity** = P(- | Dc)
    * probability of a negative test given the subject does NOT have the condition
* **Positive predictive value** = P(D | +)
    * probability subject has the condition given the test is positive
* **Negative predictive value** = P(Dc | -)
    * probability subject does NOT have the condition given the test is negative
* Prevalence of condition in population = P(D)
* P(+|Dc) = 1 - P(-|Dc) 
    * probability of a positive test given that the subject does NOT have the condition
    * Type 1 error: "false positive" = 1 - specificity
* P(-|D) = 1 - P(+|D)  
    * probability of a negative test given that the subject has the condition
    * Type 2 error: "false negative" = 1 - sensitivity
* P(Dc) = 1-P(D) 
    * probability of NOT having condition is 1 minus probability of having condition

.


    P(D | +) =         P(+|D)*P(D)
               -----------------------------
                P(+|D)*P(D) + P(+|Dc)*P(Dc)


In other words...

* the probability of having the condition 
    * given a positive test result, P(D|+)
* equals the probabilty of a positive test result
    * given the subject has the condition (true positive), P(+|D)
    * multiplied by the probability of having the condition, P(D)
* all divided by..
* the sum of..
    * the numerator (probability of positive test result given subject has the condition)
    * plus the probability of a positive test result 
        * given the subject does NOT have the condition (false positive), P(+|Dc)
        * multipled by the probability of NOT having the condition, P(Dc)


Or in other words...

* the probability of having the condition
    * given a positive test, P(D|+)
* is the probability of a positive test being true, P(+|D)
    * times the overall probability of having the condition, P(D)
* divided by the sum of...
    * (the numerator): the probability of the test being true, P(+|D)
        * times the overall probability of having the condition, P(D)
    * (the "opposite" of the numerator): the probability of the test being FALSE, P(+|Dc)
        * times the overall probability of NOT having the condition, P(Dc)
* i.e, whats the proportion of...
    * the overall probability of getting a positive test (denominator)
    * that corresponds with having the condition (numerator)
    
.

    P(Dc | -) =       P(-|Dc)*P(Dc)
                 ---------------------------
                 P(-|Dc)*P(Dc) + P(-|D)*P(D)


In other words...

* the probability of NOT having the condition
    * given a negative test, P(Dc|-)
* is the probability of a negative test being true, P(-|Dc)
    * times the overall probability of NOT having the condition, P(Dc)
* divided by the sum of...
    * (the numerator): the probability of the test being true, P(-|Dc)
        * times the overall probability of NOT having the condition, P(Dc)
    * (the "opposite" of the numerator): the probability of the test being FALSE, P(-|D)
        * times the overall probability of having the condition, P(D)
* i.e, whats the proportion of...
    * the overall probability of getting a negative test, P(-|Dc)*P(Dc) + P(-|D)*P(D)
    * that corresponds with NOT having the condition, P(-|Dc)*P(Dc)

.

    P(D | +) =      P(+|D)*P(D) 
               ---------------------------
                P(+|D)*P(D) + P(+|Dc)*P(Dc)


In other words...

* the probability of having the condition
    * given a positive test, P(D|+)
* is equal to the probability of the test being TRUE, P(+|D)
    * multiplied by the overall probability of having the condition, P(D)
* divided by the sum of...
    * (the numerator)
    * the probability of the test being FALSE, P(+|Dc)
        * multiplied by the probability  of not having the condition, P(Dc)
* i.e., the proportion of...
    * the overall probability of a positive test (denominator)
    * that corresponds with having the condition (numerator)

.

    P(Dc | +) =      P(+|Dc)*P(Dc) 
                --------------------------
                P(+|D)*P(D) + P(+|Dc)*P(Dc)         # <-- denom is the same as P(D|+)
    

In other words...

* the probability of NOT having the condition
    * given a positive test, P(Dc|+)
* is equal to the probability of the test being FALSE, P(+|Dc)
    * multiplied by the overall probability of NOT having the condition
* divided by the sum of...
    * (the numerator)
    * the probability of the test being TRUE, P(+|D)
        * multiplied by the overall probability of having the condition
* i.e, whats the proportion of...
    * the overall probability of getting a positive test (denominator)
    * that corresponds with NOT having the condition (numerator)

.

    P(D|+)        P(+|D)*P(D)
    ------ =     --------------
    P(Dc|+)      P(+|Dc)*P(Dc)      



### Diagnostic Likelihood Ratio


                  P(+|D)     P(D)
    DLR (odds) = -------- * ------
                  P(+|Dc)    P(Dc)



## ODDS

    odds = p / 1-p
    p = ODDS / 1+ODDS

E.g odds of flipping a coin and getting a head:

    p = 0.5 
    1-p = 0.5 
    odds = p / 1-p
    odds = 0.5 / 0.5 = 1:1 --> EVEN ODDS (NOT A PROBABILITY!!!)

E.g. odds of the roulette number being 1-12 (out of 36):

    p = 0.333 / 0.666 = 1:2

The ODDS is the relative value of p to NOT-p.


## Measures of Centrality (mean) and Dispersion (variance)

* Expected Value: E[X] = sum [xi * p(xi)]
    * the long-run average value of repetitions of the experiment 
* VARIANCE = E[X^2] - E[X]^2 = sum [ xi^2 * p(xi) ] - E[X]^2
    * properties of distributions
* **Population mean** is "center of mass" of pop
* **Sample mean** is "center of mass" of observed sample data
    * sample mean is estimate of pop mean
    * sample mean is unbiased
    * sample-means distribution mean approximates the population mean
    * more data in the sample-mean distribution, the more concentrated its pdf/pmf is around the population mean


