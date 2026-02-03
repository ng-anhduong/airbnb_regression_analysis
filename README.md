## Airbnb: Do megahosts improve guest experience?
### A regression analysis

#### Project goal

This project studies whether host scale/type is associated with differences in overall guest ratings (review_scores_rating). Hosts are grouped into:

Individual: exactly 1 listing

Boutique: 2–20 listings

Megahost: 21+ listings

The analysis combines descriptive statistics, visualization, and linear regression (simple + controlled models).

#### Data

Dataset: airbnb.csv

Size: 19,671 listings, 79 variables

Outcome variable: review_scores_rating (overall rating)

Feature engineering: host types

A categorical variable host_type was created from host_total_listings_count.

Distribution of listings by host type

host_type	No. listings	share
boutique	10,844	55.13%
individual	4,037	20.52%
megahost	4,790	24.35%

Interpretation: most listings in the dataset are run by small-to-mid sized operators (“boutique”), with nearly a quarter run by very large hosts (“megahosts”).

#### Exploratory finding: ratings are highly concentrated near the top

A histogram of review_scores_rating shows ratings clustered heavily around ~4.5–5.0.

Proportion of listings with rating above 4 stars: 0.990 (~99%)

Interpretation: because ratings are tightly packed near the top end, even small differences in average ratings can be meaningful for visibility and filtering.

#### Regression analysis
##### Model 1: simple regression (host type only)

Model:
review_scores_rating = β0 + β1⋅megahost + β2⋅boutique + u

Baseline category: individual.

Key estimates:

Megahost vs individual: −0.198 (p < 0.001)

Boutique vs individual: −0.079 (p < 0.001)

𝑅^2 ≈ 0.093

Meaning: compared to individual hosts, megahost and boutique listings have lower predicted ratings on average, and the differences are statistically significant.

Model check: regression coefficients match sample mean differences

The intercept (𝛽0) equals the sample mean rating for individual listings (4.844).

The megahost coefficient equals the difference in sample means between megahost and individual listings (−0.198).

This validates the interpretation of dummy-variable regression coefficients as mean differences (with the chosen baseline).

Confounding discussion: which controls matter?

Three candidate confounders were discussed:

room_type: plausible confounder → included
Rationale: room type may be correlated with host type and also with guest experience.

minimum_nights: not expected to directly affect rating after the stay → not included

host_identity_verified: not expected to directly affect rating after the stay → not included

##### Model 2: add room_type

Model: review_scores_rating ~ host_type + room_type

Key estimates:

Megahost vs individual: −0.196

Boutique vs individual: −0.080

𝑅^2 ≈ 0.094

Takeaway: controlling for room type barely changes the host-type effects; the negative relationship remains.

Model 3 (final): add price as an additional control

price was converted from a “$…” string into numeric and added to the model:
review_scores_rating ~ host_type + room_type + price

Key estimates:

Megahost vs individual: −0.212

Boutique vs individual: −0.087

Entire home/apt (vs private room): −0.0566

Price: +0.000297 per $1

𝑅^2 ≈ 0.122

Interpretation: after holding room type and price constant, megahost listings are predicted to score ~0.21 stars lower than comparable individual-host listings, and boutique listings score ~0.09 lower.

#### Comment on whether your final estimated impact of megahost and boutique host statusis economically meaningful
The final estimated impact of megahost and boutique host status on guest experience is economically meaningful. 

Holding room type and price constant, megahost listings are predicted to have about 0.21 fewer stars than comparable individual-host listings, and boutique listings are predicted to have about 0.09 fewer stars than comparable individual-host listings. 

Looking at the histogram of review_scores_rating in question 2, we can see that many listings in this dataset have ratings near 4.8 - 4.9. Therefore, a drop of 0.21 or 0.09 can be very large, and can move a listing below common rating cutoffs that guests use when filtering, which materially reduces visibility. That is why the estimated impacts are economically meaningful.

#### Comment on policy and managerial implications of the findings
Should Airbnb keep courting megahosts?
Answer: No, not without conditions.

Guest experience: Megahost listings score about 0.21 stars lower than otherwise comparable individual-host listings, which means megahost listings do not provide as good an experience as comparable individual-host listings. So growing the supply of megahost listings risks a worse guest experience.

Business outcomes: Courting more megahosts risks increasing the share of lower-rated listings, which can affect the overall platform rating and can erode trust and brand perception. Because individual hosts tend to earn higher ratings, growing individual-host supply supports repeat use and long-run revenue.

However, as megahost listings bring additional choices for the customers besides individual hosts, I think we should not stop growing megahosts entirely, but let them grow only with clear quality bars. For example, Airbnb can require a minimum average rating to stay highly ranked, enforce fast response times and cleaning/check-in standards, and down-rank or pause listings with persistent low ratings or complaints.
