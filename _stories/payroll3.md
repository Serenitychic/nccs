---
title: Payroll Taxes V3
date: 2023-08-28
description: This data story outlines the process for estimating the amount of payroll taxes nonprofits pay annually.
featured: true
featuredOrder: 1
type: methods
categories:
  - SOI extracts
  - payroll tax
author:
  - id: hmartin
  - id: jlecy
citation: 
  container-title: National Center for Charitable Statistics
  volume: 1
  issue: 1
  doi: 10.5555/12345678
links:
  - header: Replication Files
    links:  
    - text: Data + Code
      href: https://tinyurl.com/ylydvtj9
      icon: download
    - text: Code
      href: https://raw.githubusercontent.com/UrbanInstitute/nccs-recipes/main/vignettes/payroll.R
      icon: link
  - header: In Context 
    links:
    - text: Nonprofit Infrastructure Coalition 
      href: https://independentsector.org/wp-content/uploads/2023/04/nic-agenda-2023.pdf
      icon: download
---

The nonprofit sector must persistently track programs and activities in
order to measure societal benefits and convey impact to key
stakeholders. This research note introduces an uncommon but useful
approach to measuring sector size though payroll tax information
disclosed on IRS form 990s.

Researchers commonly use counts of nonprofits or total program expenses
to measure sector size. Alternatively payroll captures the mobilization
of human resources, a complementary way to convey the depth of
engagement within communities.

Historically, payroll data has been a difficult to acquire. Partnerships
between nonprofit scholars and the Bureau of Labor Statistics have
resulted in new methodologies for using the Quarterly Census of
Employment and Wages to measure the size of the nonprofit workforce, but
QCEW data on nonprofits is currently only updated once every five years.
Payroll tax information on 990 forms is more accessible and timely.

Although nonprofits are exempt from many types of taxes they are still
required to pay federal payroll taxes. These include a 6.2% Social
Security tax and 1.45% for Medicare. Taxes are levied for both part and
full-time employees. The aggregate level of payroll taxes paid by the
sector can be used as a proxy for the number of full-time equivalent
workers employed by the sector.

We estimate that nonprofits currently pay approximately \$66 billion per
year in payroll taxes. See below for details about how we arrive at this
estimate.

### Form 990 Data

We estimate the payroll taxes that the nonprofit sector contributes
annually using 2019 IRS 990 data, the most recent full year of available
data at the time the analysis was conducted.

Three years of IRS 990 data extracts must be utilized to capture a
single full year of data since there can be a 2-year lag in returns,
resulting in an idiosyncratic data aggregation challenge. Form 990 data
is compiled from the 2019, 2020, and 2021 Statistics of Income Extract
files available through the IRS:
<https://www.irs.gov/statistics/soi-tax-stats-annual-extract-of-tax-exempt-organization-financial-data>.

### Sampling Considerations

Data availability varies greatly by the size of the nonprofit. Larger
nonprofits are required to file the full Form 990, which requires
significant disclosures on programs and finances. Medium-sized
nonprofits file a more compact version of the form called the 990EZ, and
small nonprofits are only required to report that they remain active by
filing Form 990N. Private foundations utilize the 990-PF. Payroll
information disclosures vary by the type of form.

***Larger Nonprofits:*** The Form 990 collects data on the amount of
payroll taxes that larger nonprofits (i.e., those with gross receipts
\>= \$200K or total assets \>= \$500K) contribute (**\$55B in 2019**).

***Private foundations:*** The Form 990-PF does not collect data on the
amount of payroll taxes that private foundations contribute However, it
does collect data on the amount that private foundations spend on
salaries and benefits (\$3B). The Form 990 collects this data for larger
nonprofits, as well. Therefore, we can divide the amount of payroll
taxes that larger nonprofits contribute by the amount that larger
nonprofits spend on payroll taxes to calculate a payroll tax/salaries
and benefits ratio (5.75%). We can then apply that ratio to the salaries
and benefits data from the Form 990-PF to estimate the amount of payroll
taxes that private foundations contribute.

However, the IRS’s Form 990-PF data extract only includes the columns
with data on “compensation of officers, directors, trustees, etc.” and
“pension plans, employee benefits;” it excludes the column with data on
“other employee salaries and wages.” Therefore, we will underestimate
the amount of payroll taxes that private foundations contribute.
Additionally, the IRS does not provide a data extract for the 2019 Form
990-PF file. This creates a further underestimation of the amount of
payroll taxes that private foundations contribute (**estimated \$153M in
2019**).

***Smaller Nonprofits:*** The Form 990-EZ does not collect data on the
amount of payroll taxes that smaller nonprofits (i.e., those with gross
receipts \<\$200K and total assets \<\$500K) contribute It does collect
data on the amount that smaller nonprofits spend on salaries and
benefits, but the IRS’s Form 990-EZ data extracts exclude these data.
Therefore, we are unable to estimate the amount of payroll taxes that
smaller nonprofits contribute.

***Invisible Nonprofits:*** Some nonprofits, such as churches, do not
have to file annual returns. See
<https://www.irs.gov/charities-non-profits/annual-exempt-organization-return-who-must-file>.

The Form 990-N also does not collect any financial data from the
smallest nonprofits (i.e., those with gross receipts \<=\$50K), nor does
it collect data on the amount that these nonprofits spend on salaries
and benefits. Therefore, we are unable to estimate the amount of payroll
taxes that the smallest nonprofits contribute.

## Data Vignette

### Packages

You can install R packages as follows:

``` r
install.packages( "knitr" )
install.packages( "tidyverse" )
install.packages( "pander" )
install.packages( "kableExtra" )
```

If you already have these packages, you only need to load the libraries.

``` r
library( tidyverse )  # data wrangling
library( pander )     # pretty formats
library( knitr )      # pretty formats
library( kableExtra ) # html formats for tables 
```

### Data

Load the SOI Extract data obtained from the IRS:

Download all data and read locally:

``` r
URL <- "https://nccsdata.s3.us-east-1.amazonaws.com/replication/payroll/payroll.zip"
download.file( URL, destfile="payroll.zip" )
unzip( "payroll.zip" )

pc.2019 <- readr::read_csv( "payroll/19eoextract990.csv" )
pc.2020 <- readr::read_csv( "payroll/20eoextract990.csv" )
pc.2021 <- readr::read_csv( "payroll/21eoextract990.csv" )
pf.2020 <- readr::read_csv( "payroll/20eoextract990pf.csv" )
pf.2021 <- readr::read_csv( "payroll/21eoextract990pf.csv" )
```

Or read directly from the URLs:

``` r
url.pc.2019 <- "https://nccsdata.s3.us-east-1.amazonaws.com/replication/payroll/19eoextract990.csv"
url.pc.2020 <- "https://nccsdata.s3.us-east-1.amazonaws.com/replication/payroll/20eoextract990.csv"
url.pc.2021 <- "https://nccsdata.s3.us-east-1.amazonaws.com/replication/payroll/21eoextract990.csv"
url.pf.2020 <- "https://nccsdata.s3.us-east-1.amazonaws.com/replication/payroll/20eoextract990pf.csv"
url.pf.2021 <- "https://nccsdata.s3.us-east-1.amazonaws.com/replication/payroll/21eoextract990pf.csv"

pc.2019 <- readr::read_csv( url.pc.2019 )
pc.2020 <- readr::read_csv( url.pc.2020 )
pc.2021 <- readr::read_csv( url.pc.2021 )
pf.2020 <- readr::read_csv( url.pf.2020 )
pf.2021 <- readr::read_csv( url.pf.2021 )
```

Preview the data:

``` r
head( pc.2019[,1:6] ) %>% pander()
```

| elf |    ein    | tax_pd | subseccd | s501c3or4947a1cd | schdbind |
|:---:|:---------:|:------:|:--------:|:----------------:|:--------:|
|  E  | 141733367 | 201806 |    9     |        N         |    N     |
|  E  | 710610420 | 201806 |    6     |        N         |    N     |
|  E  | 231571787 | 201808 |    3     |        Y         |    Y     |
|  E  | 650083457 | 201709 |    3     |        Y         |    N     |
|  E  | 591449455 | 201709 |    6     |        N         |    N     |
|  E  | 411326460 | 201806 |    3     |        Y         |    N     |

The data dictionaries are available in the zipped directory. Additional
information can be obtained at the [SOI Extracts
page](https://www.irs.gov/statistics/soi-tax-stats-annual-extract-of-tax-exempt-organization-financial-data).

## Data Preparation

### Variable Cleanup

Convert six digit tax period dates (YYYYMM) to four digit years (YYYY):

``` r
pc.2021$year <- pc.2021$tax_pd  %>% substr( 1, 4 )
pc.2020$year <- pc.2020$tax_pd  %>% substr( 1, 4 )
pc.2019$year <- pc.2019$tax_pd  %>% substr( 1, 4 )
pf.2020$year <- pf.2020$TAX_PRD %>% substr( 1, 4 )
pf.2021$year <- pf.2021$TAX_PRD %>% substr( 1, 4 )
```

``` r
head(pc.2021$tax_pd)
head(pc.2021$year)
```

    [1] 202005 202012 202104 202003 202006 202012
    [1] "2020" "2020" "2021" "2020" "2020" "2020"

### Isolate the Study Period

SOI Extract files are organized by filing dates, the dates the returns
were received by the IRS. The filing date is usually around six months
after the end of an organization’s fiscal year, which is also twelve
months ahead of the calendar year that correspond to the nonprofits
activities.

In addition, nonprofits can back-file late returns or submit amended
returns, so although the year of the SOI extract file is different from
the periods of data contained within:

``` r
pc.2021 %>%
  group_by( year ) %>%
  summarise( n() ) %>%
  kable( col.names = c( "Tax Period", "Frequency" ),
         caption = "Tax Periods in the 2021 Form 990 Dataset", 
         align="c" ) 
```

| Tax Period | Frequency |
|:----------:|:---------:|
|    1980    |     1     |
|    2006    |     1     |
|    2007    |     2     |
|    2008    |     3     |
|    2009    |    11     |
|    2010    |    13     |
|    2011    |    17     |
|    2012    |    26     |
|    2013    |    47     |
|    2014    |    83     |
|    2015    |    144    |
|    2016    |    320    |
|    2017    |    757    |
|    2018    |   2516    |
|    2019    |   41258   |
|    2020    |  258312   |
|    2021    |   39409   |

Tax Periods in the 2021 Form 990 Dataset

``` r
pf.2021 %>%
  group_by( year ) %>%
  summarise( n() ) %>%
  kable( col.names = c( "Tax Period", "Frequency" ),
         caption = "Tax Periods in the 2021 Form 990-PF Dataset",
         align="c" ) 
```

| Tax Period | Frequency |
|:----------:|:---------:|
|    2007    |     1     |
|    2008    |     1     |
|    2009    |     3     |
|    2010    |     2     |
|    2011    |     6     |
|    2012    |    10     |
|    2013    |    14     |
|    2014    |    18     |
|    2015    |    21     |
|    2016    |    67     |
|    2017    |    170    |
|    2018    |    796    |
|    2019    |   17867   |
|    2020    |   95996   |
|    2021    |   10340   |

Tax Periods in the 2021 Form 990-PF Dataset

We need data that corresponds to the 2019 calendar year, the most recent
full year available at the time of writing.

``` r
pc.2021.subset <- filter( pc.2021, year=="2019" )
pf.2021.subset <- filter( pf.2021, year=="2019" )
pc.2020.subset <- filter( pc.2020, year=="2019" )
pf.2020.subset <- filter( pf.2020, year=="2019" )
pc.2019.subset <- filter( pc.2019, year=="2019" )
```

Inspect to ensure we have the correct data now:

``` r
pc.2021.subset %>%
  group_by( year ) %>%
  summarise( n() ) %>%
  kable( col.names = c( "Tax Period", "Frequency" ), align="c",
         caption = "Tax Periods in the Subsetted 2021 Form 990 Dataset" )
```

| Tax Period | Frequency |
|:----------:|:---------:|
|    2019    |   41258   |

Tax Periods in the Subsetted 2021 Form 990 Dataset

``` r
pf.2021.subset %>%
  group_by( year ) %>%
  summarise( n() ) %>%
  kable( col.names = c( "Tax Period", "Frequency" ), align="c",
         caption = "Tax Periods in the Subsetted 2021 Form 990-PF Dataset" ) 
```

| Tax Period | Frequency |
|:----------:|:---------:|
|    2019    |   17867   |

Tax Periods in the Subsetted 2021 Form 990-PF Dataset

### Harmonizing the Data

In order to combine files we need to ensure that the variable names are
the same.

R is case-sensitive; if one dataset uses uppercase letters for a column
name and another dataset uses lowercase letters for a column name, it
will not recognize them as the same column. As a result, it will not
combine the columns.

So we first make the names of all of the columns lowercase.

``` r
names( pc.2021.subset ) <- tolower( names( pc.2021.subset ) )
names( pc.2020.subset ) <- tolower( names( pc.2020.subset ) )
names( pc.2019.subset ) <- tolower( names( pc.2019.subset ) )
names( pf.2021.subset ) <- tolower( names( pf.2021.subset ) )
names( pf.2020.subset ) <- tolower( names( pf.2020.subset ) )
```

Then filter the data to include only the columns we plan to use in the
analysis. We use the following fields:

**Form 990-PF Columns**

| Column            | Description                                      | Location in Form 990 |
|-------------------|--------------------------------------------------|----------------------|
| ein               | Employer Identification Number                   | Header               |
| payrolltx         | Payroll taxes                                    | 990 Core_Pt IX-10(A) |
| compnsatncurrofcr | Compensation of current officers, directors, etc | 990 Core_Pt IX-5(A)  |
| compnsatnandothr  | Compensation of disqualified persons             | 990 Core_Pt IX-6(A)  |
| othrsalwages      | Other salaries and wages                         | 990 Core_Pt IX-7(A)  |
| pensionplancontrb | Pension plan contributions                       | 990 Core_Pt IX-8(A)  |
| othremplyeebenef  | Other employee benefits                          | 990 Core_Pt IX-9(A)  |

**Form 990-PF Columns**

| Column         | Description                      | Location in Form 990    |
|----------------|----------------------------------|-------------------------|
| ein            | Employer Identification Number   | Header                  |
| compofficers   | Compensation of officers         | 990-PF Pt I-13, col (a) |
| pensplemplbenf | Pension plans, employee benefits | 990-PF Pt I-15, col (a) |

``` r
pc.2021.subset2 <- 
  pc.2021.subset %>%
  select( ein, payrolltx, compnsatncurrofcr, 
          compnsatnandothr, othrsalwages, 
          pensionplancontrb, othremplyeebenef ) 

pc.2020.subset2 <- 
  pc.2020.subset %>%
  select( ein, payrolltx, compnsatncurrofcr, 
          compnsatnandothr, othrsalwages, 
          pensionplancontrb, othremplyeebenef ) 

pc.2019.subset2 <- 
  pc.2019.subset %>%
  select( ein, payrolltx, compnsatncurrofcr, 
          compnsatnandothr, othrsalwages, 
          pensionplancontrb, othremplyeebenef ) 

pf.2021.subset2 <- 
  pf.2021.subset %>%
  select( ein, compofficers, pensplemplbenf ) 

pf.2020.subset2 <- 
  pf.2020.subset %>%
  select( ein, compofficers, pensplemplbenf ) 
```

### Stack the data

``` r
pc <- 
  bind_rows( pc.2021.subset2, 
             pc.2020.subset2, 
             pc.2019.subset2 )
pf <- 
  bind_rows( pf.2021.subset2, 
             pf.2020.subset2 )
```

### Remove duplicate EINs

There might be some duplicate EINs in the combined datasets because some
organizations may have submitted amended returns.

``` r
# roughly 1,200 duplicates
n_distinct( pc$ein )
nrow( pc )

n_distinct( pf$ein )
nrow( pf )
```

    [1] 310349
    [1] 311145
    [1] 98253
    [1] 98337

Remove duplicate EINs from the combined datasets

``` r
pc <- pc %>% distinct( ein, .keep_all = TRUE )
pf <- pf %>% distinct( ein, .keep_all = TRUE )

# check to ensure it worked
n_distinct( pc$ein ) == nrow( pc )
n_distinct( pf$ein ) == nrow( pf )
```

    [1] TRUE
    [1] TRUE

## Analysis

### Step 1: Calculate Payroll Taxes Paid by Nonprofits

Calculate the amount of payroll taxes paid by Form 990 filers in 2019:

``` r
# helpful function for printing large numbers
dollarize <- function(x) 
{ paste0( "$", format( round(x,0), big.mark="," ) ) }

prtax <- sum( pc$payrolltx, na.rm = T )
prtax %>% dollarize()
```

    [1] "$55,464,358,803"

### Step 2: Estimate the Effective Payroll Tax Rate

Calculate a payroll tax/salaries and benefits ratio for Form 990 filers.

To calculate the ratio, we divide the amount of payroll taxes that Form
990 filers pay by the amount that Form 990 filers spend on salaries and
benefits.

Calculate the amount that Form 990 filers spend on salaries and
benefits.

``` r
salaries <- 
  sum( pc$compnsatncurrofcr,
       pc$compnsatnandothr,
       pc$othrsalwages,
       pc$pensionplancontrb,
       pc$othremplyeebenef, 
       na.rm = T )
```

The following function can be helpful for printing large numbers:

``` r
salaries %>% dollarize()
```

    [1] "$965,155,601,055"

We estimate the effective tax rate by tabulating the amount of payroll
taxes paid by nonprofits divided by total salary costs:

``` r
ratio <- prtax / salaries
paste0( 100*round( ratio, 4 ), "%" )
```

    [1] "5.75%"

### Step 3: Estimate PF Contributions

It’s not perfect, but it allows us to roughly estimate the payroll taxes
paid by private foundations since that data is not included in the SOI
PF Extracts:

``` r
salaries.pf <- 
  sum( pf$compofficers, 
       pf$pensplemplbenf, 
       na.rm = T )

salaries.pf %>% dollarize()
```

    [1] "$2,670,373,181"

Estimated payroll taxes contributed by PFs:

``` r
prtax.pf <- ratio * salaries.pf

prtax.pf %>% dollarize()
```

    [1] "$153,457,677"

### Step 4: Tabulate Totals

Add the amount of payroll taxes that nonprofits pay to the estimated
amount of payroll taxes contributed by Private Foundations:

``` r
tax.total <- prtax + prtax.pf
tax.total %>% dollarize()
```

    [1] "$55,617,816,480"

### Step 5: Adjust for Inflation

Normally inflation would be marginal over a 2-3 year period, but
inflation was excessive during the time frame of the study (2019 to
2023). As a result we should include an inflation adjustment. This
scaling factor can be obtained by entering the start and end year on any
reputable online inflation generator.

``` r
( tax.total * 1.1885 ) %>% dollarize()
```

    [1] "$66,101,774,886"

We see that the estimate increased from \$55 billion to \$66 billion, a
significant amount.

We would interpret this number either as the value of 2019 nonprofit
payroll tax contributions in today’s dollars, or as an estimate of what
nonprofits are likely paying in 2023 if we assume employment rates were
similar in those two time periods.

## Conclusion

We estimate that nonprofits pay approximately \$66 billion per year in
payroll taxes.

As we described at the beginning of this tutorial, this is a
conservative estimate.

- It does not include the payroll taxes that Form 990-EZ or Form 990-N
  filers pay.
- It underestimates the payroll taxes that private foundations pay.
- It also does not include the payroll taxes from nonprofits that are
  not required to file annual 990 returns (for example, churches).

This approach is a useful way to demonstrate the size and economic
contributions of the nonprofit sector using freely available IRS
administrative tax data.

<br> <br>
<hr>

<br> <br>