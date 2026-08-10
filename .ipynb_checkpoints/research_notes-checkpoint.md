# Research Notes
This is meant to serve as a comprehensive record of the decisions made in reproducing H0 using the scientific methodologies, statistical pipelines, and code tools presented by the team behind the Cosmoverse 2026 Data Challenge.

## CATS vs. EDD
### Literature Takeaways
D. Scolnic et al 2023 ApJL 954 L31 - CATS paper, emphasizes unsupervised approach to avoid bias with automated exclusion of higher blue star densities above a certain percentage of peak density, finds H0 closer to late-universe predictions - doi: [10.3847/2041-8213/ace978](https://doi.org/10.3847/2041-8213/ace978)

Taylor J. Hoyt et al 2026 ApJ 1002 11 - CATS has much poorer precision than EDD in predicting TRGB, ~3x scatter of measurements, CATS actually biased to tip of AGB for NGC 4536 and NGC 4038/9 - doi: [10.3847/1538-4357/ae29eb](https://doi.org/10.3847/1538-4357/ae29eb)

Gagandeep S. Anand et al 2021 AJ 162 80 - Many improvements in resolving individual stars of galaxies over time, focusing on multiple methods to maximize resolution and resolve stars of a galaxies halo, emphasizing the collection of as much information as possible - doi: [10.3847/1538-3881/ac0440](https://doi.org/10.3847/1538-3881/ac0440)

### Decision
- EDD allows for greater control of which stars to eliminate in a galaxy, particularly important for the central/denser areas
- CATS has 3x poorer precision than EDD in TRGB prediction
- CATS eliminates halo determination bias, but EDD provides the opportunity to customize R25 for each galaxy
- **Using EDD for reliability, but remaining cautious for bias**

## EDD Undersampling vs. Contamination
This is a major problem in this study, whether with spatial clipping, star luminosity constraints, or luminosity function smoothing levels. I will do my best to sample with minimum contamination while also maintaining a sufficient sample size.

## Sobel Edge Detection
- Simple: regular old finite difference
- Poisson: same as simple, but with the impact of variance being decreased (helps eliminate noise)
- Hatt: signal/noise of derivative, "naturally down-weighting low-count regions", introducing a bias
- **Running Poisson and Hatt for each galaxy, taking Poisson measurements if Poisson ~ Hatt for limited bias and reduced noise**

## Galaxy Measurements
### NGC 1316
- Determined bluer/lower color edge of rgb to be at 1.5 (second peak for the bootstrap sample if placed at ~1.4 which seemed to be the bluer edge of the RGB, thus included until 1.5 ), and redder/higher color edge to be ~0.8
- Kept scale_factor at 1.0, as all recorded stars were sufficiently removed from the galactic bulge (additionally, changing scale_factor within recommended ranges had no impact on which stars were included)
- For the bootstrap sampling on this galaxy, I used a tau of 0.1 to eliminate any noise, but nothing higher as the peak came out pretty clean, and I did not want to bias brightwards
- The following table is the variations in the TRGB created by adjusting tau, color range, and spatial clipping for NGC 1316. Parameters mentioned in the first column are the only ones being changed. Since my color_hi was already set at 1.5, I will not include a color_hi variation of +0.2

| Parameter | Value | TRGB Magnitude |
|-----------|-------|----------------|
| none | N/A | 27.406 |
| tau | 0.05 | 27.406 |
| tau | 0.15 | 27.396
| tau | 0.2 |27.406 |
| color_hi | 1.3 | **27.836**
| color_lo | 1.01 | 27.396
| color_lo | 0.61 | 27.416
| scale_factor | 0.8 | 27.406
| scale_factor | 1.2 | 27.406

- While everything else maintains well within the 0.03 magnitude variations of the TRGB, adjusting the color_hi value to 1.3 significantly changes the TRGB magnitude (by .43, or ~14x the healthy variance of 0.03), clearly showing that the stars between 1.3 and 1.5 are significant to finding the TRGB. This is verified by the fact that a color_hi value of 1.3 does not produce a gaussian bootstrap sample, thus meaning that the TRGB is not full described in this case.
- With a consistent use of EDD, low extinction and minimally inclined galaxy, photometric zero-point errors, extinction, and internal extinction are respectively very minimal. However, since NGC 1316 is a merger of several other smaller galaxies, there is a risk for blending/crowding even in the halo region, which biases the TRGB brighter and distances to galaxies smaller, introducing uncertainty.
![CMD Plots for NG1316](images/NGC1316.png)
*CMD plots of NG4258 and NGC1316 with selection band 0.81-1.5*

