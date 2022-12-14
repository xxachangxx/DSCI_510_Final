### 1. What’s the name of your final project? Please describe it as a research question and provide a short description

* **Project name**: What is the pattern of the spread of COVID-19 pandemic: From the perspective of time and space
* **Description**: the project would like to explore some features of the spreading of COVID-19. The subquestion include:
  1. What is the pattern of the development of COVID-19? If there is any burst, and if there is, what country or continent "contribute a lot" to the burst?
  2. Is there any relationships between the severity of COVID-19 and the economy/medical level/population structure

### 2. How to run your code?

The code is in .ipynb file, and can be run using Cell->Run All.

It should be noticed that the last part of the notebook used library bar_chart_race, which will generate a vedio.

The github link is: [github]()

### 3. What data did you collect? How did you collect them? How many data samples did you collect?

* I collect covid-19 data including total cases, new cases for each country, and other information about that country like hospital bed per thousand, excess mortality. For detailed information, please check ./data/codebook.csv

* Collect method, use `wget` to directly download csv file from [github](https://github.com/owid/covid-19-data/tree/master/public/data)

  ```bash
  # get raw data file from github repository
  ! wget https://raw.githubusercontent.com/owid/covid-19-data/master/public/data/owid-covid-data.csv
  
  # get codebook
  ! wget https://raw.githubusercontent.com/owid/covid-19-data/master/public/data/owid-covid-codebook.csv
  
  # move to ./data
  ! mv owid-covid-data.csv ./data/owid-covid-data.csv
  ! mv owid-covid-codebook.csv ./data/owid-covid-codebook.csv
  ```

* In total there are  238710 samples.
* **Change**: For original plan, data from [VACCOVID API](https://vaccovid-coronavirus-vaccine-and-treatment-tracker.p.rapidapi.com/api/npm-covid-data/) should also be colleted, but due to 523 server error, I do not collect it.
* **Challenges**: How to download the single csv file from github elegantly. Then I found that the bash command `wget` can directly download data. But another question is that, if you directly use `wget` to download the github page, you will get a `.html` file. 
  Therefore the key point to solve the problem is how to find the url that really contains the csv. In that case I use Chrome developer mode and find the url pointing at the csv file.

### 4. What kind of analyses or visualizations did you do?

#### 4.1 World total case

![world_cases](https://github.com/xxachangxx/DSCI_510_Final/blob/master/result/world_cases.png?raw=true)

This figure describes the world new cases every day, and the total cases every day from 2020-01-22 to 2022-11-29.

The subplot one has 2 lines, the blue one is the original data, because it swings too much, I conduct a rolling window to smooth it. The window is 20 days long. The purple line is the smoothed one.

There are 5 orange region in the plot. It represents 5 spike during these 3 years:

* 2020.10.28 -- 2021.02.05
* 2021.03.17 -- 2021.06.05

- 2021.07.05 -- 2021.09.23
- 2021.12.17 -- 2022.04.26
- 2022.06.15 -- 2022.08.29

The 5 period will be useful for folloing analysis

So what happened in this 5 spike? I refer to the [CDC museum website](https://www.cdc.gov/museum/timeline/covid19.html), [devex.com](https://www.devex.com/news/covid-19-in-2021-a-timeline-of-the-coronavirus-outbreak-102417) and [Wiki](https://en.wikipedia.org/wiki/Timeline_of_the_COVID-19_pandemic). They all give some information of the timeline of the COVID-19:

* Spike 1: 
  * One day after the presidential election, the U.S. reports 100,000 new cases of COVID-19 in 24 hours.(November 4, 2020)
  * Two weeks after large groups gathered for Halloween celebrations, COVID-19 case numbers spike across the U.S.(November 13, 2020)
  * As COVID-19 case numbers in the U.S. surge past 11 million, CDC recommends that Americans stay home for Thanksgiving and avoid contact with all people not living in their household for the last 14 days.(November 20, 2020)
* Spike 2:
  * **April 21 —** As [India battles a deadly second wave](https://www.devex.com/news/no-light-at-the-end-of-the-tunnel-as-india-battles-second-wave-of-covid-19-99719), it records 315,735 new infections nationwide — the **highest number of cases reported** in a single day in any country since the beginning of the pandemic.
* Spike 3:
  * Amid a **Delta** variant surge, CDC releases updated masking guidance recommending that everyone in areas with substantial or high transmission wear a mask indoors.(July 27, 2021)
* Spike 4:
  * WHO designates the COVID-19 “**Omicron**” variant, first identified by scientists in South Africa, as a “variant of concern.” Changes in the spike protein of the Omicron variant of the SARS-CoV-2 virus, concern scientists around the world due to the potential for increased transmissibility and decreased vaccine protection.(November 26, 2021)
  * CDC releases data estimating that the Omicron variant is around 1.6 times more transmissible than the Delta variant.(December 20, 2021)
  * The U.S. reports nearly 1 million new COVID-19 infections– the highest daily total of any country in the world. The number of hospitalized COVID-19 patients has risen nearly 50% in just one week.( January 3, 2022)
  * 
    France reports nearly half a million new cases, a record increase; Italy records 228,179 daily infections – as it happened (January 19, 2022)
* Spike 5:
  * As COVID-19 case numbers rise across the U.S. due to the highly transmissible omicron subvariants BA.4 and BA.5., FDA calls for Omicron-specific updates to COVID-19 vaccine boosters from Pfizer-BioNTech and Moderna in fall 2022 (June 30, 2022)
  * Japan reported 253,265 new COVID-19 cases Saturday, marking the third straight day of over 250,000 amid a seventh wave of infections that appears to have no end in sight.(Aug 20, 2022)

#### 4.2 Continent cases

![world_cases](https://github.com/xxachangxx/DSCI_510_Final/blob/master/result/continent_cases.png?raw=true)

This plot shows how total cases and new cases changes in each continents. The 5 orange region have the same meaning as mentioned above.

The first subplot depict total case for each continent; the second depict new cases for each continent; the third depict the percentage of continent total cases to world total cases; and the final subplot depict the percentage of continent new cases to world new cases.

From these plots we can find that:

* Not all continent's total cases and new cases increased sharply in the during the 5 spike period.
* For the first spike, Europe and North America contribute the most new cases
* For the second spike, the main new cases source is Asia
* For the third, Asia, North America and Europe produce the majority of the new cases
* For the fourth, Europe and Asia
* For the fifth, firstly Europe and then Asia. Two spike from the two continent create the fifth spike

#### 4.3 Top 10 countries in the 5 spike

The granularity of continent is still too big to analyze. We need to zoom in to see what is the behavior of each country.

The code generate top 10 country which has the most new cases during the 5 spike respectively

```
Period 1: 2020-10-28 to 2021-02-05

location
United States     18040099
Brazil             3959621
United Kingdom     2974884
India              2812269
Russia             2337688
France             2099203
Italy              2032668
Africa             1891250
Germany            1802726
Spain              1796687

location
United States     0.299
Brazil            0.066
United Kingdom    0.049
India             0.047
Russia            0.039
France            0.035
Italy             0.034
Africa            0.031
Germany           0.030
Spain             0.03

--------------------------------------------------
Period 2: 2021-03-17 to 2021-06-05

location
India            17256145
Brazil            5231494
United States     3797819
Turkey            2364826
France            1942462
Argentina         1705276
Colombia          1208446
Iran              1190996
Germany           1114304
Italy              968949

India            0.330
Brazil           0.100
United States    0.073
Turkey           0.045
France           0.037
Argentina        0.033
Colombia         0.023
Iran             0.023
Germany          0.021
Italy            0.019

--------------------------------------------------
Period 3: 2021-07-05 to 2021-09-23

location
United States     8782634
India             2978192
United Kingdom    2626669
Africa            2530721
Brazil            2401925
Iran              2222411
Indonesia         1914594
Russia            1683340
Turkey            1487667
Malaysia          1364272
dtype: int64

United States     0.190
India             0.064
United Kingdom    0.057
Africa            0.055
Brazil            0.052
Iran              0.048
Indonesia         0.041
Russia            0.036
Turkey            0.032
Malaysia          0.030

--------------------------------------------------
Period 4: 2021-12-17 to 2022-04-26

location
United States     30611006
France            19866220
Germany           17667043
South Korea       16458314
Italy             10853159
United Kingdom    10032976
Vietnam            9078535
India              8334048
Brazil             8146551
Russia             7924846
dtype: int64

United States     0.129
France            0.084
Germany           0.075
South Korea       0.069
Italy             0.046
United Kingdom    0.042
Vietnam           0.038
India             0.035
Brazil            0.034
Russia            0.033


--------------------------------------------------
Period 5: 2022-06-15 to 2022-08-29

location
Japan            9470954
United States    8334193
Germany          5033924
South Korea      4778481
France           4524659
Italy            4102622
Oceania          2847186
Brazil           2827430
Australia        2314146
Taiwan           2218084

Japan            0.149
United States    0.132
Germany          0.079
South Korea      0.075
France           0.071
Italy            0.065
Oceania          0.045
Brazil           0.045
Australia        0.037
Taiwan           0.035

--------------------------------------------------
```

The result is quite obvious:

* For spike 1, the US had the most new cases. The US accouted for about 30% of the total new cases
* For spike 2, India and Brazil had the most new cases. They accouted for about 43% of the total new cases

* For spike 3, the US again had the most new cases. But this time it only accounted for about 19% of the total new cases
* For spike 4, The US, Germany and France had similar new cases, and they in total accounted for about 28% of the total new cases
* For the last spike, Japan and US has nearly the same new cases. They respeactively accounted for 15% and 13% of the total new cases

#### 4.4 What is correlated with Excess Mortality

The last question I am interested in is that what is related to the excess mortality. Excess mortality can be considered as the severity of the COVID-19 in a country. If we can find parameters that is highly related to the excess mortality, we can have more preparation and issues more targeted policies.

In total 3 parameters was tested using scipy.stat: stringency_index (composite measure based on 9 response indicators including school closures, workplace closures, and travel bans, rescaled to a value from 0 to 100), hospital bed per thousand and human development index. And here is the result:

```
The r for excess mortality and stringency_index is 0.14, with p value = 0.136, which is not significant
The r for excess mortality and hospital bed per thousand is -0.24, with p value = 0.008, which is significant
The r for excess mortality and human development index is -0.38,with p value < .001, which is significant
```

It turns out that hospital bed per thousand and human development index have the significant relationship with the excess mortality. With more hospital bed and higher human development index, the excess mortality will decrease significantly. Though the stringency index is not significant, it still has a positive relationship with excess mortality, which means that with stringent policy, excess mortality can be reduced to some extent

### 5 Future Work

* More correlation test could be done, there could be other parameters that are related to excess mortality.
* Give more detailed analysis about the feature of different spike: 