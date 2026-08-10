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
![CMD plots of NG4258 and NGC1316 with selection band 0.81-1.5](images/NGC1316)


