# Analysis-of-US-Gun-Sales



Analyzing FBI NICS Firearm Background Checks
## Table of contents
1. [I. Objective](#introduction)
2. [II. Data Overview](#overview)
3. [V. Conclusions](#conclusions) <br/>
    1. [Research Question 1](#ans1)<br/>
    2. [Research Question 2](#ans2)<br/>
    3. [Research Question 3](#ans3)<br/>
    4. [Research Question 4](#ans4)<br/>
    5. [Research Question 5](#ans5)<br/>
    6. [Research Question 6](#ans6)<br/>
    7. [Research Question 7](#ans7)<br/>
    8. [Research Question 8](#ans8)<br/>
4. [VI. Data Limitations](#limitations) <br/>


1
<a name="objective"></a>
2
## I. Objective 
3
1. Which state in America has the highest and lowest estimated guns per capita?
4
2. Which popultation statistic is has the highest correlation with estimated guns per capita in America?
5
3. Which state in America has reported the highest rate of growth in estimated gun sales?
6
4. What trends can be observed in estimated gun sales over time in America?
7
5. What is the seasonal (monthly) trend in estimated gun sales?

II. Data Overview
Two data sets were used in this analysis:

FBI's National Instant Criminal Background Check System:
This data comes from the FBI's National Instant Criminal Background Check System.

According to FBI.gov:

Mandated by the Brady Handgun Violence Prevention Act of 1993 and launched by the FBI on November 30, 1998, NICS is used by Federal Firearms Licensees (FFLs) to instantly determine whether a prospective buyer is eligible to buy firearms. Before ringing up the sale, cashiers call in a check to the FBI or to other designated agencies to ensure that each customer does not have a criminal record or isn’t otherwise ineligible to make a purchase. More than 230 million such checks have been made, leading to more than 1.3 million denials.

The background check data base records the number of background checks conducted per month in each of the 50 U.S. states and five U.S. territories from November 1998 - March 2017. Background checks relating to different transactions (purchase, rental, resale, pawning, etc.) and different gun types (hand gun, long gun, etc) are distinctly recorded.

The original dataset has footnotes among which include:

These statistics represent the number of firearm background checks initiated through the NICS They do not represent the number of firearms sold Based on varying state laws and purchase scenarios, a one-to-one correlation cannot be made between a firearm background check and a firearm sale.

U.S. Census Data
This data comes from the US Census Bureau. It contains demographic statistics relating to economy, housing, race,etc. for each of the 50 states. Most variables just have one data point per state (2016), but a few have data for more than one year (e.g. for 2010).

III. Data Wrangling
a. Data Acquisition

Preliminary Observations:
The data ranges from 1998 to 2017 and observations are documented on a monthly basis. There are data points for each month except for the starting (1998) and ending (2017) years. These two years will therefore be excluded from the analysis to ensure consistent number of months per year in NICs data.
NICs data records background checks and is a proxy for sales.
According to the [Small Arms Survey]
"The magnitude of the demand for firearms in the United States can be approximated if one is willing to make two assumptions: firstly, that all permit checks are routine procedural checks by states against FBI records and are not associated with an intent to purchase a gun; and, secondly, that all in-store (retailer) checks by licensed firearms dealers against FBI records result in at least one firearms purchase.

As approximations go, one may then add ‘handgun’ checks, plus ‘long gun’ checks, plus two ‘multiple’ checks (at least one handgun and one long gun), and augment the resulting number by a factor of 1.1, termed here the multiple gun sales factor (MGSF)."

Accordingly, only the columns hand gun, long gun and multiple (gun) will be used in the analysis. Estimated total gun sales will be calculated as follows:

𝐸𝑠𝑡𝑖𝑚𝑎𝑡𝑒𝑑 𝑇𝑜𝑡𝑎𝑙 𝑆𝑎𝑙𝑒𝑠=[(𝐻𝑎𝑛𝑑 𝐺𝑢𝑛+𝐿𝑜𝑛𝑔 𝐺𝑢𝑛)+(𝑀𝑢𝑙𝑡𝑖𝑝𝑙𝑒∗2)]∗1.1


IV. Data Analysis

Research Question 1
What is the distrubtion of the interest in the three gun types as of 2016 across the 50 states in the United States?
  

Research Question 2
What is the population distrubtion across the United States as of 2016?


Research Question 3
What is the estimated gun sales distrubtion across the United States as of 2016?

      

Research Question 4
Which states had the highest and lowest estimated guns per capita?

'
ac16=abs(gc16[gc16.columns[1:]].corr()['guns_per_capita'][:-1])
8
correlations_16=pd.DataFrame({'corr': c16, 'abs_corr': ac16})
1
correlations_16.loc[correlations_16['abs_corr'].idxmax()]
corr       -0.618576
abs_corr    0.618576
Name: Foreign born persons, percent, 2011-2015, dtype: float64
d) Correlation Plots

1
#census variable with the highest correlation to guns per capita in 2010 
2
fig, ax = py.subplots(figsize = (9,5))
3
sns.regplot(gc10_1['Foreign born persons, percent, 2011-2015'],gc10_1['guns_per_capita'])
4
py.title('Correlation between Estimated Guns per Capita and Percent of Foreigners')
5
py.xlabel('Percent of Foreigners [2011-2015]')
6
py.ylabel('Estimated Guns per Capita 2010')
7
py.savefig('correlation_1.png')
8
​

1
#2010 census variable with the highest correlation to guns per capita in 2010 
2
fig, ax = py.subplots(figsize = (9,5))
3
sns.regplot(gc10_2['Population per square mile, 2010'],gc10_2['guns_per_capita'])
4
py.title('Correlation between Estimated Guns per Capita and Percent of Foreigners')
5
py.xlabel('Population per Square Mile 2010')
6
py.ylabel('Estimated Guns per Capita 2010')
7
py.savefig('correlation_2.png')
8
​

1
#census variable with the highest correlation to guns per capita in 2016 
2
fig, ax = py.subplots(figsize = (9,5))
3
sns.regplot(gc16['Foreign born persons, percent, 2011-2015'],gc16['guns_per_capita'])
4
py.title('Correlation between Estimated Guns per Capita and Percent of Foreigners')
5
py.xlabel('Percent of Foreigners [2011-2015]')
6
py.ylabel('Estimated Guns per Capita 2016')
7
py.savefig('correlation_3.png')


Research Question 6
Which states at the highest rate of growth in estimated gun sales?

Methodology:
Compound Annual Growth Rate will be used to measure growth in estimated gun sales between 1999 and 2016 Compound Annual Growth Rate was chosen because it dampens the effect of volatility of periodic sales.
𝐶𝐴𝐺𝑅=(𝐿𝑎𝑠𝑡𝑌𝑒𝑎𝑟𝑆𝑎𝑙𝑒𝑠𝐹𝑖𝑟𝑠𝑡𝑌𝑒𝑎𝑟𝑆𝑎𝑙𝑒𝑠)1/𝑛−1
where 𝑛 is the number of years
1
#extract gun sales for the first and last year for each state
2
first_year=gun_trimmed[gun_trimmed['year']==1999].loc[:,['state','sales_estimate','year_month']]
3
first_year=first_year.groupby(['state']).sum()['sales_estimate']
4
last_year=gun_trimmed[gun_trimmed['year']==2016].loc[:,['state','sales_estimate','year_month']]
5
last_year=last_year.groupby(['state']).sum()['sales_estimate']
6
​
7
​
8
#calculate CAGR for each state
9
growths=pd.DataFrame({'first_year':first_year,'last_year':last_year,'ratio':last_year/first_year})
10
growths['ratio']=((growths['last_year']/growths['first_year'])**(1/18))-1
11
growths=growths.replace(np.inf,np.nan)
12
​
13
#identify states with the five highest CAGRs 
14
growths.sort_values(by='ratio',ascending=False).head(5)
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-1-0cc839eb6c62> in <module>
      1 #extract gun sales for the first and last year for each state
----> 2 first_year=gun_trimmed[gun_trimmed['year']==1999].loc[:,['state','sales_estimate','year_month']]
      3 first_year=first_year.groupby(['state']).sum()['sales_estimate']
      4 last_year=gun_trimmed[gun_trimmed['year']==2016].loc[:,['state','sales_estimate','year_month']]
      5 last_year=last_year.groupby(['state']).sum()['sales_estimate']

NameError: name 'gun_trimmed' is not defined


Research Question 7
What are trends in estimated gun sales over time?

Methodology:
Aggregate the estimated sales across each state to show the overall trend in estimated gun sales between 1999 and 2016 in America.
Adjust for seasonality by using the rolling method whch splits the data into windows of time (in this case each window will be a period of 12 months). The data in each window is then aggregated calculating the mean. The windows overlap and 'roll' at the same frequency as the data so the transformed time series is at the same frequency as the original time series. Adjusting for seasonality smooths out periodic fluctuations in the data to observe long term trends.
1
import matplotlib.dates as mdates
2
sales_by_year=gun_trimmed.groupby(['year_month']).sum()['sales_estimate'].astype('int64')
3
sales_by_year=pd.DataFrame({'year_month':sales_by_year.index,
4
                            'sales':sales_by_year}).reset_index(drop=True)
5
​
1
sales_by_year['seasonally_adjusted']=sales_by_year['sales'].rolling(window=12, center=True, min_periods=12).mean().fillna(0).astype("int32")
2
sales_by_year['year_month']=pd.to_datetime(sales_by_year['year_month'])
3
​
1
#Unadjusted for seasonality
2
​
3
fig, ax = py.subplots(figsize = (16,10))
4
sns.lineplot(x="year_month", y="sales", data=sales_by_year)
5
py.title('Estimated Gun Sales in US States and Territories 1999-2016')
6
py.xlabel('Year')
7
py.ylabel('Estimated Gun Sales')
8
py.savefig("trend_unadjusted.png")
9
​

1
#Seasonally adjusted
2
​
3
fig, ax = py.subplots(figsize = (16,10))
4
sales_by_year=sales_by_year[sales_by_year["year_month"]>'1999-12-01']
5
sales_by_year=sales_by_year[sales_by_year["year_month"]<'2016-01-01']
6
sns.lineplot(x="year_month", y="seasonally_adjusted", data=sales_by_year)
7
py.title('Estimated Gun Sales in US States and Territories 1999-2016')
8
py.xlabel('Year')
9
py.ylabel('Estimated Gun Sales')
10
py.savefig("seasonally_adjusted.png")
11
​


Research Question 8:
What is the seasonal (monthly) trend in estimated gun sales?

Methodology:
Group the estimated gun sales by month and sum across each state and year to capture the change in estimated gun sale s across a year.
1
gun_trimmed.rename(columns={'month':'month_date'},inplace=True)
2
gun_trimmed['month']=gun_trimmed['month_date'].apply(lambda x: x[-2:])
3
In [555]:
4
​
  File "<ipython-input-226-1a3356220f54>", line 3
    In [555]:
             ^
SyntaxError: invalid syntax


1
sales_month=gun_trimmed.groupby(['month']).sum()['sales_estimate'].reset_index()
2
​
3
​
4
sales_month=pd.DataFrame({'sales':sales_month['sales_estimate'],
5
                               'month_int':sales_month['month']
6
                              })
7
​
8
sales_month["month_name"]=pd.to_datetime(sales_month['month_int'], format='%m').dt.month_name().str.slice(stop=3)
9
py.figure(figsize=(16, 10))
10
sns.lineplot(x="month_name", y="sales", data=sales_month,sort=False).set(xticks=sales_month.month_name.values);
11
py.title('Estimated Gun Sales by Month between 1998-2016')
12
py.ylabel('Estimtaed Gun Sales in Millions')
13
py.xlabel('Month')
14
py.savefig("monthly_trend.png")
15
​

V. Conclusions
1. What is the distrubtion of the interest in the three gun types as of 2016 across the 50 states in the United States?

This cursory outlook indicates that the gun sales landscape is dominated by California and Texas. This leads to a potential hypothesis that state population plays a significant role in estimated gun sales in comparison to factors like political affiliation (red vs blue) as Texas and California have the highest state population but opposing political associations.



2. What is the population distrubtion across the United States as of 2016? 

3. What is the estimated gun sales distrubtion across the United States as of 2016?

The distribution of estimated gun sales in 2016 is skewed to the right. The concentration of estimated gun sales appears to collect around the lower end of the distribution.



4. Which state in America has the highest and lowest estimated guns per capita?

Year	Highest Guns per Capita	Lowest Guns Per Capita
2010	Alaska (0.0959)	New Jersey (0.0064)
2016	Alaska (0.1171)	Iowa (0.0134)
a) Alaska has the highest gun per capita in both 2010 and 2016. Moreover, the gun per capita increased from 0.0959 in 2010 to 0.1171 in 2016. 

b) New Jersey had the lowest gun per capita in 2010 of 0.0064. 

c) Iowa had the lowest gun per capita in 2016 of 0.0134. 

Again, the lowest gun per capita value rose between 2010 and 2016. 

Note - Hawaii had an estimated gun per capita of 0 in both 2010 and 2016 because all its background checks were conducted as permit checks or rechecks. As a result this state was excluded from this part of the analysis because permit checks were not incorporated in the estimation of guns per capita.

5. Which popultation statistic is has the highest correlation with estimated guns per capita in America? 


a) Percent of Foreigners 2011-2015 has the highest (negative) correlation of -0.6190 to estimated guns per capita in 2010 from the whole census data set.



b) Population per Square Mile 2010 has the highest (negative) correlation of -0.5569 to estimated guns per capita in 2010 from subset of census data relating to 2010.



c) Percent of Foreigners 2011-2015 has the highest (negative) correlation of -0.6186 to estimated guns per capita in 2016 from the whole census data set.



6. Which state in America has reported the highest rate of growth in estimated gun sales?

a) Districut of Columbia is the U.S. territory with the highest growth (per CAGR) of estimated gun sales at 21.87%. 

b) Massachusetts is the U.S. state with the highest growth (per CAGR) of estimated gun sales at 10.71%.

7. What trends can be observed in estimated gun sales over time in America?

There is an exponential increase in the estimated gun sales over time in America between 1998-2016. Major points of inflection include:


2001: The year of the 9/11 attacks.
2009: The first year President Obamas took office.
2013: The year of President Obama's call for stricter gun laws after a mass shooting at Sandy Hook Elementary School at the end of 2012.**

** Source: New York Times

Trend in Estimated Gun Sales [1999-2016]


Seasonally Adjusted Trend in Estimated Gun Sales [1999-2016]


5. What is the seasonal (monthly) trend in estimated gun sales?

There is a significant spike in the estimated gun sales in the month of December. It should be noted that the events listed above in (4) all occured around December.




VI. Limitations
1.The NICs data records background checks mandated for the purchase of a firearm by licensed dealers. It is only a proxy for gun sales and these background checks fail to capture exchange of guns not conducted through a licensed dealer.

2. Census data are often estimates and largely directional rather than accurate due to failure to complete census forms or fabrication while filling out the form. This impacts metrics like Estimated Gun Sales per capita.

3. The research questions identified various correlations but it is important to bare in mind, albeit cliched, that correlation does not imply causation.


<a name="overview"></a>
## II. Data Overview
Two data sets were used in this analysis:
1. **FBI's National Instant Criminal Background Check System:**<br/>
This data comes from the FBI's National Instant Criminal Background Check System.

  According to [FBI.gov](https://www.fbi.gov/services/cjis/nics):

> Mandated by the Brady Handgun Violence Prevention Act of 1993 and launched by the FBI on November 30, 1998, NICS is used by Federal Firearms Licensees (FFLs) to instantly determine whether a prospective buyer is eligible to buy firearms. Before ringing up the sale, cashiers call in a check to the FBI or to other designated agencies to ensure that each customer does not have a criminal record or isn’t otherwise ineligible to make a purchase. More than 230 million such checks have been made, leading to more than 1.3 million denials.

The background check data base records the number of background checks conducted *per month* in each of the 50 U.S. states and five U.S. territories from *November 1998 - March 2017*. Background checks relating to different transactions (purchase, rental, resale, pawning, etc.) and different gun types (hand gun, long gun, etc) are distinctly recorded.

The original dataset has footnotes among which include:
>These statistics represent the number of firearm background checks initiated through the NICS They **do not** represent the number of firearms sold Based on varying state laws and purchase scenarios, a one-to-one correlation cannot be made between a firearm background check and a firearm sale.

2. **U.S. Census Data**<br/>
This data comes from the US Census Bureau. It contains demographic statistics relating to economy, housing, race,etc. for each of the 50 states. Most variables just have one data point per state (2016), but a few have data for more than one year (e.g. for 2010).







[![](https://img.shields.io/badge/linkedin-@Shilin_Li-orange.svg?colorA=abcdef&logo=data:image/jpeg;base64,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)](https://www.linkedin.com/in/shilin-li-40474777/)


### Introduction


The data comes from the FBI's National Instant Criminal Background Check System. The NICS is used by to determine whether a prospective buyer is eligible to buy firearms or explosives. Gun shops call into this system to ensure that each customer does not have a criminal record or isn’t otherwise ineligible to make a purchase. The data has been supplemented with state level data from [census.gov](https://www.census.gov).

![alt text](https://images.pexels.com/photos/1260563/pexels-photo-1260563.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940)


(Image is from a copyright-free website: https://www.pexels.com/royalty-free-images/.)


- The [NICS data](https://d17h27t6h515a5.cloudfront.net/topher/2017/November/5a0a4db8_gun-data/gun-data.xlsx) is found in one sheet of an .xlsx file. It contains the number of firearm checks by month, state, and type.
- The [U.S. census data]( https://d17h27t6h515a5.cloudfront.net/topher/2017/November/5a0a554c_u.s.-census-data/u.s.-census-data.csv) is found in a .csv file. It contains several variables at the state level. Most variables just have one data point per state (2016), but a few have data for more than one year.


| Table of Contents |
| :--- |
| <a href='#Prerequisites'> Prerequisites </a> 🔍📜 | 
| <a href='#Design'> Design </a>  📐 |
| <a href='#Conclusions'> Conclusions </a>  📌 |
| <a href='#License'> License </a> 🔖 |


<a id='Prerequisites'></a>

### Prerequisites


![](https://img.shields.io/badge/platform-ios-green.svg?colorA=abcdef)


- Python 3.6.3
- Jupyter Notebook
- Anaconda-Navigator


<a id='Design'></a>

### Design


![](https://img.shields.io/badge/language-Python-orange.svg)


**_Step One_** - Choose Data Set


Click this [link](https://docs.google.com/document/d/e/2PACX-1vTlVmknRRnfy_4eTrjw5hYGaiQim5ctr9naaRd4V9du2B5bxpd8FEH3KtDgp8qVekw7Cj1GLk1IXdZi/pub?embedded=True) to download the corresponding data.

**_Step Two_** - Get Organized


This project eventually contain:


- The report communicating any findings;
- Any Python code used during the analysis;
- The data set;


**_Step Three_** - Analyze


Brainstorm some questions that could be answered using the data set, then start answering those questions, we would mainly focus on looking at the relationships between multiple variables. 


<a id='Conclusions'></a>

### Conclusions


In current study, a good amount of profound analysis has been carried out. Prior to each step, deailed instructions was given and interpretions was also provided afterwards. The dataset included 2 tables, but they have to be loaded by different measures. The data was ranging from 1998 to 2017, which consisted of detailed information of registered gun. Based on such substantial data, the analysis would be more reliable as opposed to small scale analysis.
The limitations of current study were obvious as well, data was seperated into two tables which could affect the process of analysis. On the other hand, the population estimation were only recorded for 2010 and 2016, which limit some analysis to a small range, same for many other parameters, such as "Foreign born persons, percent", "Veterans, 2011-2015", etc.


<a id='License'></a>

## License 


[![MIT Licence](https://badges.frapsoft.com/os/mit/mit.svg?v=103)](https://github.com/Shilin0806/Investigate_the_FBI_Gun_Dataset/blob/master/LICENSE)
