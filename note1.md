My R Notes for Data management
================
Congli (Claire) Zhang
Starting at: 6/28/2021

This is my notes of R code for data manipulation and management.

Organizing the notes using:

-   header 2, area of use, e.g., visualization, clean and tidy data,
    modeling, etc.
-   header 3, package::function
-   header 4, whatever important heading for that function

## 1. Importing and exporting data

When you don’t have a project created beforehand:

``` r
# here::i_am("your code folder/your R or RMD file")
# datatable <- rio::import(here::here("your data folder", "your data file"))
# rio::export(your datatable, here::here("your data folder", "your data file"))
```

When you have an existing project:

``` r
# datatable <- rio::import(here::here("your data folder", "your data file"))
# rio::export(your datatable, here::here("your data folder", "your data file"))
```

## 2. Inspecting the dataset

### Base R - handy, quick, and flexible

#### head()

arrange by a variable and show whatever number of observations you like:

``` r
head(mtcars %>% arrange(desc(gear)), 10) 
```

    ##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Ford Pantera L 15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## Maserati Bora  15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
    ## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2

#### table()

create tabular results of categorical variables. check for one variable:

``` r
table(mtcars$cyl) # does not report NA's
```

    ## 
    ##  4  6  8 
    ## 11  7 14

``` r
table(mtcars$cyl, exclude = NULL) # reports NA's
```

    ## 
    ##  4  6  8 
    ## 11  7 14

``` r
table(mtcars$cyl, exclude = "4")
```

    ## 
    ##  6  8 
    ##  7 14

``` r
table(mtcars$gear > 4)
```

    ## 
    ## FALSE  TRUE 
    ##    27     5

``` r
table(mtcars$gear > 4, useNA = "always")
```

    ## 
    ## FALSE  TRUE  <NA> 
    ##    27     5     0

contingency table for two variables:

``` r
table(mtcars$cyl, is.na(mtcars$carb))
```

    ##    
    ##     FALSE
    ##   4    11
    ##   6     7
    ##   8    14

binned frequency table:

``` r
table(cut(mtcars$cyl, seq(min(mtcars$cyl), max(mtcars$cyl), by = 0.5)))
```

    ## 
    ## (4,4.5] (4.5,5] (5,5.5] (5.5,6] (6,6.5] (6.5,7] (7,7.5] (7.5,8] 
    ##       0       0       0       7       0       0       0      14

### gtsummary::tbl_summary()

Instantly fell in love with this package/function when I was exhausting
options to create the infamous “Table 1”. Made a habit of doing this for
first inspection of new data.

the default result:

``` r
library(gtsummary)
tbl_summary(mtcars)
```

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#dbhkyrgkds .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#dbhkyrgkds .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#dbhkyrgkds .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#dbhkyrgkds .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#dbhkyrgkds .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dbhkyrgkds .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#dbhkyrgkds .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#dbhkyrgkds .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#dbhkyrgkds .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#dbhkyrgkds .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#dbhkyrgkds .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#dbhkyrgkds .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#dbhkyrgkds .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#dbhkyrgkds .gt_from_md > :first-child {
  margin-top: 0;
}

#dbhkyrgkds .gt_from_md > :last-child {
  margin-bottom: 0;
}

#dbhkyrgkds .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#dbhkyrgkds .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#dbhkyrgkds .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#dbhkyrgkds .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#dbhkyrgkds .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#dbhkyrgkds .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#dbhkyrgkds .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#dbhkyrgkds .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dbhkyrgkds .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#dbhkyrgkds .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#dbhkyrgkds .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#dbhkyrgkds .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#dbhkyrgkds .gt_left {
  text-align: left;
}

#dbhkyrgkds .gt_center {
  text-align: center;
}

#dbhkyrgkds .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#dbhkyrgkds .gt_font_normal {
  font-weight: normal;
}

#dbhkyrgkds .gt_font_bold {
  font-weight: bold;
}

#dbhkyrgkds .gt_font_italic {
  font-style: italic;
}

#dbhkyrgkds .gt_super {
  font-size: 65%;
}

#dbhkyrgkds .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="dbhkyrgkds" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Characteristic</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>N = 32</strong><sup class="gt_footnote_marks">1</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left">mpg</td>
      <td class="gt_row gt_center">19.2 (15.4, 22.8)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">cyl</td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">11 (34%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
      <td class="gt_row gt_center">7 (22%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
      <td class="gt_row gt_center">14 (44%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">disp</td>
      <td class="gt_row gt_center">196 (121, 326)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">hp</td>
      <td class="gt_row gt_center">123 (96, 180)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">drat</td>
      <td class="gt_row gt_center">3.70 (3.08, 3.92)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">wt</td>
      <td class="gt_row gt_center">3.33 (2.58, 3.61)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">qsec</td>
      <td class="gt_row gt_center">17.71 (16.89, 18.90)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">vs</td>
      <td class="gt_row gt_center">14 (44%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">am</td>
      <td class="gt_row gt_center">13 (41%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">gear</td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
      <td class="gt_row gt_center">15 (47%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">12 (38%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
      <td class="gt_row gt_center">5 (16%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">carb</td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">1</td>
      <td class="gt_row gt_center">7 (22%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">2</td>
      <td class="gt_row gt_center">10 (31%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
      <td class="gt_row gt_center">3 (9.4%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">10 (31%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
      <td class="gt_row gt_center">1 (3.1%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
      <td class="gt_row gt_center">1 (3.1%)</td>
    </tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="2">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Statistics presented: Median (IQR); n (%)
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table></div>

the default result for comparison table:

``` r
tbl_summary(mtcars, by = cyl)
```

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#xuaajozicl .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#xuaajozicl .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#xuaajozicl .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#xuaajozicl .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#xuaajozicl .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xuaajozicl .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#xuaajozicl .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#xuaajozicl .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#xuaajozicl .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#xuaajozicl .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#xuaajozicl .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#xuaajozicl .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#xuaajozicl .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#xuaajozicl .gt_from_md > :first-child {
  margin-top: 0;
}

#xuaajozicl .gt_from_md > :last-child {
  margin-bottom: 0;
}

#xuaajozicl .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#xuaajozicl .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#xuaajozicl .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#xuaajozicl .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#xuaajozicl .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#xuaajozicl .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#xuaajozicl .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#xuaajozicl .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#xuaajozicl .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#xuaajozicl .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#xuaajozicl .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#xuaajozicl .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#xuaajozicl .gt_left {
  text-align: left;
}

#xuaajozicl .gt_center {
  text-align: center;
}

#xuaajozicl .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#xuaajozicl .gt_font_normal {
  font-weight: normal;
}

#xuaajozicl .gt_font_bold {
  font-weight: bold;
}

#xuaajozicl .gt_font_italic {
  font-style: italic;
}

#xuaajozicl .gt_super {
  font-size: 65%;
}

#xuaajozicl .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="xuaajozicl" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Characteristic</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>4</strong>, N = 11<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>6</strong>, N = 7<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>8</strong>, N = 14<sup class="gt_footnote_marks">1</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left">mpg</td>
      <td class="gt_row gt_center">26.0 (22.8, 30.4)</td>
      <td class="gt_row gt_center">19.7 (18.6, 21.0)</td>
      <td class="gt_row gt_center">15.2 (14.4, 16.2)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">disp</td>
      <td class="gt_row gt_center">108 (79, 121)</td>
      <td class="gt_row gt_center">168 (160, 196)</td>
      <td class="gt_row gt_center">350 (302, 390)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">hp</td>
      <td class="gt_row gt_center">91 (66, 96)</td>
      <td class="gt_row gt_center">110 (110, 123)</td>
      <td class="gt_row gt_center">192 (176, 241)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">drat</td>
      <td class="gt_row gt_center">4.08 (3.81, 4.17)</td>
      <td class="gt_row gt_center">3.90 (3.35, 3.91)</td>
      <td class="gt_row gt_center">3.12 (3.07, 3.22)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">wt</td>
      <td class="gt_row gt_center">2.20 (1.88, 2.62)</td>
      <td class="gt_row gt_center">3.21 (2.82, 3.44)</td>
      <td class="gt_row gt_center">3.75 (3.53, 4.01)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">qsec</td>
      <td class="gt_row gt_center">18.90 (18.56, 19.95)</td>
      <td class="gt_row gt_center">18.30 (16.74, 19.17)</td>
      <td class="gt_row gt_center">17.18 (16.10, 17.55)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">vs</td>
      <td class="gt_row gt_center">10 (91%)</td>
      <td class="gt_row gt_center">4 (57%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">am</td>
      <td class="gt_row gt_center">8 (73%)</td>
      <td class="gt_row gt_center">3 (43%)</td>
      <td class="gt_row gt_center">2 (14%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">gear</td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
      <td class="gt_row gt_center">1 (9.1%)</td>
      <td class="gt_row gt_center">2 (29%)</td>
      <td class="gt_row gt_center">12 (86%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">8 (73%)</td>
      <td class="gt_row gt_center">4 (57%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
      <td class="gt_row gt_center">2 (18%)</td>
      <td class="gt_row gt_center">1 (14%)</td>
      <td class="gt_row gt_center">2 (14%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">carb</td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">1</td>
      <td class="gt_row gt_center">5 (45%)</td>
      <td class="gt_row gt_center">2 (29%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">2</td>
      <td class="gt_row gt_center">6 (55%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">4 (29%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">3 (21%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">4 (57%)</td>
      <td class="gt_row gt_center">6 (43%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">1 (14%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">1 (7.1%)</td>
    </tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="4">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Statistics presented: Median (IQR); n (%)
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table></div>

it’s arguments are not super handy though:

``` r
mtcars %>% 
  tbl_summary(by = cyl) %>%
  modify_header(label ~ "**Key Variables**")
```

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#yzyigwxzeo .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#yzyigwxzeo .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yzyigwxzeo .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#yzyigwxzeo .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#yzyigwxzeo .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yzyigwxzeo .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yzyigwxzeo .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#yzyigwxzeo .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#yzyigwxzeo .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#yzyigwxzeo .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#yzyigwxzeo .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#yzyigwxzeo .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#yzyigwxzeo .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#yzyigwxzeo .gt_from_md > :first-child {
  margin-top: 0;
}

#yzyigwxzeo .gt_from_md > :last-child {
  margin-bottom: 0;
}

#yzyigwxzeo .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#yzyigwxzeo .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#yzyigwxzeo .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yzyigwxzeo .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#yzyigwxzeo .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yzyigwxzeo .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#yzyigwxzeo .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#yzyigwxzeo .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yzyigwxzeo .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yzyigwxzeo .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#yzyigwxzeo .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yzyigwxzeo .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#yzyigwxzeo .gt_left {
  text-align: left;
}

#yzyigwxzeo .gt_center {
  text-align: center;
}

#yzyigwxzeo .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#yzyigwxzeo .gt_font_normal {
  font-weight: normal;
}

#yzyigwxzeo .gt_font_bold {
  font-weight: bold;
}

#yzyigwxzeo .gt_font_italic {
  font-style: italic;
}

#yzyigwxzeo .gt_super {
  font-size: 65%;
}

#yzyigwxzeo .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="yzyigwxzeo" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Key Variables</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>4</strong>, N = 11<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>6</strong>, N = 7<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>8</strong>, N = 14<sup class="gt_footnote_marks">1</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left">mpg</td>
      <td class="gt_row gt_center">26.0 (22.8, 30.4)</td>
      <td class="gt_row gt_center">19.7 (18.6, 21.0)</td>
      <td class="gt_row gt_center">15.2 (14.4, 16.2)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">disp</td>
      <td class="gt_row gt_center">108 (79, 121)</td>
      <td class="gt_row gt_center">168 (160, 196)</td>
      <td class="gt_row gt_center">350 (302, 390)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">hp</td>
      <td class="gt_row gt_center">91 (66, 96)</td>
      <td class="gt_row gt_center">110 (110, 123)</td>
      <td class="gt_row gt_center">192 (176, 241)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">drat</td>
      <td class="gt_row gt_center">4.08 (3.81, 4.17)</td>
      <td class="gt_row gt_center">3.90 (3.35, 3.91)</td>
      <td class="gt_row gt_center">3.12 (3.07, 3.22)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">wt</td>
      <td class="gt_row gt_center">2.20 (1.88, 2.62)</td>
      <td class="gt_row gt_center">3.21 (2.82, 3.44)</td>
      <td class="gt_row gt_center">3.75 (3.53, 4.01)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">qsec</td>
      <td class="gt_row gt_center">18.90 (18.56, 19.95)</td>
      <td class="gt_row gt_center">18.30 (16.74, 19.17)</td>
      <td class="gt_row gt_center">17.18 (16.10, 17.55)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">vs</td>
      <td class="gt_row gt_center">10 (91%)</td>
      <td class="gt_row gt_center">4 (57%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">am</td>
      <td class="gt_row gt_center">8 (73%)</td>
      <td class="gt_row gt_center">3 (43%)</td>
      <td class="gt_row gt_center">2 (14%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">gear</td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
      <td class="gt_row gt_center">1 (9.1%)</td>
      <td class="gt_row gt_center">2 (29%)</td>
      <td class="gt_row gt_center">12 (86%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">8 (73%)</td>
      <td class="gt_row gt_center">4 (57%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
      <td class="gt_row gt_center">2 (18%)</td>
      <td class="gt_row gt_center">1 (14%)</td>
      <td class="gt_row gt_center">2 (14%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">carb</td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">1</td>
      <td class="gt_row gt_center">5 (45%)</td>
      <td class="gt_row gt_center">2 (29%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">2</td>
      <td class="gt_row gt_center">6 (55%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">4 (29%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">3 (21%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">4 (57%)</td>
      <td class="gt_row gt_center">6 (43%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">1 (14%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">0 (0%)</td>
      <td class="gt_row gt_center">1 (7.1%)</td>
    </tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="4">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Statistics presented: Median (IQR); n (%)
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table></div>

just include a few variables:

``` r
mtcars %>% 
  tbl_summary(by = cyl,
              include = c(cyl, mpg, vs, gear),
              statistic = list(all_continuous() ~ "{mean} ({sd})",
                               all_categorical() ~ "{n} / {N} ({p}%)")) %>% 
  add_p() %>% 
  modify_header(label ~ "**Key Variables**")
```

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#raciedmaym .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#raciedmaym .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#raciedmaym .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#raciedmaym .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#raciedmaym .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#raciedmaym .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#raciedmaym .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#raciedmaym .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#raciedmaym .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#raciedmaym .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#raciedmaym .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#raciedmaym .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#raciedmaym .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#raciedmaym .gt_from_md > :first-child {
  margin-top: 0;
}

#raciedmaym .gt_from_md > :last-child {
  margin-bottom: 0;
}

#raciedmaym .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#raciedmaym .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#raciedmaym .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#raciedmaym .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#raciedmaym .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#raciedmaym .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#raciedmaym .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#raciedmaym .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#raciedmaym .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#raciedmaym .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#raciedmaym .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#raciedmaym .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#raciedmaym .gt_left {
  text-align: left;
}

#raciedmaym .gt_center {
  text-align: center;
}

#raciedmaym .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#raciedmaym .gt_font_normal {
  font-weight: normal;
}

#raciedmaym .gt_font_bold {
  font-weight: bold;
}

#raciedmaym .gt_font_italic {
  font-style: italic;
}

#raciedmaym .gt_super {
  font-size: 65%;
}

#raciedmaym .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="raciedmaym" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Key Variables</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>4</strong>, N = 11<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>6</strong>, N = 7<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>8</strong>, N = 14<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>p-value</strong><sup class="gt_footnote_marks">2</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left">mpg</td>
      <td class="gt_row gt_center">26.7 (4.5)</td>
      <td class="gt_row gt_center">19.7 (1.5)</td>
      <td class="gt_row gt_center">15.1 (2.6)</td>
      <td class="gt_row gt_center"><0.001</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">vs</td>
      <td class="gt_row gt_center">10 / 11 (91%)</td>
      <td class="gt_row gt_center">4 / 7 (57%)</td>
      <td class="gt_row gt_center">0 / 14 (0%)</td>
      <td class="gt_row gt_center"><0.001</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">gear</td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"><0.001</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
      <td class="gt_row gt_center">1 / 11 (9.1%)</td>
      <td class="gt_row gt_center">2 / 7 (29%)</td>
      <td class="gt_row gt_center">12 / 14 (86%)</td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
      <td class="gt_row gt_center">8 / 11 (73%)</td>
      <td class="gt_row gt_center">4 / 7 (57%)</td>
      <td class="gt_row gt_center">0 / 14 (0%)</td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
      <td class="gt_row gt_center">2 / 11 (18%)</td>
      <td class="gt_row gt_center">1 / 7 (14%)</td>
      <td class="gt_row gt_center">2 / 14 (14%)</td>
      <td class="gt_row gt_center"></td>
    </tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="5">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Statistics presented: Mean (SD); n / N (%)
          <br />
        </p>
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>2</em>
          </sup>
           
          Statistical tests performed: Kruskal-Wallis test; Fisher's exact test
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table></div>

get fancier with the names of the variables and levels

``` r
mtcars$gear <- factor(mtcars$gear, levels = c(3, 4, 5), labels = c("Three", "Four", "Five"))

mtcars %>% 
  tbl_summary(by = cyl,
              include = c(cyl, mpg, vs, gear),
              label = list(mpg = "MPG",
                           vs = "VS",
                           gear = "GEAR"),
              statistic = list(all_continuous() ~ "{mean} ({sd})",
                               all_categorical() ~ "{n} / {N} ({p}%)")) %>% 
  add_p() %>% 
  modify_header(label ~ "**Key Variables**")
```

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#lxprqdrdqg .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#lxprqdrdqg .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#lxprqdrdqg .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#lxprqdrdqg .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#lxprqdrdqg .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#lxprqdrdqg .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#lxprqdrdqg .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#lxprqdrdqg .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#lxprqdrdqg .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#lxprqdrdqg .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#lxprqdrdqg .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#lxprqdrdqg .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#lxprqdrdqg .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#lxprqdrdqg .gt_from_md > :first-child {
  margin-top: 0;
}

#lxprqdrdqg .gt_from_md > :last-child {
  margin-bottom: 0;
}

#lxprqdrdqg .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#lxprqdrdqg .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#lxprqdrdqg .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#lxprqdrdqg .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#lxprqdrdqg .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#lxprqdrdqg .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#lxprqdrdqg .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#lxprqdrdqg .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#lxprqdrdqg .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#lxprqdrdqg .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#lxprqdrdqg .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#lxprqdrdqg .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#lxprqdrdqg .gt_left {
  text-align: left;
}

#lxprqdrdqg .gt_center {
  text-align: center;
}

#lxprqdrdqg .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#lxprqdrdqg .gt_font_normal {
  font-weight: normal;
}

#lxprqdrdqg .gt_font_bold {
  font-weight: bold;
}

#lxprqdrdqg .gt_font_italic {
  font-style: italic;
}

#lxprqdrdqg .gt_super {
  font-size: 65%;
}

#lxprqdrdqg .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="lxprqdrdqg" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Key Variables</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>4</strong>, N = 11<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>6</strong>, N = 7<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>8</strong>, N = 14<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>p-value</strong><sup class="gt_footnote_marks">2</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left">MPG</td>
      <td class="gt_row gt_center">26.7 (4.5)</td>
      <td class="gt_row gt_center">19.7 (1.5)</td>
      <td class="gt_row gt_center">15.1 (2.6)</td>
      <td class="gt_row gt_center"><0.001</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">VS</td>
      <td class="gt_row gt_center">10 / 11 (91%)</td>
      <td class="gt_row gt_center">4 / 7 (57%)</td>
      <td class="gt_row gt_center">0 / 14 (0%)</td>
      <td class="gt_row gt_center"><0.001</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">GEAR</td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"></td>
      <td class="gt_row gt_center"><0.001</td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Three</td>
      <td class="gt_row gt_center">1 / 11 (9.1%)</td>
      <td class="gt_row gt_center">2 / 7 (29%)</td>
      <td class="gt_row gt_center">12 / 14 (86%)</td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Four</td>
      <td class="gt_row gt_center">8 / 11 (73%)</td>
      <td class="gt_row gt_center">4 / 7 (57%)</td>
      <td class="gt_row gt_center">0 / 14 (0%)</td>
      <td class="gt_row gt_center"></td>
    </tr>
    <tr>
      <td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Five</td>
      <td class="gt_row gt_center">2 / 11 (18%)</td>
      <td class="gt_row gt_center">1 / 7 (14%)</td>
      <td class="gt_row gt_center">2 / 14 (14%)</td>
      <td class="gt_row gt_center"></td>
    </tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="5">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Statistics presented: Mean (SD); n / N (%)
          <br />
        </p>
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>2</em>
          </sup>
           
          Statistical tests performed: Kruskal-Wallis test; Fisher's exact test
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table></div>

## 3. Cleaning and tidying data

### dplyr::mutate()

recode:

``` r
foo <- mtcars %>% 
  mutate(cyl = recode(cyl, "4" = "four", "6" = "six", "8" = "eight"))
```

panel data strategy - you may want to set flag for cases when you have
missing values on a time-invariant variable, gender for example, you
want to code the missing value from a certain year by using the
non-missing value from other years:

``` r
foo <- mtcars %>%
  group_by(cyl) %>% 
  arrange(gear) %>% 
  mutate(last_obs_fg = row_number() == n()) %>% 
  mutate(carb_new = ifelse(carb == 4, carb, carb[last_obs_fg == TRUE]))
```
