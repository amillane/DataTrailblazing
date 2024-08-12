---
layout: post
title: "Caching In With Bayesian Statistics"
date: 2024-08-09
author: Drew Millane and Sam Spackman
introduction: Geocaching is the largest treasure hunt in the world. With the number of active geocachers and caches in the world growing every year, it is important to know what kind of geocache people like to find.
read_time: true
image: 
  thumbnail: https://github.com/amillane/DataTrailblazing/blob/master/assets/images/posts/geocache/geocachethumbnail.jpgraw=true
  caption: "Photo from [Unsplash](https://www.unsplash.com)"
---

# Geocache: What is it? 
Geocaching is the world’s largest treasure hunt. The first geocache was hidden on May 3rd, 2000 and
since then over 3 million more geocaches have been hidden across 191 different countries and all 7
continents. There are over 300,000 cache owners in the world. In 2020 a geocache tagged along for a
ride to Mars with NASA’s Perseverance rover and landed in the Jezero Crater in early 2021, marking
the first interplanetary geocache. With such a buzz about geocaching, it is only natural to wonder what
makes a good geocache.

No two geocaches are the same. Some are placed in difficult-to-reach places (see the geocache on
Mars above), others are easy to get to but craftily hidden. A cache’s “Terrain” rating indicates the
physical effort that will be needed to get to the cache while a cache’s “Difficulty” rating indicates the
amount of effort that will go into solving or finding the cache. Some geocaches are as large as a boulder,
others are as small as a thimble. When a geocacher finds a geocache they have the option to grant that
cache a “favorite”, similar to a “like” on social media. The goal of our analysis is to determine whether
a cache’s Terrain or Difficulty rating affects the number of favorites it receives.

![An example of a geocache on BYU campus. This cache has a Terrain and Difficulty rating of 2, is size ”Other”, and has received 48 favorites to date](https://github.com/amillane/DataTrailblazing/blob/master/assets/images/posts/geocache/cache_page_example.png?raw=true "Spherical Miracle")

# The Data
We scraped the data of 140,000 geocaches hidden in Utah and California, getting the ID, Difficulty,
Terrain, Date Hidden, and number of Favorites.

# Our Model

The favorite measure is quantified as a count, prompting our selection of a Poisson likelihood. There are
nine groups each for terrain and difficulty. In the model depicted below, every group received their own
rate parameter, which follows to a Gamma distribution. Additionally, we imposed priors on the alpha
and beta parameters for each group, resulting in a total of 27 parameters to be estimated.
For the alpha and beta priors, we opted for parameters that would yield an expected count of
approximately five favorites for each group. This decision was made due to our limited insight into the
mean count for each group, assuming a uniformity across them.

$$ y_{i} \mid \lambda_{i} \sim \text{Pois}(\lambda_{i}) $$

$$ \lambda_{i} \mid \alpha_{i}, \beta_{i} \sim \text{Gamma}(\alpha_{i}, \beta_{i}) $$

$$ \alpha_{i} \mid c, d \sim \text{Gamma}(c, d) $$

$$ \beta_{i} \mid e, f \sim \text{Gamma}(e, f) $$

$$ y_i \text{ is the number of favorites a given cache has received} $$

$$ \text{Prior: } c = 5, \, d = 1, \, e = 1, \, f = 1 $$


Our objective was to sample the posterior distribution for each group manually and through a Probabilistic Programming Language (PPL). This approach allows us to infer whether there are differences between the groups based on favorites.

# Sampling
## By Hand 
For the by-hand approach, we first needed to start with our joint posterior density, as shown below:

$$
p(\lambda_{i} \mid \alpha_{i}, \beta_{i}, c, d, e, f, y_{1}, \ldots, y_{m}) \propto p(y \mid \lambda_{i}) p(\alpha_{i} \mid c, d) p(\beta_{i} \mid e, f)
$$

$$
\quad \prod_{i=1}^{n}\prod_{j=1}^{m} \frac{\lambda^{y_{ij}} e^{-\lambda_{i}}}{y_{ij}!} \frac{\beta_{i}^{\alpha_{i}}}{\Gamma(\alpha_{i})} \lambda_{i}^{\alpha_{i}-1} e^{-\beta_{i} \lambda_{i}} \frac{d^{c}}{\Gamma(c)} \alpha_{i}^{c-1} e^{-d \alpha_{i}} \frac{f^{e}}{\Gamma(e)} \beta_{i}^{e-1} e^{-f \beta_{i}}
$$

The approach we wanted to take was to derive the full conditionals of \(\lambda\), \(\alpha\), and \(\beta\) for each group. To derive the full conditionals, we took the log of the joint. Below are the full conditionals for the parameters:

$$
(\lambda_{i} \mid \cdot) = \sum_{j=1}^{m} [y_{ij} \log(\lambda_{i})] - m \lambda_{i} + (\alpha_{i} - 1) \log(\lambda_{i}) - \beta_{i} \lambda_{i}
$$

$$
(\alpha_{i} \mid \cdot) = \alpha_{i} \log(\beta_{i}) - \log(\Gamma(\alpha_{i})) + \alpha_{i} \log(\lambda) + (c - 1) \log(\alpha_{i}) - d \alpha_{i}
$$

$$
(\beta_{i} \mid \cdot) \sim \text{Gamma}(\alpha_{i} + e, \lambda_{i} + f)
$$

1. **Choose initial values** for all `λ`, `α`, and `β`.

2. **For 10,000 Draws:**

    1. **For each group:**
    
        - **Sample** `β` from a Gamma distribution.
        - **Slice sample** the full conditional of `α`.
        - **Slice sample** the full conditional of `λ`.

# PPL

For our Probabilistic Programming Language (PPL), we opted for STAN. STAN facilitated the setup of the hierarchical model more efficiently than deriving the full conditionals manually. However, in terms of computational time, the PPL lagged significantly behind the slice sampler. While the manual sampler took approximately 55 seconds, the PPL required around 442 seconds to complete the computation. This substantial time gap underscores the value of the manual sampler.


# Model Diagnostics
To validate our sampling from the posterior distribution, we assessed both the mixing quality and effective sample size of our sampler.

![Trace Plots](https://github.com/amillane/DataTrailblazing/blob/master/assets/images/posts/geocache/Trace.jpeg?raw=true "Trace Plots")

Based on our trace plot analysis, it is evident that our sampler exhibits excellent mixing behavior and effectively generates samples that appear to be independent. Additionally, the effective sample size for each parameter $\lambda$ closely matches the total number of samples collected, minus those discarded during the burn-in period. In conclusion, our sampler performs admirably in capturing samples from the posterior distribution.


| **Lambda** | **Value** |
|------------|-----------|
| var1       | 8697.055  |
| var2       | 8931.027  |
| var3       | 9578.830  |
| var4       | 9000.000  |
| var5       | 9389.972  |
| var6       | 9792.548  |
| var7       | 8683.698  |
| var8       | 9000.000  |
| var9       | 9000.000  |

## Comparing Samplers

We compared our manual and PPL samplers to check if they agreed within Monte Carlo error. By taking samples from both and calculating the differences, we created confidence intervals. The intervals, as shown below, encompass zero, suggesting that the samples are statistically equivalent within Monte Carlo error. This supports us to go forward with our analysis.

### *Difference in PPL/By-hand estimation of mean favorites per group for Difficulty*

| **Difficulty Level** | **Lower Bound** | **Upper Bound** |
|----------------------|------------------|------------------|
| 1                    | -0.0012          | 0.0005           |
| 1.5                  | -0.0001          | 0.0003           |
| 2                    | -0.0001          | 0.0003           |
| 2.5                  | -0.0007          | 0.0003           |
| 3                    | -0.0007          | 0.0006           |
| 3.5                  | -0.0009          | 0.0014           |
| 4                    | -0.0012          | 0.0017           |
| 4.5                  | -0.0001          | 0.0049           |
| 5                    | -0.0036          | 0.0019           |

### *Difference in PPL/By-hand estimation of mean favorites per group for Terrain*

| **Terrain Level** | **Lower Bound** | **Upper Bound** |
|-------------------|------------------|------------------|
| 1                 | -0.0004          | 0.0020           |
| 1.5               | -0.0001          | 0.0004           |
| 2                 | -0.0003          | 0.0002           |
| 2.5               | -0.0004          | 0.0003           |
| 3                 | -0.0003          | 0.0005           |
| 3.5               | -0.0005          | 0.0004           |
| 4                 | -0.0004          | 0.0010           |
| 4.5               | -0.0006          | 0.0023           |
| 5                 | -0.0024          | 0.0010           |

## Sensitivity of Priors

We wanted to see how our model reacts to different priors. We calculated confidence intervals for our original prior and for new ones, where one prior had a shape parameter of 1000 while the others had a shape and scale parameter of 1. The table below shows that there's not much difference between the two priors, indicating that our model isn't sensitive to priors because of the large amount of data we have.

### *The difference of the priors: The one we chose and the priors c = 1000, d = 1, e = 1, f = 1*

| **Lower Bound** | **Upper Bound** |
|-----------------|------------------|
| 0.0004          | 0.0028           |
| 0.0003          | 0.0008           |
| 0.0011          | 0.0016           |
| 0.0023          | 0.0030           |
| 0.0028          | 0.0036           |
| 0.0056          | 0.0065           |
| 0.0081          | 0.0095           |
| 0.0166          | 0.0195           |
| 0.0148          | 0.0182           |

## Results

Plotted below are the densities of the mean favorites of each group for Difficulty and Terrain. On both graphs the extreme groups (highest and lowest ratings) have the highest mean number of favorites. Also of note is the fact that each group has very distinct distributions, almost completely without overlap. This implies that the Difficulty and Terrain rating of a geocache does indeed affect the number of favorites that the cache receives.



![Densities of Difficulty rating](https://github.com/amillane/DataTrailblazing/blob/master/assets/images/posts/geocache/Diffculty.jpeg?raw=true)

![Densities of mean number of favorites by Terrain rating](https://github.com/amillane/DataTrailblazing/blob/master/assets/images/posts/geocache/Terrain.jpeg?raw=true)

## Frequentist Analysis

We conducted a frequentist analysis on our data, employing the Poisson likelihood to determine the Maximum Likelihood Estimators (MLE) for each group. Our findings for both difficulty and terrain revealed estimates closely resembling those from our Bayesian analysis. This consistency can be attributed to the amount of data at our disposal. However, the Bayesian approach offers the advantage of interpreting parameters with statements of probability, a capability not afforded by the frequentist approach.


| **Difficulty Level** | **Bayesian Lower** | **Bayesian Upper** | **Frequentist** |
|----------------------|---------------------|---------------------|------------------|
| 1                    | 8.3105              | 8.4326              | 8.3709           |
| 1.5                  | 2.4336              | 2.4603              | 2.4472           |
| 2                    | 2.9928              | 3.0254              | 3.0091           |
| 2.5                  | 4.2801              | 4.3467              | 4.3130           |
| 3                    | 5.1512              | 5.2386              | 5.1940           |
| 3.5                  | 6.0822              | 6.2378              | 6.1602           |
| 4                    | 7.0369              | 7.2340              | 7.1352           |
| 4.5                  | 8.0938              | 8.4289              | 8.2625           |
| 5                    | 10.2651             | 10.6396             | 10.4520          |


| **Terrain Level** | **Bayesian Lower** | **Bayesian Upper** | **Frequentist** |
|-------------------|---------------------|---------------------|------------------|
| 1                 | 12.2178             | 12.3779             | 12.2977          |
| 1.5               | 4.0593              | 4.0948              | 4.0772           |
| 2                 | 2.8456              | 2.8818              | 2.8634           |
| 2.5               | 2.5303              | 2.5774              | 2.5538           |
| 3                 | 2.3400              | 2.3891              | 2.3646           |
| 3.5               | 2.0860              | 2.1450              | 2.1158           |
| 4                 | 2.7744              | 2.8675              | 2.8203           |
| 4.5               | 4.2199              | 4.4108              | 4.3138           |
| 5                 | 5.1148              | 5.3407              | 5.2294           |

## Conclusion

After reviewing our analysis we conclude that the number of favorites that a cache receives does indeed depend on its Difficulty and Terrain rating. Many geocaches like to have the option of quick and easy to find geocaches so they can keep up a daily caching streak. Perhaps easy to find geocaches receive many favorites for this reason. On the other side of the spectrum, perhaps geocachers keep difficult caches on their radar and once they have successfully found these caches they feel rewarded after the amount of effort they put into locating it that they want to award these caches a favorite. Speculation aside, it is clear that caches with the lowest and highest Terrain and Difficulty ratings receive the most favorites.
