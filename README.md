
# Influencer Campaign Performance Dashboard

# Dashoard Link: https://app.powerbi.com/groups/me/reports/f618adcd-834f-456d-9a93-04d150f6daa9/be503ec2f2d9b5558062?experience=power-bi

# Problem Statement:
This dashboard helps marketing teams and brands understand the performance of their influencer campaigns better. It tracks various aspects of influencer activities—such as follower count, post engagement, and campaign-based revenue—to measure the return on investment (ROI) and optimize future strategies.

Through insights like top-performing influencers, platforms with the highest engagement, and personas with better ROI, teams can identify what’s working and what needs improvement. The dashboard allows performance tracking at the influencer and post level and highlights poor-performing campaigns to avoid similar patterns in the future.

Since campaign performance varies across platforms and influencer types, filtering by brand, product, platform, or category helps decision-makers get a focused view. Also, since the dashboard enables ROI and incremental ROAS calculation, it helps ensure that marketing budgets are used efficiently.


### Steps followed:


- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- step 3: Created a new table named ProductBrand using the Enter Data feature to map products to their respective brands. This table was then merged with the tracking_data table using a left join on the product column as the primary key, in order to incorporate the missing brand information.
- step 4: Connected tables using relationships in Model View:
(a) influencers[influencer_id] → posts[influencer_id]

(b) influencers[influencer_id] → tracking_data[influencer_id]

(c) influencers[influencer_id] → payouts[influencer_id]

(d) influencers[influencer_id] → posts[influencer_id]

(e) product_brand[product] → tracking_data[product]

- Step 5 : For calculating Measures Table, null values were not taken into account.

- step 6: Measures Table for Basic Metrices was created:
(a) Total Orders

Following DAX expression was written for the same,
Total Orders = SUM(tracking_data[orders])

(b) Total Payout

Following DAX expression was written for the same,
Total Payout = SUM(payouts[total_payout])

(c) Total Revenue

Following DAX expression was written for the same,
Total Revenue = SUM(tracking_data[revenue])

- step 7: Measures Table for Return on Ad Spend (ROAS) was created:

Following DAX expression was written for the same,
ROAS = DIVIDE([Total Revenue], [Total Payout])

- step 8: Measures Table  Incremental ROAS (assume 30% baseline) was created:
(a) Baseline Revenue

Following DAX expression was written for the same,
Baseline Revenue = [Total Revenue] * 0.3

(b) Incremental Revenue

Following DAX expression was written for the same,
Incremental Revenue = [Total Revenue] - [Baseline Revenue]

(c) Incremental ROAS

Following DAX expression was written for the same,
Incremental ROAS = DIVIDE([Incremental Revenue], [Total Payout])

- step 9: Visual cards were created for Measures:
(a) Total Orders: 313
(b) Total Payout: 174k
(c) Total Revenue: 13.03k
(d) ROAS: 0.07
(e)  Incremental ROAS: 0.05

Snap of Visual cards,
![Image](https://github.com/user-attachments/assets/702988ac-b6e9-47f9-82d2-32a0182323f0)


- step 10: Slicer Filter were created:
(a) category
(b) product
(c) brand
(d) basis
(e) platform
(f) gender

snap of Slicer Filter,
![Image](https://github.com/user-attachments/assets/84dc65c5-30ea-4359-ad17-371ae369dbd9)

- step 11: New measure was created to calculate Return on investment

Following DAX expression was written for the same, 
ROI = 
DIVIDE(
    [Total Revenue] - [Total Payout],
    [Total Payout]
) * (-1)
 In ROI multiply by (-1) is done to overcome negative values.

- step 12: New measure was created to calculate Engagement Rate:

(a) Total Comments

Following DAX expression was written for the same, 
Total Comments = SUM(posts[comments])

(b) Total Likes

Following DAX expression was written for the same, 
Total Likes = SUM(posts[likes])

(c) Total Reach

Following DAX expression was written for the same, 
Total Reach = SUM(posts[reach])

(d) Engagement Rate

Following DAX expression was written for the same, 
Engagement Rate =
DIVIDE(SUM(posts[likes]) + SUM(posts[comments]), SUM(posts[reach]))


- step 13: New measure was created to calculate Insights:

(a) ROAS per Influencer

Following DAX expression was written for the same, 
ROAS per Influencer = 
DIVIDE(
    CALCULATE(SUM(tracking_data[revenue])),
    CALCULATE(SUM(payouts[total_payout]))
)


(b) Lowest ROAS Influencer

Following DAX expression was written for the same, 
Lowest ROAS Influencer = 
CALCULATE (
    SELECTEDVALUE ( influencers[name] ),
    TOPN(
        1,
        ADDCOLUMNS(
            SUMMARIZE(influencers, influencers[influencer_id], influencers[name]),
            "ROAS", [ROAS per Influencer]
        ),
        [ROAS per Influencer], ASC
    )
)


(c) Top Engaged Influencer 

Following DAX expression was written for the same, 
Top Engaged Influencer = 
CALCULATE (
    SELECTEDVALUE ( influencers[name] ),
    TOPN(
        1,
        ADDCOLUMNS(
            SUMMARIZE(influencers, influencers[influencer_id], influencers[name]),
            "Engagement", [Engagement Rate]
        ),
        [Engagement Rate], DESC
    )
)

(d) ROI per Influencer

Following DAX expression was written for the same, 
ROI per Influencer = 
DIVIDE(
    CALCULATE(SUM(tracking_data[revenue])) - CALCULATE(SUM(payouts[total_payout])),
    CALCULATE(SUM(payouts[total_payout]))
) * (-1)

snap of Insights,
![Image](https://github.com/user-attachments/assets/93a7e036-27ab-4778-acd9-ec4d8b182306)

- step 14: The report was then published to Power BI Service.

# Snapshot of Dashboard
![Image](https://github.com/user-attachments/assets/01f3a1ec-7bb2-435c-973f-bf12d7a3cc33)

![Image](https://github.com/user-attachments/assets/c986214e-35ff-4196-abc2-4a3f772a4b98)

![Image](https://github.com/user-attachments/assets/a226125b-bae6-4acf-bef1-c674aa2f8943)

## Insights Overall

A four page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

## Revenue by platform
(a) Instagram has highest: 6.9k
(b) YouTube: 4.7k
(c) Twitter: 1.4k

## Total Revenue by Year(2025) & Month
(a) April: 0.1k
(b) May: 5.4k
(c) June: 4.0k
(d) July: 3.6k

## ROAS by campaign
(a) GlowUp2025: 0.023
(b) PowerPush: 0.019
(c) FitStart2025: 0.017
(d) ShredSeason: 0.015
(e) BeautyBoost24: 0.001

## ROAS by Product
(a) Omega-3: 0.021
(b) Creatine: 0.015
(c) Multivitamins: 0.014
(d) Pre Workout: 0.013
(e) Whey Protein: 0.013

## Post reach by platform
(a) Instagram has highest: 5.6M
(b) YouTube: 4.5M
(c) Twitter: 1.5M

## Total Payout by basis
(a) post: 111k (64.12%)
(b) order: 62k (35.88%)

## Insights by Influencer type (category):

## (a) Beauty:

## Total Revenue
3.00k

## Total Payout
36k

## Total orders
83

## ROAS
0.08

## Incremental ROAS
0.06

## Revenue by platform

(a) YouTube has highest: 1125
(b) Twitter: 1063
(c) Instagram: 815

## Total Revenue by Year (2025) & Month
(a) May: 1543
(b) July: 732
(c) June: 728

## ROAS by campaign
(a) ShredSeason: 0.025
(b) GlowUp2025: 0.023
(c) FitStart2025: 0.020
(d) PowerPush: 0.011
(e) BeautyBoost24: 0.006

## ROAS by Product
(a) Whey Protein: 0.026
(b) Omega-3: 0.018
(c) Pre Workout: 0.014
(d) Multivitamins: 0.014
(e) Creatine: 0.013

## Post reach by platform
(a) Twitter has highest: 1.26M
(b) YouTube: 1.13M
(c) Instagram: 0.84M

## Total Payout by basis
(a) Order: 25K (69.86%)
(b) Post: 11K (30.14%)

## ROAS per Influencer
0.08

## ROI per Influencer
0.92

## Lowest ROAS Influencer
Jesse Lee

## Top Engaged Influencer
Tracy Murillo


## (b) Fitness:

## Total Revenue
4.70k

## Total Payout
74k

## Total Orders
110

## ROAS
0.06

## Incremental ROAS
0.04

## Revenue by platform
(a) Instagram has highest: 3.1k
(b) YouTube: 1.4k
(c) Twitter: 0.2k

## Total Revenue by Year (2025) & Month
(a) May: 2.4k
(b) July: 1.2k
(c) June: 1.0k
(d) April: 0.1k

## ROAS by campaign
(a) GlowUp2025: 0.025
(b) PowerPush: 0.018
(c) FitStart2025: 0.011
(d) ShredSeason: 0.010

## ROAS by Product
(a) Omega-3: 0.021
(b) Creatine: 0.016
(c) Multivitamins: 0.012
(d) Pre Workout: 0.011
(e) Whey Protein: 0.003

## Post reach by platform
(a) Instagram has highest: 2.21M
(b) YouTube: 0.93M
(c) Twitter: 0.13M

## Total Payout by basis
(a) Post: 68K (91.89%)
(b) Order: 6K (8.11%)

## ROAS per Influencer
0.06

## ROI per Influencer
0.94

## Lowest ROAS Influencer
Jennifer Thompson

## Top Engaged Influencer
Ian Jackson MD



## (c) Lifestyle:

## Total Revenue
1.72k

## Total Payout
34k

## Total Orders
36

## ROAS
0.05

## Incremental ROAS
0.04

## Revenue by platform
(a) Instagram has highest: 1056
(b) YouTube: 475
(c) Twitter: 192

## Total Revenue by Year (2025) & Month
(a) May: 661
(b) July: 531
(c) June: 532


## ROAS by campaign
(a) GlowUp2025: 0.015
(b) PowerPush: 0.023
(c) FitStart2025: 0.008
(d) ShredSeason: 0.005

## ROAS by Product
(a) Omega-3: 0.004
(b) Creatine: 0.013
(c) Multivitamins: 0.015
(d) Pre Workout: 0.005
(e) Whey Protein: 0.015

## Post reach by platform
(a) Instagram has highest: 0.71M
(b) YouTube: 0.37M
(c) Twitter: 0.13M

## Total Payout by basis
(a) Post: 31K (91.58%)
(b) Order: 3K (8.42%)

## ROAS per Influencer
0.05

## ROI per Influencer
0.95

## Lowest ROAS Influencer
Robert Liu

## Top Engaged Influencer
Robert Liu



## (d) Nutrition:

## Total Revenue
1.20k

## Total Payout
6708

## Total Orders
33

## ROAS
0.18

## Incremental ROAS
0.13

## Revenue by platform
(a) Instagram has highest: 679
(b) YouTube: 521


## Total Revenue by Year (2025) & Month
(a) May: 176
(b) July: 659
(c) June: 365


## ROAS by campaign
(a) GlowUp2025: 0.053
(b) PowerPush: 0.056
(c) FitStart2025: 0.036
(d) ShredSeason: 0.005

## ROAS by Product
(a) Omega-3: 0.036
(b) Creatine: 0.031
(c) Multivitamins: 0.036
(d) Pre Workout: 0.041
(e) Whey Protein: 0.035

## Post reach by platform
(a) Instagram has highest: 0.86M
(b) YouTube: 0.69M


## Total Payout by basis
(a) Post: 2K (22.93%)
(b) Order: 5K (77.07%)

## ROAS per Influencer
0.18

## ROI per Influencer
0.82

## Lowest ROAS Influencer
Justin Snyder

## Top Engaged Influencer
Justin Snyder




## (e) Tech:

## Total Revenue
2.40k

## Total Payout
23k

## Total Orders
51

## ROAS
0.10

## Incremental ROAS
0.07

## Revenue by platform
(a) Instagram has highest: 1211
(b) YouTube: 1189


## Total Revenue by Year (2025) & Month
(a) May: 637
(b) July: 744
(c) June: 1019


## ROAS by campaign
(a) GlowUp2025: 0.018
(b) PowerPush: 0.028
(c) FitStart2025: 0.040
(d) ShredSeason: 0.017

## ROAS by Product
(a) Omega-3: 0.048
(b) Creatine: 0.012
(c) Multivitamins: 0.009
(d) Pre Workout: 0.022
(e) Whey Protein: 0.011

## Post reach by platform
(a) Instagram has highest: 1.34M
(b) YouTube: 1.00M


## Total Payout by basis
(a) Order: 23K (100%)


## ROAS per Influencer
0.10

## ROI per Influencer
0.90

## Lowest ROAS Influencer
Colleen Rivera

## Top Engaged Influencer
Colleen Rivera




## Insights by product:

## (a) Creatine:

## Total Revenue
2.55k

## Total Payout
174k

## Total orders
61

## ROAS
0.01

## Incremental ROAS
0.01

## Revenue by platform

(a) YouTube: 1372
(b) Twitter: 1079
(c) Instagram: 98

## Total Revenue by Year (2025) & Month
(a) May: 855
(b) July: 308
(c) June: 1265
(d) April: 121

## ROAS by campaign
(a) ShredSeason: 0.0019
(b) GlowUp2025: 0.0072
(c) FitStart2025: 0.0027
(d) PowerPush: 0.0028


## ROAS by Product
(a) Creatine: 0.015

## Post reach by platform
(a) Twitter: 1.5M
(b) YouTube: 4.5M
(c) Instagram: 5.6M

## Total Payout by basis
(a) Order: 62K (35.88%)
(b) Post: 111K (64.12%)

## ROAS per Influencer
0.01

## ROI per Influencer
0.99

## Top Engaged Influencer
Ian Jackson MD



## (b) Multivitamins:

## Total Revenue
2.36k

## Total Payout
174k

## Total orders
53

## ROAS
0.01

## Incremental ROAS
0.01

## Revenue by platform

(a) YouTube: 983
(b) Twitter: 927
(c) Instagram: 445

## Total Revenue by Year (2025) & Month
(a) May: 1152
(b) July: 723
(c) June: 480


## ROAS by campaign
(a) ShredSeason: 0.0028
(b) GlowUp2025: 0.0045
(c) FitStart2025: 0.0029
(d) PowerPush: 0.0021
(e) BeautyBoost24: 0.0012

## ROAS by Product
(a) Multivitamins: 0.014

## Post reach by platform
(a) Twitter: 1.5M
(b) YouTube: 4.5M
(c) Instagram: 5.6M

## Total Payout by basis
(a) Order: 62K (35.88%)
(b) Post: 111K (64.12%)

## ROAS per Influencer
0.01

## ROI per Influencer
0.99

## Top Engaged Influencer
Ian Jackson MD


## (c) Omega-3:

## Total Revenue
3.64k

## Total Payout
174k

## Total orders
79

## ROAS
0.02

## Incremental ROAS
0.01

## Revenue by platform

(a) YouTube: 1061
(b) Twitter: 510
(c) Instagram: 2069

## Total Revenue by Year (2025) & Month
(a) May: 1792
(b) July: 1097
(c) June: 751


## ROAS by campaign
(a) ShredSeason: 0.0049
(b) GlowUp2025: 0.0044
(c) FitStart2025: 0.0061
(d) PowerPush: 0.0056


## ROAS by Product
(a) Omega-3: 0.021

## Post reach by platform
(a) Twitter: 1.5M
(b) YouTube: 4.5M
(c) Instagram: 5.6M

## Total Payout by basis
(a) Order: 62K (35.88%)
(b) Post: 111K (64.12%)

## ROAS per Influencer
0.02

## ROI per Influencer
0.98

## Top Engaged Influencer
Ian Jackson MD



## (d) Pre Workout:

## Total Revenue
2.31k

## Total Payout
174k

## Total orders
62

## ROAS
0.01

## Incremental ROAS
0.01

## Revenue by platform

(a) YouTube: 822
(b) Twitter: 55
(c) Instagram: 1432

## Total Revenue by Year (2025) & Month
(a) May: 781
(b) July: 664
(c) June: 864


## ROAS by campaign
(a) ShredSeason: 0.0017
(b) GlowUp2025: 0.0037
(c) FitStart2025: 0.0023
(d) PowerPush: 0.0056


## ROAS by Product
(a) Pre Workout: 0.013

## Post reach by platform
(a) Twitter: 1.5M
(b) YouTube: 4.5M
(c) Instagram: 5.6M

## Total Payout by basis
(a) Order: 62K (35.88%)
(b) Post: 111K (64.12%)

## ROAS per Influencer
0.01

## ROI per Influencer
0.99

## Top Engaged Influencer
Ian Jackson MD



## (e) Whey Protein:

## Total Revenue
2.17k

## Total Payout
174k

## Total orders
58

## ROAS
0.01

## Incremental ROAS
0.01

## Revenue by platform
(a) YouTube: 504
(b) Twitter: 311
(c) Instagram: 1359

## Total Revenue by Year (2025) & Month
(a) May: 786
(b) July: 779
(c) June: 609


## ROAS by campaign
(a) ShredSeason: 0.0035
(b) GlowUp2025: 0.0028
(c) FitStart2025: 0.0030
(d) PowerPush: 0.0033


## ROAS by Product
(a) Whey Protein: 0.013

## Post reach by platform
(a) Twitter: 1.5M
(b) YouTube: 4.5M
(c) Instagram: 5.6M

## Total Payout by basis
(a) Order: 62K (35.88%)
(b) Post: 111K (64.12%)

## ROAS per Influencer
0.01

## ROI per Influencer
0.99

## Top Engaged Influencer
Ian Jackson MD


## Insights by Brand:

## (a) HKVitals:

## Total Revenue
6.00k

## Total Payout
174k

## Total orders
132

## ROAS
0.03

## Incremental ROAS
0.02

## Revenue by platform
(a) YouTube: 2.0k
(b) Twitter: 1.0k
(c) Instagram: 3.0k

## Total Revenue by Year (2025) & Month
(a) May: 2.9k
(b) July: 1.8k
(c) June: 1.2k


## ROAS by campaign
(a) ShredSeason: 0.0077
(b) GlowUp2025: 0.0089
(c) FitStart2025: 0.0090
(d) PowerPush: 0.0077
(e) BeautyBoost24: 0.0012

## ROAS by Product
(a) Omega-3: 0.021
(b) Multivitamins: 0.014

## Post reach by platform
(a) Twitter: 1.5M
(b) YouTube: 4.5M
(c) Instagram: 5.6M

## Total Payout by basis
(a) Order: 62K (35.88%)
(b) Post: 111K (64.12%)

## ROAS per Influencer
0.03

## ROI per Influencer
0.97

## Top Engaged Influencer
Ian Jackson MD

## Lowest ROAS Influencer
Jennifer Thompson




## (b) MuscleBlaze:

## Total Revenue
7.03k

## Total Payout
174k

## Total orders
181

## ROAS
0.04

## Incremental ROAS
0.03

## Revenue by platform
(a) YouTube: 2.7k
(b) Twitter: 0.5k
(c) Instagram: 3.9k

## Total Revenue by Year (2025) & Month
(a) May: 2.4k
(b) July: 1.8k
(c) June: 2.7k
(d) April: 0.1k

## ROAS by campaign
(a) ShredSeason: 0.007
(b) GlowUp2025: 0.014
(c) FitStart2025: 0.008
(d) PowerPush: 0.012


## ROAS by Product
(a) Creatine: 0.015
(b) Pre Workout: 0.013
(c) Whey Protein: 0.013

## Post reach by platform
(a) Twitter: 1.5M
(b) YouTube: 4.5M
(c) Instagram: 5.6M

## Total Payout by basis
(a) Order: 62K (35.88%)
(b) Post: 111K (64.12%)

## ROAS per Influencer
0.04

## ROI per Influencer
0.96

## Top Engaged Influencer
Ian Jackson MD

## Lowest ROAS Influencer
Barbara Hall


## Insights by Platform:

## (a) Instagram:

## Total Revenue
6.87k

## Total Payout
81k

## Total orders
160

## ROAS
0.08

## Incremental ROAS
0.06

## Revenue by platform

(a) Instagram: 6.9k

## Total Revenue by Year (2025) & Month
(a) May: 2.5k
(b) June: 2.4k
(c) July: 2.0k

## ROAS by campaign
(a) ShredSeason: 0.013
(b) GlowUp2025: 0.027
(c) FitStart2025: 0.020
(d) PowerPush: 0.025


## ROAS by Product
(a) Whey Protein: 0.017
(b) Omega-3: 0.025
(c) Pre Workout: 0.018
(d) Multivitamins: 0.011
(e) Creatine: 0.013

## Post reach by platform
(a) Instagram: 5.6M

## Total Payout by basis
(a) Order: 25K (30.2%)
(b) Post: 57k (69.8%)

## ROAS per Influencer
0.08

## ROI per Influencer
0.92

## Lowest ROAS Influencer
Jill Soto

## Top Engaged Influencer
Ian Jackson MD



## (b) Twitter:

## Total Revenue
1.42k

## Total Payout
39k

## Total orders
37

## ROAS
0.04

## Incremental ROAS
0.03

## Revenue by platform

 (a) Twitter: 1420

## Total Revenue by Year (2025) & Month
(a) May: 1040
(b) June: 176
(c) July: 204

## ROAS by campaign
(a) ShredSeason: 0.011
(b) GlowUp2025: 0.004
(c) FitStart2025: 0.009
(d) PowerPush: 0.006
(e) BeautyBoost24: 0.005


## ROAS by Product
(a) Whey Protein: 0.008
(b) Omega-3: 0.013
(c) Pre Workout: 0.001
(d) Multivitamins: 0.011
(e) Creatine: 0.003

## Post reach by platform
(a) Twitter: 1.51M

## Total Payout by basis
(a) Order: 14K (35.17%)
(b) Post: 25k (64.83%)

## ROAS per Influencer
0.04

## ROI per Influencer
0.96

## Lowest ROAS Influencer
Robert Liu

## Top Engaged Influencer
Robert Liu


## (c) Twitter:

## Total Revenue
4.74k

## Total Payout
54k

## Total orders
116

## ROAS
0.09

## Incremental ROAS
0.06

## Revenue by platform

 (a) YouTube: 4.7k

## Total Revenue by Year (2025) & Month
(a) April: 121
(a) May: 1861
(b) June: 1422
(c) July: 1337

## ROAS by campaign
(a) ShredSeason: 0.020
(b) GlowUp2025: 0.029
(c) FitStart2025: 0.018
(d) PowerPush: 0.021

## ROAS by Product
(a) Whey Protein: 0.009
(b) Omega-3: 0.020
(c) Pre Workout: 0.015
(d) Multivitamins: 0.018
(e) Creatine: 0.026

## Post reach by platform
(a) YouTube: 4.5M

## Total Payout by basis
(a) Order: 24K (44.98%)
(b) Post: 29k (55.02%)

## ROAS per Influencer
0.09

## ROI per Influencer
0.91

## Lowest ROAS Influencer
Jennifer Thompson

## Top Engaged Influencer
Jennifer Thompson




