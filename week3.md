
# Coursera: Statistical Inference: Week 3: T-tests


## t-tests

* for comparing two datasets to each other 
* to determine whether the means are significantly different
* useful for small datasets (small sample size n)
* types of datasets:
    * independent / unpaired samples: samples are independent; have no related characteristics between them
    * paired samples: samples have related characteristics
        * "repeated measures": test the same subject twice (e.g before and after)
        * "matched-pairs sample": match the two samples based on related characteristics (e.g. other variables)
* normal distributions are described by mean + sd.
* t-distributions are described by single value: "degrees of freedom"
    * usually dof = n-1
    * as d-o-f increases (n increases), approximates a standard normal
* assumes underlying data is iid and gaussian distributed (e.g a sampling distribution)
    * why?

.

    X' - u         <-- sample means - pop mean
    ------
    S / sqrt(n)     <-- estimated SE; S = sd(X') = SE of the mean = SE = sd / sqrt(n). 


* because this equation is not normally distributed. 
* since S is an estimate of sd (based on the sample data)
    * it tends to not quite approximate a normal dist 
        * especially at low values of n.
    * tends to undershoot
    * dof (n-1) and t-dist corrects for undershoot

* if we replaced S with sd (the actual pop sd),
    * then it would be normally distributed.
    * since pop sd is typically unknown, we usually are working with the estimate, S, based on sample data


## Computing t-test confidence intervals:

### Paired groups:

    X' +- t(q,n-1) * S / sqrt(n) 
    X' +- t(q,n-1) * se

* t(q,n-1) applies the relevant quantile (q) to the given t distribution (n-1)
* Note: q=0.975 for 95% interval (two-tailed)
* S is the sample stddev (sd of X)
* S / sqrt(n) is the se of X'
* Note: when n is very large, t approximates Z quantiles (Z = standard normal distribution)
* this can be rearranged:

.

    ( X' - u0 )      X' - u0
    -------------  = ----------  = t(q,n-1) 
     s/sqrt(n)          se


### Independent groups, EQUAL VARIANCES:


    Y'-X' +- t(q, nx + ny - 2) * Sp * ( 1/nx + 1/ny ) ^ 1/2
    Y'-X' +- t(q, nx + ny - 2) * Sp * sqrt( 1/nx + 1/ny )
    
    t = (Y' - X') / (Sp * sqrt(1/nx + 1/ny))
    df = nx + ny - 2

    // i.e the difference between group means, + or - a range, where
    // the range is based on the t-dist-quantile, for a desired conf int 
    // and a given d-o-f (R: qt(q,(nx+ny-2)), multiplied by the "pooled std dev"
    // across the two groups times the sqrt of the pooled sample sizes.
    // The last part more or less resembles the SE = S / sqrt(n)
    // Diff from paired:
         - t dist d-o-f is different
         - "se" is computed differently

...where

    (1-a) * 100% is the conf interval for uy - ux; i.e, 95% = (1-0.5); 1-a/2 = 0.975 for 95% conf int
    dof = nx + ny - 2
    Sp is the pooled std dev

    Sp^2 = pooled VARIANCE = (nx-1) * Sx^2 + (ny-1) * Sy^2 
                            ------------------------------
                                    (nx + ny - 2)
     Note: if nx == ny, then it's just the average of the two

    NOTE: assumes a (roughly) EQUAL variance across the two groups
    Note: when n is very large, t approximates Z quantiles (approx normal distribution)


## Z-TEST:

    Y'-X' +- Z(q) * Sp * sqrt(1/nx + 1/ny)

    diff.in.se.units = z.diff = (Y'-X') / Sp / sqrt(1/nx + 1/ny)

* Gives you the difference between..
    * the Y group mean (Y') 
    * and the X group mean (X')
* expressed in terms of standard-error units 
    * aka Z units for the sample statistic

You can now use pnorm to determine the probability of seeing the difference in means.

    pnorm(-z.diff)                 // probability of getting z-diff or less
    pnorm(z.diff, lower.tail=F)    // probability of getting z-diff or more
    2 * pnorm(-z.diff)             // prob of getting z-diff or more extreme (less or more)

This probability is aka the "p value""


### independent groups, UNEQUAL variances: 

    Y'-X' +- t(df) * (Sx^2/nx + Sy^2/ny) ^ 1/2
    t = (Y' - X') / sqrt( Sx^2/nx + Sy^2/ny )

...where

    t(df) =        ( Sx^2/nx + Sy^2/ny ) ^ 2
            --------------------------------------
            ( Sx^2/nx ) ^ 2       ( Sy^2/ny ) ^ 2
            ----------------  +  -----------------
                (nx - 1)            (ny - 1)

NOTE: if nx and ny are large, then t dist approximates normal, so you can use Z quantile instead of t quantile

R Example: 

    dof <- (Sx2/nx + Sy2/ny)^(2) / ( (Sx2/nx)^2 / (nx-1) + (Sy2/ny)^2 / (ny-1) )


R t-dist functions:

    dt
    pt - t density (for calculating p values)
    qt - t quantiles: qt(0.975, 9-1) // n-1 dof
    rt - randoms
    t.test
    
    t.test(len ~ supp, data=ToothGrowth, paired=T)
    t.test(ToothGrowth[ToothGrowth$supp=="OJ",]$len,ToothGrowth[ToothGrowth$supp=="VC",]$len, paired=T)
    t.test(ToothGrowth[ToothGrowth$supp=="OJ",]$len - ToothGrowth[ToothGrowth$supp=="VC",]$len)


* One sample t-test (paired=T)
    * computes diff of each data point
    * instead of comparing the variation of the two sets of data as a whole
    * dof = n-1, where n is the number of PAIRS





