---
title: NCCS Core Series (Harmonized)
date: 2024-02-01
description: A comprehensive panel of nonprofit organizations that file IRS form 990
categories:
  - 990
  - financial-trends
  - nonprofits
featured: true
featuredOrder: 1
primaryCtaUrl: "../../catalogs/catalog-core_harmonized.html"
primaryCtaText: Data Catalog
primaryCtaCaption:
author:
- id: jlecy
citation: 
  author: "Jesse Lecy"
  citationDate: "2024"
  container-title: "NCCS Core Series"
---

## Overview

The Core Data Series is a comprehensive dataset that provides essential information on nonprofit operations spanning the past 30 years. It serves as a valuable resource for researchers, policymakers, funders, and anyone interested in understanding nonprofit sector dynamics in the United States.

The NCCS Core Data Series is derived from nonprofits’ annual Form 990 filings with the Internal Revenue Service (IRS). These data include financial information, governance details, and other organizational characteristics. The key components include the following:

* **Financial data**: includes revenue, expenses, assets, and liabilities, enabling financial analysis and benchmarking  
* **Programs and activities**: descriptions of the organization's activities, programs, and mission statements  
* **Geographic information**: the location and service areas of nonprofits  
* **Time series data**: offer data from 1989 onward, which makes it possible to track trends in the nonprofit sector

The harmonized version of the Core Data Series has been updated with the following features:

* Standardized variable names and data types across all data sets
* Updated data dictionaries that cover all data sets
* A new ID column (**RTRN_ID**) that maps each row to a unique filing ID
* A column (**DUP_RTRN_X**) that indicates the presence of multiple filings by the same EIN
* A column (**TAX_YEAR**) that indicates the tax year for each filing

This makes it easier for users to construct panel data sets. A description of these 
variables is provided in the accompanying data dictionaries.

## Versions

The data is separated into two populations: 

* 501c3 Charities (990 + 990EZ filers)
* All other 501c Nonprofits (990 + 990EZ filers) 

And two scopes that differ by organizational size (smaller nonprofits file Form 990EZ):

* PZ scope: all 990 + 990EZ filers - a larger number of organizations but smaller number of variables 
* PC scope: only includes full Form 990 filers - a smaller number of organizations but a larger number of variables

The PZ version includes approximately 150 variables from 400,000 nonprofits. The PC version includes about 300 variables from 200,000 nonprofits. These numbers vary over time. Taken together, the combination
of population and scope distinctions creates 4 separate data sets:

* 501C3-CHARITIES-PZ: Contains variables found in both 990 and 990EZ filings for 501(c)(3) organizations
* 501C3-CHARITIES-PC: Contains variables found only in 990 filings for 501(c)(3) organizations
* 501CE-NONPROFIT-PZ: Contains variables found in both 990 and 990EZ filings for all other 501(c) organizations
* 501CE-NONPROFIT-PC: Contains variables found only in 990 filings for all other 501(c) organizations

Data for private foundations is undergoing harmonization and will be released at a later date. 

## Use

Data can be downloaded via the [**data catalog**](https://urbaninstitute.github.io/nccs/catalogs/catalog-core_harmonized.html) page.

## Data Series Attributes 

The dataset covers a wide range of nonprofit organizations, including charities, foundations, religious organizations, educational institutions, and more. It encompasses organizations of various sizes, missions, and locations across the United States.

The NCCS Core Data Series is widely used by researchers, policymakers, and nonprofits themselves to gain insights into the sector. It can inform studies on topics such as nonprofit financial health, sector trends, and the effects of policy changes on charitable organizations.NCCS takes measures to ensure data quality and accuracy, making it a reliable resource for research and analysis.

## Sample Scope Definitions

All 990 filers are split into five groups using combinations of two variables:

* **organizational type scope or tscope** This variable distinguishes between 501c3 public charities, 501c3 private foundations, and all other types of 501c organizations. 
* **Form filing scope or fscope**: This variable describes which types of filers are included in the dataset. 

![image](https://github.com/lecy/nccs/assets/1209099/8a2d94ca-346a-4679-b30e-f3328a7d0df9)

**Other nonprofits**: For other nonprofits, the situation is a little trickier. They are divided into two categories: 

*	**501(c)(3) public charities**: These organizations are distinct because all donations made to them are tax deductible. Datasets containing 501(c)(3) organizations are labeled 501C3-SCOPE-CHARITIES. 
*	**Other organizations with tax-exempt types**: This category includes tax-exempt types 501(c)(1) to 501(c)(92), excluding the 501(c)(3) charities. These are labeled as 501CE-SCOPE-NONPROFIT, with 501cE standing for "everything other than 501(c)(3).”  

**Form Scope:**

The Core Data Series contains two types of form scope: 

* All full 990 filers (fscope=**PC**)  
* The broader 990 + 990EZ filers (fscope=**PZ**)

![image](https://github.com/lecy/nccs/assets/1209099/cf809446-da58-4867-9870-b0035a942847)
 
**Data Set Naming Conventions**

The data sets in the catalog have the following naming conventions:

![image](http://127.0.0.1:4000/nccs/public/img/nccs-core-harmonized-naming-conventions.jpg)

For example, CORE-2015-501C3-CHARITIES-PC.csv contains variables from Form 990 filings (**PC**) for 501(c)(3) organizations (**501C3-CHARITIES**) in tax year 2015.

**Form Scope PZ**: The datasets with form scope of PZ will contain the full set of 990 and 990-EZ tax filers for a given year, but the trade-off is that 990-EZ filers have a much smaller form and thus fewer variables. Datasets with PZ scope represent a more extensive population and a more limited selection of fields. 

**Form Scope PC**: Datasets with form scope of PC will contain the smaller subset of full 990 form filers but contain a larger number of variables. 

The PZ version of the data series is available for tax years 1989-2019. The PC version is available for tax years 2012-2019.

See the [Data Guide](https://nccs-data.urban.org/NCCS-data-guide.pdf) for more details. 

**Exclusion of 990N ePostcard Filers:**

The Core Series does not include 990N ePostcard filers, even though they are the most common type of nonprofit. These organizations have annual revenues below $50,000, and the 990N form they file includes minimal information, including updates to the organization address and key contact.

While these organizations may have limited financial resources, they play a significant role in the voluntary sector and civil society. For example, many volunteer-run nonprofits like parent teacher associations, block associations, and little league clubs fall in this group. They actively build social capital in communities and provide important collective actions that can be accomplished with very modest financial support. 

This is an important consideration when designing a sampling framework for a study. The Core Series will not capture the activities of these small organizations. If these organizations are relevant to your study, consider incorporating information from the [990N ePostcard Dataset](https://urbaninstitute.github.io/nccs/datasets/postcard/) or 1023-EZ data on nonprofit startups.

## Definition of "Year" in File Names

The new NCCS Core series has been reorganized by the tax year, or the closest approximation we can get to the period of performance described by the 990 data. Time gets complicated because nonprofits can select their own accounting periods with fiscal years that can end in any month. As a result, the tax year does not align perfectly with the calendar year. For example, tax year 2018 can include activities from some months in 2018 and some months in 2019. But this new approach approximates the true period of nonprofit performance much more closely than organizing panels by filing dates.

<br>
<br>
<br>
<br>










