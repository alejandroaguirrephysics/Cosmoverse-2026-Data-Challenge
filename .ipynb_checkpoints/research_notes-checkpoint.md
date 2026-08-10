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
- Using EDD for reliability, but remaining cautious for bias

## EDD Undersampling vs. Contamination
This is a major problem in this study, whether with spatial clipping, star luminosity constraints, or luminosity function smoothing levels.


