---
title: Features and fitting
keywords: RANSAC, Harris
order: 5 # Lecture number for 2020
---

[//]: # (TODO!!!  ** overall intro **)


Table of content

- [RANSAC](#ransac)
  - [Lines](#lines)
  - [Model Fitting](#model-fitting)
  - [Voting Based Fitting](#voting-based-fitting)
  - [Random Sample Consensus](#random-sample-consensus)
- [Local Invariant Features](#local-invariant-features)
- [Harris Corner Detector](#harris-corner-detector)

[//]: # (This is how you can make a comment that won't appear in the web page! It might be visible on some machines/browsers so use this only for development.)

[//]: # (Notice in the table of contents that [First Big Topic] matches #first-big-topic, except for all lowercase and spaces are replaced with dashes. This is important so that the table of contents links properly to the sections)

[//]: # (Leave this line here, but you can replace the name field with anything! It's used in the HTML structure of the page but isn't visible to users)


<a name='ransac'></a>
#RANSAC

In this lecture, we will introduce model fitting for line detection using the RANSAC (Random Sample Consensus) algorithm.

<a name='lines'></a>
## Lines

Straight lines characterize many objects around us. Here are few examples where line detection is useful:

<div class="fig figcenter">
  <img src="{{ site.baseurl }}/assets/pixels/lines-intro.png">
</div>

Looking at these examples and intuitively thinking of line detection, few questions arise.
- Once we have points that belong to a line, what is the line?
- How many lines exist?
- Which points belong to which line?

_Note:_ Edge detection is a technique that is used to identify boundaries. It does not provide information on the orientation of pixels. For example, whether these pixels form a line or corner.

<a name='model-fitting'></a>
## Model Fitting

We would like to form a higher-level succinct representation of the features in the image by grouping multiple features based on a simple model. This section focuses on lines described as one such model (i.e. line fitting) with edge points as features.

**Challenges**

As with any fitting problem, we need to take into account missing information, noise, and outliers. Our solution should be able to detect the best fitting line from edge points that could have clutter and noise. It should also be able to detect lines that bridge missing evidence.

<div class="fig figcenter">
  <img src="{{ site.baseurl }}/assets/pixels/line-fitting-challenges.png">
</div>

Least squares regression is a common technique to find a line of best fit for a dataset. However, least squares is prone to the negative effect of outliers. When dealing with images it is often the case that the number of outliers is large. A better approach is to find a set of inliers to initiate model fitting. In the next section, we'll explore such a technique that helps us find consistent matches.

<div class="fig figcenter">
  <img src="{{ site.baseurl }}/assets/pixels/line-fitting-outliers.png">
</div>

<a name='voting-based-fitting'></a>
## Voting-based Fitting

We could try to fit the model by exhaustively checking all combination of features. However, this approach is inefficient and bears a O(N<sup>2</sup>) time complexity.

Voting is a decent alternative. The idea is to have features vote for compatible models and to capture model parameters that form a majority. It turns out that this set of winning model parameters are not affected by votes from clutter and noisy features as their voting will be inconsistent with the consensus. The problem of missing observations can also be overlooked as the fitted model can span multiple fragments. In general, voting is considered to be robust to outliers and missing data.

[Hough Transform](../edge_detection) uses this voting strategy for detecting lines. Check out this video for a quick review of Hough transform:

<iframe style="display: block; margin: auto;" width="560" height="315" src="https://www.youtube.com/embed/4zHbI-fFIlI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<!-- <<image 4.6 slide 11>> -->

<a name='random-sample-consensus'></a>
## Random Sample Consensus (RANSAC)

Both Hough transform and RANSAC rely on voting to arrive at the optimum model. The part where they differ is in how the model is chosen. As the name suggests, RANSAC introduces randomness in the model selection process. A number of models are proposed until one is found that is supported by a consensus of features (voters). Let's try to understand the algorithm.

**Algorithm**

RANSAC algorithm was developed by Fischler and Bolles in 1981. Here's the gist:

1. Randomly sample a group of points required to fit the model
2. Find model parameters from this sample
3. Calculate the votes from the fraction of inliers within a predetermined threshold of the model
4. Repeat steps 1-3 until model is found with high confidence

**Algorithm Pseudocode**

<div class="fig figcenter">
  <img src="{{ site.baseurl }}/assets/pixels/ransac-pseudocode.png">
</div>

**Model Parameters**

The algorithm can be tuned with the following parameters:

- $n$ – minimum number of points required to estimate model parameters
- $k$ – maximum number of iterations
- $t$ – distance threshold to determine points that fit well to model
- $d$ – number of close points required to assert a model fits well

The parameter that needs most attention is _k_ – number of iterations. Next, we'll see how to determine its value.

**Estimate number of iterations**

Random sampling requires a minimum number of samples to be drawn to provide a high confidence estimate of parameters. The number of samples largely depends on outliers and sample size. In other words, the number of iterations is directly proportional to noise in the data and complexity of the model. This can be described by the following probabilities:

$$ \begin{equation} P_f = (1 - W^n)^k = 1 - p \end{equation} $$

where $k$ is the number of samples,  $n$ is the minimum number of points required to estimate model parameters (e.g. line has 2 parameters), $W$ is the fraction of inliers, and $P_f$ represents probability of _k_ samples failing.

Solving for $k$ gives us the minimum number of samples required to keep the failure rate low.

$$ \begin{equation} k = \frac{\log(1 - p)}{\log(1 - W^n)} \end{equation} $$

The table below shows the number of samples required for different choices of noise and model size. It's evident that $k$ increases sharply as $n$ increases.

<div class="fig figcenter">
  <img src="{{ site.baseurl }}/assets/pixels/RANSAC-number-of-iterations.png">
</div>

**Refined RANSAC**

**Analysis**

**Summary**

<a name='local-invariant-features'></a>
# Local Invariant Features

<a name='harris-corner-detector'></a>
# Harris Corner Detector
