# Research Notes
This is meant to serve as a comprehensive record of the decisions made in reproducing H0 using the scientific methodologies, statistical pipelines, and code tools presented by the team behind the Cosmoverse 2026 Data Challenge.

## CATS vs. EDD
### Literature Takeaways
D. Scolnic et al 2023 ApJL 954 L31 - CATS paper, emphasizes unsupervised approach to avoid bias with automated exclusion of higher blue star densities above a certain percentage of peak density, finds H0 closer to late-universe predictions - doi: [10.3847/2041-8213/ace978](https://doi.org/10.3847/2041-8213/ace978)

Taylor J. Hoyt et al 2026 ApJ 1002 11 - CATS has much poorer precision than EDD in predicting TRGB, ~3x scatter of measurements, CATS actually biased to tip of AGB for NGC 4536 and NGC 4038/9 - doi: [10.3847/1538-4357/ae29eb](https://doi.org/10.3847/1538-4357/ae29eb)

Gagandeep S. Anand et al 2021 AJ 162 80 - Many improvements in resolving individual stars of galaxies over time, focusing on multiple methods to maximize resolution and resolve stars of a galaxy's halo, emphasizing the collection of as much information as possible - doi: [10.3847/1538-3881/ac0440](https://doi.org/10.3847/1538-3881/ac0440)

### Decision
- EDD allows for greater control of which stars to eliminate in a galaxy, particularly important for the central/denser areas
- CATS has 3x poorer precision than EDD in TRGB prediction
- CATS eliminates halo determination bias, but EDD provides the opportunity to customize R25 for each galaxy
- **Using EDD for reliability, but remaining cautious for bias**

## EDD Undersampling vs. Contamination
This is a major problem in this study, whether with spatial clipping, star luminosity constraints, or luminosity function smoothing levels. I will do my best to sample with minimal contamination while also maintaining a sufficient sample size.

## Sobel Edge Detection
- Simple: regular old finite difference
- Poisson: same as simple, but with the impact of variance being decreased (helps eliminate noise)
- Hatt: signal/noise of derivative, "naturally down-weighting low-count regions", introducing a bias
- **Running Poisson and Hatt for each galaxy, taking Poisson measurements if Poisson ~ Hatt for limited bias and reduced noise**

## Galaxy Measurements
### NGC 1316
#### Distance Modulus / Uncertainty / Tip-Contrast
| Parameter | Value |
|-----------|-------|
| mu_target | 31.258 ± 0.062 mag |
| d_Mpc | 17.85 ± 0.51 Mpc |
| tip_contrast_anchor | 4.9 |
| tip_contrast_target | 6.2 |
#### Parameter Choices
- For F606W filter, determined high color edge of rgb to be at 1.45 (second peak for the bootstrap_anchor sample if placed at 1.5, second peak for the bootstrap_target sample if placed at ~1.4, so found a middle ground), and low color edge to be 0.81
- With the F555W filter, I used color constraints of 1-2.1 so as to maintain the entire RGB spectrum while still minimizing contamination
- Set scale_factor at 0.8, to provide a sufficient sample for NGC 4258/anchor galaxy while still excluding the galactic bulge
- For the bootstrap sampling on this galaxy, I used a tau of 0.1 to eliminate any noise, but nothing higher as I did not want to bias brightwards
#### Systematic Uncertainty
- The following table is the variations in the TRGB created by adjusting tau, color range, and spatial clipping for NGC 1316. Parameters mentioned in the first column are the only ones being changed. Poisson results were very similar to hatt results, eliminating that source of error. Since my color_hi for F606W was already set at 1.45, instead of testing a color_hi increase of 0.2, I will test a color_hi increase of 0.05, and since my color_hi for F555W is already 2.1, I will only test a decrease of 0.2 and not an increase, to avoid going past the flat TRGB limits placed by Jang & Lee 2017

| Parameter | Value | NGC1316 TRGB Magnitude |
|-----------|-------|------------------------|
| none | N/A | 27.406 |
| tau | 0.05 | 27.406 |
| tau | 0.15 | 27.396 |
| tau | 0.2 | 27.416 |
| color_hi F606W | 1.5 | 27.406 |
| color_hi F606W| 1.25 | **tip_contrast <3** |
| color_lo F606W| 1.01 | 27.386 |
| color_lo F606W| 0.61 | 27.416 |
| color_hi F555W| 1.9 | 27.406 |
| color_lo F555W| 1.2 | 27.406 |
| color_lo F555W| 0.8 | 27.406 |
| scale_factor | 1 | 27.406 |
| scale_factor | 1.2 | 27.406 |

- While everything else maintains well within the 0.03 magnitude variations of the TRGB, adjusting the color_hi value for the F606W filter to 1.25 does not yield a TRGB result due to a low tip_contrast, thus meaning that making the color_hi too low does not allow for proper description of the TRGB
- With a consistent use of EDD, low extinction and minimal inclination, photometric zero-point errors, extinction, and internal extinction are respectively very minimal. However, since NGC 1316 is a merger of several other smaller galaxies, there is a risk for blending/crowding even in the halo region, which biases the TRGB brighter and distances to galaxies smaller, introducing uncertainty.
![CMD Plots for NG1316](images/NGC1316.png)
*CMD plots of NG4258 and NGC1316 with selection band 0.81-1.5 on F606W filter*

### NGC 1404
#### Distance Modulus / Tip-Contrast
| Parameter | Value |
|-----------|-------|
| mu_target | 30.665 ± 0.060 mag |
| d_Mpc | 13.58 ± 0.38 Mpc |
| tip_contrast_anchor | 5.7 |
| tip_contrast_target | 9.81 |
#### Parameter Choices
- For F606W filter, determined high color edge of rgb to be at 1.4 (messier/uncertain peak for the bootstrap sample if placed at 1.5, thus lowered to 1.4), and low color edge to be 0.95 (any higher or lower again created a more uncertain peak for both NGC 4258 and consequently NGC 1404 in a similar manner)
- For F555W filter, I used RGB color constraints of 1.4-2, as wider, shifted, or narrower constraints resulted in non unimodal/gaussian bootstrap samples and large error
- Moved scale_factor at 0.8, as any larger scale_factor resulted in multiple peaks in the bootstrap sample and higher error, a direct consequence of undersampling. Did not go any lower so as to avoid contamination by the galactic bulge
- On this galaxy, I used a tau of 0.1 to eliminate any noise, but nothing higher as the peak came out pretty clean, and I did not want to bias brightwards, again using a middleward approach to minimize both bias and noise
#### Systematic Uncertainty
- The following table is the variations in the TRGB created by adjusting tau, color range, and spatial clipping for NGC 1404. Poisson results were very similar to hatt results, eliminating that source of error. Parameters mentioned in the first column are the only ones being changed. Since my color_hi for the F606W filter was set to 1.4 and 2 for the F555W filter, I will test a color_hi variation of +0.1 as opposed to +0.2, to avoid going past the flat TRGB limits placed by Jang & Lee 2017

| Parameter | Value | NGC1404 TRGB Magnitude |
|-----------|-------|----------------|
| none | N/A | 26.797
| tau | 0.05 | **26.857**
| tau | 0.15 | 26.787
| tau | 0.2 | 26.827
| color_hi F606W | 1.5 | 26.797
| color_hi F606W | 1.2 | **N_faint < 400**
| color_lo F606W| 1.15 | 26.777
| color_lo F606W| 0.75 | 26.807
| color_lo F555W | 1.6 | 26.797
| color_lo F555W | 1.2 | 26.797
| color_hi F555W| 2.1 | 26.797
| color_hi F555W| 1.8 | 26.797
| scale_factor | 1.0 | **27.091**
| scale_factor | 1.2 | **27.118**

- While everything else maintains within the 0.03 magnitude variations of the TRGB, a tau of 0.05 creates a significant variation of ~2x the healthy amount, caused by noise in NGC 1404 probably due to the minimal spatial clipping. Here, I chose to get a sufficient sample of NGC 1404 halo stars and risk some contamination rather than undersample and risk inaccurate findings. I was also unable to calculate a TRGB when adjusting the color_hi F606W value to 1.2 due to the lack of 400 stars 1 magnitude below the TRGB, thus I am unable to find out if there is systematic error with a lower color_hi constraint on the F606W filter. Additionally, the large error when increasing scale_factor to 1 and 1.2 can be attributed to an insufficient sample size, thus providing incorrect determinations.
- With a consistent use of EDD, minimal inclination, and no known high densities, photometric zero-point errors, internal extinction, and crowding/blending are respectively very minimal. However, since the galaxy is passing through Fornax intracluster medium, the extinction could be very significant with this galaxy, introducing a major systematic uncertainty.
![CMD Plots for NG1404](images/NGC1404.png)
*CMD plots of NG4258 and NGC1404 with selection band 0.95-1.4 on F606W filter*
### NGC 1365
#### Distance Modulus / Tip-Contrast
| Parameter | Value |
|-----------|-------|
| mu_target | 31.209 ± 0.060 mag |
| d_Mpc | 17.45 ± 0.48 Mpc |
| tip_contrast_anchor | 5.1 |
| tip_contrast_target | 4.91 |
#### Parameter Choices
- For the F606W filter, I chose to have color constraints of 0.7-1.42, as on the lower end differences up until 1 and and down until 0.5 had minimal impact and only introduced further unreliable contamination, and on the higher end there was a slightly lower than 1.5 mag drop off in star density, thus I lowered it to 1.42 to eliminate contamination but maintain a sufficient sample size
- For the F555W filter, I chose color constraints of 1-2.1, as on the lower end differences up until 1.5 and and down until 0.5 had minimal impact and only introduced further unreliable contamination, and anything lower than 2.1 introduced a significant amount of noise due to undersampling
- Moved scale_factor at 0.8, as any larger scale_factor resulted in multiple peaks in the bootstrap sample and higher error, a direct consequence of undersampling. Did not go any lower so as to avoid contamination by the galactic bulge
- Since this was a noisier galaxy with less definition, I decided to increase tau to 0.13 too smooth out that lower definition, and also to minimize noise while still maintaining an unbiased result
#### Systematic Uncertainty
- The following table is the variations in the TRGB created by adjusting tau, color range, and spatial clipping for NGC 1365. Poisson results were very similar to hatt results, eliminating that source of error. Parameters mentioned in the first column are the only ones being changed. Since my color_hi for the F606W filter was set to 1.42 and 2.1 for the F555W filter, I will respectively only test a color_hi variation of +0.08 as opposed to +0.2 for the F606W filter, and only a color_hi variation of -0.2 as opposed so both -0.2 and +0.2, so as to avoid going past the flat TRGB limits placed by Jang & Lee 2017

| Parameter | Value | NGC1365 TRGB Magnitude |
|-----------|-------|----------------|
| none | N/A | 27.359
| tau | 0.05 | **27.449**
| tau | 0.1 | **27.399**
| tau | 0.15 | 27.339
| tau | 0.2 | **27.319**
| color_hi F606W | 1.5 | 27.349
| color_hi F606W | 1.22 | **27.399**
| color_lo F606W| 0.9 | 27.349
| color_lo F606W| 0.5 | 27.359
| color_lo F555W | 1.2 | 27.359
| color_lo F555W | 0.8 | 27.359
| color_hi F555W| 1.9 | 27.359
| scale_factor | 1.0 | 27.359
| scale_factor | 1.2 | 27.328

- While everything else maintains within the 0.03 magnitude variations of the TRGB, any values of tau below or above the ~0.13-0.15 range introduce respectively too much noise or too much TRGB shifting due to excessive smoothing, making this a smoothing-dependent galaxy. Additionally, lowering the F606W filter color_hi limited the sample too much, introducing error and creating the unhealthy error above 0.03 mag of the TRGB.
- With a consistent use of EDD, low extinction, and no known high densities, photometric zero-point errors, extinction, and crowding/blending are respectively very minimal. However, the inclination is a little higher than other galaxies, introducing the risk of dust contaminating the results.
![CMD Plots for NG1365](images/NGC1365.png)
*CMD plots of NG4258 and NGC1365 with selection band 0.7-1.42 on F606W filter*

### M96
#### Distance Modulus / Tip-Contrast
| Parameter | Value |
|-----------|-------|
| mu_target | 30.431 ± 0.052 mag |
| d_Mpc | 12.20 ± 0.29 Mpc |
| tip_contrast_anchor | 8.0 |
| tip_contrast_target | 4.04 |
#### Parameter Choices
- For the F606W filter, I chose to have color constraints of 0.89-1.4 as 0.89 falls right along where the line between high and low star density on the CMD changes from diagonal to vertical, preventing contamination as much as possible, and 1.4 instead of 1.5 as to eliminate other peaks/further conatmination, while still maintaining a sufficient sample size. I kept color constraints of 1.5-2.1 for the F555W filter, similar to that of the default as other wider, more limited, or shifted constraints yielded excessive contamination.
- I kept the scale_factor at 1 for this galaxy, as anything lower only affected the anchor galaxy while simultaneously introducing more room for contamination, and anything higher limited the sample size excessively
- On this galaxy, I used a tau of 0.1 to eliminate any noise, but nothing higher as the peak came out pretty clean, and I did not want to bias brightwards, again using a middleward approach to minimize both bias and noise
#### Systematic Uncertainty
- The following table is the variations in the TRGB created by adjusting tau, color range, and spatial clipping for NGC 1365. Poisson results were very similar to hatt results, eliminating that source of error. Parameters mentioned in the first column are the only ones being changed. I will only test a variance of -0.2 on the color_hi constraint for the F555W filter, and a +0.1 instead of +0.2 variance on the color_hi constraint for the F606W filter so as to avoid going past the flat TRGB limits placed by Jang & Lee 2017

| Parameter | Value | M96 TRGB Magnitude |
|-----------|-------|----------------|
| none | N/A | 26.433
| tau | 0.05 | 26.453
| tau | 0.15 | **26.383**
| tau | 0.2 | **26.343**
| color_hi F606W | 1.5 | 26.413
| color_hi F606W | 1.2 | 26.403
| color_lo F606W| 1.09 | 26.453
| color_lo F606W| 0.69 | 26.433
| color_lo F555W | 1.7 | 26.433
| color_lo F555W | 1.3 | 26.433
| color_hi F555W| 1.9 | 26.433
| scale_factor | 0.8 | 26.433
| scale_factor | 1.2 | 26.422

- While everything else maintains within the 0.03 magnitude variations of the TRGB, any values of tau below or above the 0.05-0.1 range introduce too much TRGB shifting due to excessive smoothing, making this a smoothing-dependent galaxy.
- With a consistent use of EDD, low extinction, and no known high densities, photometric zero-point errors, extinction, and crowding/blending are respectively very minimal. However, even though the inclination is pretty ideal, M96 is known for having inconsistent and uneven patches of dust and gas throughout, thus leading to a real possibility of internal extinction blurring results and introducing systematic error
![CMD Plots for M96](images/M96.png)
*CMD plots of NG4258 and M96 with selection band 0.89-1.4 on F606W filter*

## H0 Measurement
- M96 was not accepted by either the TRGB_to_H0 or mu_to_H0 pipeline, so its contribution to any H0 values is excluded
### mu_to_H0 Pipeline Results
- Only NGC1365 was accepted for this pipeline, and using the mu_to_H0.py file with my mu and sigma_mu values for this galaxy, I got:

| M_B | H0 | chi2/dof |
|-----|----|----------|
| -19.3524 ± 0.1160 mag | 69.8231 ± 3.7304 km/s/Mpc | 234.6/277 = 0.847

- Although this value looks totally plausible, with the way that this pipeline works, I am really just changing one value out of 40 default ones, meaning that this H0 is not very useful
### TRGB_to_H0 Pipeline Results
- NGC1365, NGC1316, and NGC1404 were accepted for this pipeline, and using the mu_to_H0.py file with my mu and sigma_mu values for these galaxies, I got:

| Individual/Combined | M_B | H0 | chi2/dof |
|---------------------|-----|----|----------|
| I/NGC1365 | -19.2647 ± 0.2889 mag | 72.9842 ± 9.7143 km/s/Mpc | 435.6/496 = 0.878
| I/NGC1404 | -18.2506 ± 0.1202 mag | 116.4283 ± 6.4548 km/s/Mpc | 438.3/497 = 0.882
| I/NGC1316 | -19.3732 ± 0.2165 mag | 69.4275 ± 6.9271 km/s/Mpc | 436.2/497 = 0.878
| C/All | -18.5963 ± 0.0996 mag | 99.3152 ± 4.5648  km/s/Mpc | 465.3/502 = 0.927

- NGC1365 and NGC1316 seem decently consistent with modern measured values of H0, albeit with significant error, but NGC1404 seems too far off with ~116 km/s/Mpc, which can probably be attributed to the fact that NGC1404 is passing through Fornax intracluster medium, likely producing significant error in the population of halo stars used to calculate the TRGB.