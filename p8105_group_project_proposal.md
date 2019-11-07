P8105 Data Science - Group Project Proposal
================

  - [Group Members and UNIs](#group-members-and-unis)
  - [Project Title](#project-title)
  - [Motivation for the Project](#motivation-for-the-project)
  - [Intended Final Products](#intended-final-products)
  - [Anticipated Data Source(s)](#anticipated-data-sources)
  - [Planned Analyses/Visualizations/Coding
    Challenges](#planned-analysesvisualizationscoding-challenges)
  - [Planned Timeline](#planned-timeline)

#### Group Members and UNIs

*In alphabetical order:*

1.  Jon Brock (JPB2210)  
2.  Ava Hamilton (AH3108)  
3.  Steven Lawrence (SL4269)  
4.  Molly Martorella (MEM2331)  
5.  Stephen Powers (SP3694)

-----

#### Project Title

“The effects of gentrification on community conflict and housing
conditions”

-----

#### Motivation for the Project

Gentrification is a growing problem across NYC and the US, leading to
rent increases, the displacement of communities, and community conflict.
We want to assess how changes in the proportion of different racial and
ethnic populations relates to changes in housing conditions. We also
want to investigate community conflicts by looking at the rate and type
of 311 complaints as the population in an area is changing. We
hypothesize that a shift towards white/european populations will result
in improved housing conditions, and that changes in population
proportions will lead to a increase in 311 complaints.

-----

#### Intended Final Products

1.  Describe the distribution of 311 complaints with respect to
    differences in housing and sub-populations.
2.  Time series analysis of living conditions (lead, plumbing, water,
    elevator, infestations, etc.) as it relates to underlying
    populations across NYC.
3.  Time-series analysis of 311 complaints with respect to changing
    population proportions.

-----

#### Anticipated Data Source(s)

[NYC 311 Complaints
Data](https://data.cityofnewyork.us/d/erm2-nwe9/visualization)  
[NYC Housing Authority
Data](https://data.cityofnewyork.us/Housing-Development/NYCHA-Development-Data-Book/evjd-dqpz)  
[US Census Bureau
Data](https://www.census.gov/data/tables/time-series/demo/housing-patterns/housing-patterns-tables.html)  
[NYC Population
Data](https://www1.nyc.gov/site/planning/planning-level/nyc-population/nyc-population.page)  
[Immigration and Risk of Children Lead Poisoning: Case-Control
Study](https://ajph.aphapublications.org/doi/full/10.2105/AJPH.2006.093229)

Census api for r; population estimates. 

-----

#### Planned Analyses/Visualizations/Coding Challenges

##### *Analyses*

1.  Time Series Analyses: We will use census data to calculate
    proportions of different racial and ethnic populations across the 5
    boroughs of NYC for the past 10-20 years. We will then look at the
    incidence and type of housing and 311 complaints across the same
    time period. We will use the autocorrelation function to assess seasonality
    and use spearman rank sum test to identify the presence of trend in the number
    of 311 rate over the time period 10-20 years. 
2.  Targeted analyses: We will identify key population shifts in the
    data and perform focused statistical analyses to test our
    hypotheses.

##### *Visualizations*

1.  NYC Maps - a dashboard where the year can be selected and the
    population, 311 rate, or housing condition can be selected for.  
2.  The rate of each type of housing issue within each population within
    each borough.  
3.  Before/after plot of population shift and how that affected living
    conditions and 311 complaints (will look at both type and rate).
4.  Seasonal plot of 311 rate 

##### *Coding Challenges*

**INSERT INFO HERE**
1.  

-----

#### Planned Timeline

  - Week 1 (11/04/19 - 11/10/19):
      - (11/07/19 by 13:00) Form a team and submit this proposal
  - Week 2 (11/11/19 - 11/17/19):
      - Project review in-person meeting - No deliverables this week  
      - Have census, housing, and 311 data tidied
  - Week 3 (11/18/19 - 11/24/19):
      - Make backbone of eventual website  
      - Create basic plots showing \[ENTER INFO\]  
      - Plotting of changing populations over time, changing 311
        complaint rates, and of different housing conditions among
        populations and neighborhoods overtime
  - Week 4 (11/25/19 - 12/01/19):
      - Create dashboard that synthesizes timecourse analyses into a
        plotly map  
      - Finalize building of website including other plots and more
        focused plots and analyses of changing population structure
  - Week 5 (12/02/19 - 12/08/19):
      - (12/05/19 by 16:00) Written report giving detailed project
        description due  
      - (12/05/19 by 16:00) Webpage overview of project, with short
        explanatory video (published online) due  
      - (12/05/19 by 20:00) Brief assessment of teammates’ contributions
        (as a short document) due
  - Week 6 (12/10/19): **Final Presentations “In-Class”**

-----
