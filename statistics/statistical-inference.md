### 1. Describe real-world examples of questions that can be answered with the statistical inference methods presented in this course. (e.g., estimation, hypothesis testing).
- Inferential questions
    - One in which you analyze the data to see if there are patterns, trends, or relationships between variables in a representative sample.
    - We want to quantify how much the patterns, trends, or relationships between variables is applicable to all individuals units in the population.

- Examples
    - Is eating at least 5 servings a day of fresh fruit and vegetables is associated with fewer viral illnesses per year?
    - Is the gestational length of first born babies the same as that of non-first borns?

- Inferential vs. Exploratory
    - Inferential question is a question about the true population.
    - Exploratory question is a question about the data set.
    (ex: Do diets rich in certain foods have differing frequencies of viral illnesses _in a set of data_ collected from a group of individuals?)

### 2. Name common population parameters (mean, proportion, median, variance, standard deviation, and correlation) that are often estimated using sample data, and write computer scripts to calculate estimates of these parameters.

### 3. Define the following terms in relation to statistical inference: population, sample, population parameters, estimate, sampling distribution.

| Terms                                                 | Definitions                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| population                                            | the entire set of entities/objects of interest                                                                                   |
| population parameter                                  | a numerical summary value about the population                                                                                   |
| sample                                                | a collection of observations from a population                                                                                   |
| point estimate                                        | a single number calculated from a random sample that estimates an unknown population parameter of interest                       |
| sampling distribution                                 | a distribution of point estimates, where each point estimate was calculated from a different random sample from the same population |
| observation | a quantity or quality (or a set of these) from a single member of a population                                                   |


### 4. Write a computer script to draw random samples from a finite population (e.g., census data) and reveal a sampling distribution from a finite population.

- Drawing one sample of size 40
```r
samples_1 <- rep_sample_n(virtual_box, size = 40)
samples_1
```

- Drawing 10000 sample of size 40
```r
samples_10000 <- rep_sample_n(virtual_box, size = 40, reps = 10000)
samples_10000
```

### 5. Explain random and representative sampling and how this can influence estimation.
- If and only if our sample is random and representative, we can say that our sample estimates can reflect the true population parameter $p$.

### 6. Define random variables and explain how they relate to sampling.
- Definition
    - Mapping outcomes of a random process (e.g., flipping a coin, rolling a dice, pullling a timbit out of the box…) to numbers.

- How they relate to sampling?


### 7. Define standard error and explain its purpose.
- Definition
    - The standard deviation of the sampling distribution of a sample estimate derived from population.

- Purpose
    - Standard errors quantify the effect of sampling variation induced on our estimates.
    - They quantify how much we can expect different proportions of a shovel’s balls that are red to _vary_ from one sample to another sample to another sample, and so on.
    - As sample size increases, the standard error decreses.

### 8. Compare and contrast population distribution, sample distribution and an estimator’s sampling distribution.
- Shape: Population distribution, sample distribution are similar, but sampling distribution is (usually) bell-shaped
- Point estimate: Population distribution, sampling distribution are close, but sample distribution is a little different (sample variation)

### 9. Explain what a sampling distribution is, list its properties, and its purpose in statistical inference.
- Definition
    - A distribution of point estimates, where each point estimate was calculated from a different random sample from the same population

- Purpose
    - Estimates true population estimates
    - Using these sampling distributions, for a given sample size n, we can make statements about what values we can typically expect.

- Sample size and sampling distribution
    - As we increase the sample size, the sampling distribution gets narrower = standard error gets smaller
    - As we increase the sample size, each sample is more likely to have an estimate that is closer to the true population parameter we're trying to estimate. 

### 10. Explain why we don’t know/have a sampling distribution in practice/real life.
- In real life, we only have one sample and don't have access to the true population. 
- The estimate from a single sample is not good enough and won't be the exact population parameter.
- So, we do simulations to try to see patterns of what the sampling distribution of the sample estimate would look like for a given sample. 
    - We can use these patterns to approximate the sampling distribution of the sample estimate in real life case when we only have one sample.

### 11. Define bootstrapping.
- Definition
    - Bootstrapping refers to constructing an approximation to the sampling distribution using only one sample.

- Method
    - Take a bootstrap sample : a random sample taken with replacement from the original sample of the same size as the original sample
    - Calculate the bootstrap point estimate (mean, median, proportion, slope ...) from that bootstrap sample
    - Repeat 1, 2 many times to create a bootstrap distribution (a distribution of bootstrap point estimates)
    - Calculate the plausible range of values around our observed point estimate.

### 12. Write a computer script to create a bootstrap distribution to approximate a sampling distribution.

- Bootstrapping with `infer` package
```r
bootstrap_sample_dist <- one_sample %>%
  ungroup() %>%
  specify(response = diameter) %>%
  generate(reps = 10000) %>%
  calculate(stat = "mean")
  # visualize()
```

- Bootstrapping with `rep_sample_n()`
```r
boot_dist <- one_sample %>% 
    rep_sample_n(size = k, replace = TRUE, reps = 10000) %>% 
    summarise(stat = sum(type == "candy corn" / nrow(one_sample)))
```

### 13. Contrast a bootstrap sampling distribution with a sampling distribution obtained using multiple samples.
- Bootstrap sampling distribution
    - Point estimate: diffferent from the true population point estimate / sampling distribution (similar to one sample distribution point estimate)
    - Shape (spread): similar to the sampling distribution
    -> Generate a plausible range where we believe the value of true population parameter lies to report along with our estimate

- Sampling distribution
    - Point estimate: similar to true population point estimate
    - Shape (spread): different from the true population distribution, similar to the bootstrap sampling distribution


### 14. Define what a confidence interval is, and why we want to generate one.
- Definition
    - The confidence interval (CI) is a range of values that's likely to include a population value with a certain degree of confidence.

- Example
    - 95% confidence interval : If we were to repeat the process of sampling and calulating a 95% confidence interval many times, 95% of the time we would expect our population parameter's value to lie within the confidence interval.

- Higher confidence levels correspond to wider confidence intervals (we want to be more sure), and lower confidence levels correspond to narrower confidence intervals.

### 15. Explain how the bootstrap sampling distribution can be used to create confidence intervals.
- Since we can’t continue sampling from the population, we instead bootstrap from the one sample we have to estimate the sampling variability.
- Using the bootstrap distribution, we calculate the "plausible range" of the true point estimate will lie to.

### 16. Write a computer script to calculate confidence intervals for a population parameter using bootstrapping.

- One method to construct a confidence interval is to use the middle 95% of values of the bootstrap distribution.
- Recap: bootstrap distribution is an estimate of sampling distribution.

- Get bootstrap distribution
```r
bootstrap_distribution <- one_sample %>% 
    rep_sample_n(size = 40, reps = 1000, replace = TRUE) %>%
    summarise(stat = mean(age))
bootstrap_distribution
```

- Calculate confidence interval
```r
library(infer)
ci_95 <- bootstrap_distribution %>% 
    get_confidence_interval(level = 0.95, type = "percentile")
ci_95

>>>
2.5%        97.5%
<dbl>       <dbl>
75.54875    86.15125
```

### 17. Effectively visualize point estimates and confidence intervals.
- Visualization: geom_errorbar()
```r
    geom_errorbar(data = ci_large_95, 
                  mapping = aes(x = "Canadian Seniors", 
                                y = mean(one_large_sample$age),
                                ymin = `2.5%`,      ## lower ci
                                ymax = `97.5%`),    ## upper ci
                  size = 1, color = "blue", width=.1)
```

### 18. Interpret and explain results from confidence intervals.
- We are 95% “confident” that the true <point estimate> of <population> is somewhere between <lower ci> and <upper ci>.


### 19. Give an example of a question you could answer with a hypothesis test.
- Will changing the design of the website lead to a change in customer engagement (measured by click-through-rate, for example)?
- Is the gestation length of first babies different from that of non-first babies?

- We use data from a sample to help us decide between two competing hypotheses about a **population**

### 20. Given an inferential question, formulate hypotheses to be used in a hypothesis test.
1. Null Hypothesis $H_0$
- A claim that there really is "no effect" or "no difference."
- In many cases, the null hypothesis represents the status quo or that nothing interesting is happening.

2. Alternative hypothesis $H_A$
- The claim for which we seek significant evidence.
- The alternative is usually what the experimenter or researcher wants to establish or find evidence for.

3. Example
- Will changing the design of the website lead to a change in customer engagement (measured by click-through-rate, for example)?
    - Null hypothesis, $H_0$: **The population click-through rate** for the two versions of the websites are equal.
    - Alternative hypothesis, $H_A$: **The population click-through rate** for the two versions of the websites are not equal.

- Is the gestation length of first babies different from that of non-first babies?
    - $H_0$: **Population mean pregnancy length** for first born babies and non-first babies are equal.
    - $H_A$: **Population mean pregnancy length** for first born babies and non-first babies are not equal. ## include population, point estimate

### 21. Identify the correct steps and components of a basic hypothesis test.
1. Assume that $H_0$ is true
2. Reject $H_0$ if the sample evidence strongly suggests that it is false, and accept $H_A$
3. Fail fo reject $H_0$ if the sample does not provide such evidence.

- Hypothesis test with permutation
1. Define your null and alternative hypotheses
2. Compute a test statistic (𝛿*) that corresponds to the null hypothesis
3. Use a model of the null hypothesis to generate a random dataset similar to the original dataset (𝛿) and calculate a test statistic from that randomly generated dataset (do this many times to generate a distribution) (sample statistic distribution)
4. See where your test statistic (𝛿*) from your sample(s) falls on this distribution
5. If it is near the extremes (past some threshold) you reject the null hypothesis, otherwise you fail to reject the null hypothesis.

### 22. Write computer scripts to perform hypothesis testing via randomization (e.g., permutation) and interpret the output.
1. Set up our hypotheses
- 𝐻0: The click through rate for the Interact version of the website is the same as the click through rate for the Services version of the website (𝑝𝐼𝑛𝑡𝑒𝑟𝑎𝑐𝑡=𝑝𝑆𝑒𝑟𝑣𝑖𝑐𝑒𝑠)
- 𝐻𝐴: The click through rate for the Interact version of the website is different from the click through rate for the Services version of the website (𝑝𝐼𝑛𝑡𝑒𝑟𝑎𝑐𝑡≠𝑝𝑆𝑒𝑟𝑣𝑖𝑐𝑒𝑠)

2. Compute a test statistic (𝛿*)
- 𝛿 : $H_0$ - $p_{internet} - p_{services} = 0$
- Calculate 𝛿* from our sample
```r
delta_sample <- diff(click_through_est$click_rate)
delta_sample

webpage	    click_rate	lower_ci	upper_ci
<chr>	    <dbl>	    <dbl>	    <dbl>
Interact	0.02847709	0.02228642	0.03509080
Services	0.04849885	0.03693226	0.06081601

>>> 0.0200217507546521
```

3. Generate the simulated data under the model $H_0$
```r
null_distribution_webpage <- click_through %>% 
    specify(formula = click_target ~ webpage, success = "1")  %>%   ## Numeric variable first
    hypothesize(null = "independence") %>% 
    generate(reps = 1000, type = "permute")  %>%            ## Method: permutation
    calculate(stat = "diff in props", order = c("Services", "Interact"))        ## Point estimate: "diff in props"
null_distribution_webpage

replicate	stat
<int>	    <dbl>
1	        0.004648803
2	        -0.005994007
3	        -0.004811473
⋮	              ⋮
998	        0.002283734
999	        -0.010724145
1000	    0.007013872
```

4. Visualize the distribution of $H_0$
```r
h0_dist <- null_distribution_webpage %>% 
    visualize() +
    theme(text = element_text(size=20))
h0_dist
```
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_552_stat-inf-1_students/_images/04_lecture-hyp-test-intro_63_0.png)

5. Find threshold for rejecting $H_0$
```r
threshold <- quantile(null_distribution_webpage$stat, c(0.025, 0.975))      ## Use quantile function, find 95% confidence interval

h0_dist <- h0_dist + geom_vline(xintercept = c(threshold[[1]], threshold[[2]]), 
                     color = "blue",
                    lty = 2)
```

6. Compare whether the test statistic (𝛿*) pass the threshold or not
- If our test statistic is very large (or very small) so that it lies beyond the threshhold for significance:
    - We reject $H_0$ and accept $H_A$
- If not, 
    - We fail to reject $H_0$

### 23. Identify the advantages of randomization tests when estimating parameters different from proportions and means.
- We can simulate the null hypothesis being true

### 24. Describe the relationship between confidence intervals and hypothesis testing.

### p-value
- Definition:
    - The probability of obtaining a test statistic just as extreme or more extreme than the observed test statistic assuming the null hypothesis, 𝐻0, is true

- calculate p-value with infer
```r
null_distribution_webpage %>% 
    get_pvalue(obs_stat = delta_sample, direction = "both")

p_value
<dbl>
0.002
```

- If p-value < $\alpha(0.05 for 95%, 0.01 for 99%)$, we reject $H_0$, accept $H_A$
    - If not, we fail to reject $H_0$

### Calculate ci and mean
```r
summary_stat <- bootstrap_dist %>%
  ungroup() %>%
  group_by(party) %>%
  nest() %>%
  mutate(mean = map_dbl(data, ~mean(.$stat))) %>%
  mutate(ci = map(data, ~get_confidence_interval(., level = 0.95, type = "percentile"))) %>%
  unnest(ci) %>%
  rename(lower = `lower_ci`, upper = `upper_ci`)
```
