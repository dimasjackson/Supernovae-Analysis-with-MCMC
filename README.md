# Estimation of the Fraction of Matter and Energy of the Universe
### Data Analysis · Non-naive Bayes Regression · Bayesian Inference · Markov Chain Monte Carlo Method 

**Problem:** What is the fractional amount of matter and energy of the universe?

**Solution:** I collected supernova data from an international collaboration survey, built from scratch a non-naive bayes regression algorithm to fit the luminosity distance curve and used Bayesian inference to find the probabilities of the parameters values, given the data collected. Then, I used the Markov Chain Monte Carlo method to estimate the final values and respective uncertainties.

**Result:** The fractions of matter and radiation that I estimated are in agreement with the Planck Satellite 2018 release.


# Abstract

In this project we will use the Pantheon+SH0ES luminosity distances data of 1701 Supernovae to estimate cosmological parameters. The parameters that we are interested in are the Hubble constant $H_0$, the matter density fraction $\Omega_m$ and the dark energy fraction $\Omega_\Lambda$, which is responsible for the accelerated expansion of the universe. The bayesian statistics will be adopted to calculate the posterior probability distribution of the parameters using the likelihood distribution and the prior probability. The likelihood for a set of cosmological parameters will be computed given the experimental supernovae distances. Also, we will implement the emcee algorithm to generate samples with a Markov Chain Monte Carlo (MCMC) model starting from the parameters values that maximize the likelihood. Finally, the results will be plotted in the space of parameters and the best values estimated avereging over some percentile of the samples.

# Data Extraction

The data of 1701 supernovae luminosity distances was extracted from the [Phanteon+SH0ES Git Hub Data Release](https://github.com/PantheonPlusSH0ES/DataRelease/tree/main/Pantheon%2B_Data/4_DISTANCES_AND_COVAR), download the data and set your directory properly.

# Theorethical Model
The luminosity distance in a flat expanding Friedmann-Lemaitre-Robertson-Walker university is given by:

$$ d_L = (1+z)d_H \int_{0}^{z}dz^{\prime} \left[ \Omega_r(1+z^{\prime})^4 + \Omega_m(1+z^{\prime})^3+ \Omega_{\Lambda} \right]^{-1/2} ,$$

where $d_H=c/H_0$ is the Hubble distance.

![theo_dist](https://user-images.githubusercontent.com/114688989/214916558-4a1358da-d3eb-45c1-ba68-71f5aca72a92.png)


# Importing the Covariance Matrix

The covariance matrix will be imported from the [Pantheon+SH0ES Git Hub Data Release](https://github.com/PantheonPlusSH0ES/DataRelease/tree/main/Pantheon%2B_Data/4_DISTANCES_AND_COVAR). The format of the covariance (.cov) file is NxN lines where the matrix should be read in sequentially.  The first line gives the number of rows/columns in the matrix (N=1701).  The STATONLY matrix has only elements that correspond to the statistical distance uncertainties for individual SNe. This includes intrinsic scatter off-diagonal components when the light-curves represent the same SN observed by different surveys.

# Likelihood Distribution Calculation

The likelihood distribution that will be used is

$$ \mathcal{L}(\mu_{\rm{exp}}, C | \theta) = 2N\pi \exp{ \left[ \frac{  -(\mu(\theta) - \mu_{\rm{exp}})^T  C  (\mu(\theta) - \mu_{\rm{exp}}) - \sum_{j}c_j  }{2} \right] } ,$$

where $C$ is the covariance matrix, $c_j$ is its eigenvalues, $\theta = (\Omega_r, \Omega_m, H_0)$ and $N$ is the dimension of the data array.

# Curve fit

The Scipy minimize function will be used to compute the parameters $\Omega_\Lambda, \Omega_m, h$ that optimizes the likelihood for the given data. It’s worth noting that the optimize module minimizes functions whereas we would like to maximize the likelihood. This goal is equivalent to minimizing the negative likelihood (or in this case, the negative log likelihood).

![dist_modulus](https://user-images.githubusercontent.com/114688989/214916893-cb913c85-56b9-4521-97b2-6523ea7dfb84.png)

# Plotting the Parameters Estimatitive

The corner plot shows the marginalized distribution for each parameter independently in the histograms along the diagonal and then the marginalized two dimensional distributions in the other panels.

![corner](https://user-images.githubusercontent.com/114688989/214916668-71d4f276-c1aa-4952-99ed-2d1aa902d8d6.png)

# Conclusion
We have analysed the supernova data from Phanteon+SH0ES collaboration to estimate the universe fractions of matter, vaccum energy and the the Hubble parameter using bayesian inference. These parameters was computed considering 10000 Markov Chains generated by 2000 walkers using the emcee implementation of the Monte Carlo algorithm. Since about 40 steps are needed for the chain to "forget" where it started we discarded the initial 100 steps. The final estimation choosen was the 50-th percentile of the flatted samples and the uncertainty the diference of the central value with the 25-th and the 75-th percentiles respectively. The final values encountered are matter density 

$\Omega_m = 0.333^{+0.066}_{−0.068}$

the dark energy density 

$\Omega_\Lambda = 0.711^{+0.071}_{−0.066}$

and the Hubble parameter 

$H_0 = 68.712^{+6.552}_{−6.497}{\rm km/s/Mpc}$

which agree with the direct observation of these parameters released by th Planck Satelite collaboration (2018): $H_0 = (67.4\pm0.5){\rm km/s/Mpc};  \Omega_m=0.315\pm0.007$ and $\Omega_\Lambda = 1 - \Omega_m$.

# References
* [Phanteon+SH0ES results 2021/2022](https://pantheonplussh0es.github.io/)
* [Library emcee documentation](https://emcee.readthedocs.io/en/stable/tutorials/line/)
* [Planck Satelite collaboration (2018)](https://arxiv.org/pdf/1807.06209.pdf)
