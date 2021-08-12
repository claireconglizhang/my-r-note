My R Notes for Data management
================
Congli (Claire) Zhang
Starting at: 6/28/2021

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

               ┌────────────────────────────────────────────────
               │  mpg   cyl    disp    hp   drat     wt   qsec  
               ├────────────────────────────────────────────────
               │ 26       4   120      91   4.43   2.14   16.7  
               │ 30.4     4    95.1   113   3.77   1.51   16.9  
               │ 15.8     8   351     264   4.22   3.17   14.5  
               │ 19.7     6   145     175   3.62   2.77   15.5  
               │ 15       8   301     335   3.54   3.57   14.6  
               │ 21       6   160     110   3.9    2.62   16.5  
               │ 21       6   160     110   3.9    2.88   17    
               │ 22.8     4   108      93   3.85   2.32   18.6  
               │ 24.4     4   147      62   3.69   3.19   20    
               │ 22.8     4   141      95   3.92   3.15   22.9  
               └────────────────────────────────────────────────

Column names: mpg, cyl, disp, hp, drat, wt, qsec, vs, am, gear, carb

7/11 columns shown.

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

<div id="cnatbjauzm" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#cnatbjauzm .gt_table {
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

#cnatbjauzm .gt_heading {
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

#cnatbjauzm .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#cnatbjauzm .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#cnatbjauzm .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cnatbjauzm .gt_col_headings {
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

#cnatbjauzm .gt_col_heading {
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

#cnatbjauzm .gt_column_spanner_outer {
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

#cnatbjauzm .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#cnatbjauzm .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#cnatbjauzm .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#cnatbjauzm .gt_group_heading {
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

#cnatbjauzm .gt_empty_group_heading {
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

#cnatbjauzm .gt_from_md > :first-child {
  margin-top: 0;
}

#cnatbjauzm .gt_from_md > :last-child {
  margin-bottom: 0;
}

#cnatbjauzm .gt_row {
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

#cnatbjauzm .gt_stub {
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

#cnatbjauzm .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cnatbjauzm .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#cnatbjauzm .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cnatbjauzm .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#cnatbjauzm .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#cnatbjauzm .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cnatbjauzm .gt_footnotes {
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

#cnatbjauzm .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#cnatbjauzm .gt_sourcenotes {
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

#cnatbjauzm .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#cnatbjauzm .gt_left {
  text-align: left;
}

#cnatbjauzm .gt_center {
  text-align: center;
}

#cnatbjauzm .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#cnatbjauzm .gt_font_normal {
  font-weight: normal;
}

#cnatbjauzm .gt_font_bold {
  font-weight: bold;
}

#cnatbjauzm .gt_font_italic {
  font-style: italic;
}

#cnatbjauzm .gt_super {
  font-size: 65%;
}

#cnatbjauzm .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Characteristic</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>N = 32</strong><sup class="gt_footnote_marks">1</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">mpg</td>
<td class="gt_row gt_center">19.2 (15.4, 22.8)</td></tr>
    <tr><td class="gt_row gt_left">cyl</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">11 (34%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
<td class="gt_row gt_center">7 (22%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
<td class="gt_row gt_center">14 (44%)</td></tr>
    <tr><td class="gt_row gt_left">disp</td>
<td class="gt_row gt_center">196 (121, 326)</td></tr>
    <tr><td class="gt_row gt_left">hp</td>
<td class="gt_row gt_center">123 (96, 180)</td></tr>
    <tr><td class="gt_row gt_left">drat</td>
<td class="gt_row gt_center">3.70 (3.08, 3.92)</td></tr>
    <tr><td class="gt_row gt_left">wt</td>
<td class="gt_row gt_center">3.33 (2.58, 3.61)</td></tr>
    <tr><td class="gt_row gt_left">qsec</td>
<td class="gt_row gt_center">17.71 (16.89, 18.90)</td></tr>
    <tr><td class="gt_row gt_left">vs</td>
<td class="gt_row gt_center">14 (44%)</td></tr>
    <tr><td class="gt_row gt_left">am</td>
<td class="gt_row gt_center">13 (41%)</td></tr>
    <tr><td class="gt_row gt_left">gear</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
<td class="gt_row gt_center">15 (47%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">12 (38%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
<td class="gt_row gt_center">5 (16%)</td></tr>
    <tr><td class="gt_row gt_left">carb</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">1</td>
<td class="gt_row gt_center">7 (22%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">2</td>
<td class="gt_row gt_center">10 (31%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
<td class="gt_row gt_center">3 (9.4%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">10 (31%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
<td class="gt_row gt_center">1 (3.1%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
<td class="gt_row gt_center">1 (3.1%)</td></tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="2">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Median (IQR); n (%)
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table>
</div>

the default result for comparison table:

``` r
tbl_summary(mtcars, by = cyl)
```

<div id="jkbtayccnb" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#jkbtayccnb .gt_table {
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

#jkbtayccnb .gt_heading {
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

#jkbtayccnb .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#jkbtayccnb .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#jkbtayccnb .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#jkbtayccnb .gt_col_headings {
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

#jkbtayccnb .gt_col_heading {
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

#jkbtayccnb .gt_column_spanner_outer {
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

#jkbtayccnb .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#jkbtayccnb .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#jkbtayccnb .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#jkbtayccnb .gt_group_heading {
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

#jkbtayccnb .gt_empty_group_heading {
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

#jkbtayccnb .gt_from_md > :first-child {
  margin-top: 0;
}

#jkbtayccnb .gt_from_md > :last-child {
  margin-bottom: 0;
}

#jkbtayccnb .gt_row {
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

#jkbtayccnb .gt_stub {
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

#jkbtayccnb .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#jkbtayccnb .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#jkbtayccnb .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#jkbtayccnb .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#jkbtayccnb .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#jkbtayccnb .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#jkbtayccnb .gt_footnotes {
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

#jkbtayccnb .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#jkbtayccnb .gt_sourcenotes {
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

#jkbtayccnb .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#jkbtayccnb .gt_left {
  text-align: left;
}

#jkbtayccnb .gt_center {
  text-align: center;
}

#jkbtayccnb .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#jkbtayccnb .gt_font_normal {
  font-weight: normal;
}

#jkbtayccnb .gt_font_bold {
  font-weight: bold;
}

#jkbtayccnb .gt_font_italic {
  font-style: italic;
}

#jkbtayccnb .gt_super {
  font-size: 65%;
}

#jkbtayccnb .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Characteristic</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>4</strong>, N = 11<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>6</strong>, N = 7<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>8</strong>, N = 14<sup class="gt_footnote_marks">1</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">mpg</td>
<td class="gt_row gt_center">26.0 (22.8, 30.4)</td>
<td class="gt_row gt_center">19.7 (18.6, 21.0)</td>
<td class="gt_row gt_center">15.2 (14.4, 16.2)</td></tr>
    <tr><td class="gt_row gt_left">disp</td>
<td class="gt_row gt_center">108 (79, 121)</td>
<td class="gt_row gt_center">168 (160, 196)</td>
<td class="gt_row gt_center">350 (302, 390)</td></tr>
    <tr><td class="gt_row gt_left">hp</td>
<td class="gt_row gt_center">91 (66, 96)</td>
<td class="gt_row gt_center">110 (110, 123)</td>
<td class="gt_row gt_center">192 (176, 241)</td></tr>
    <tr><td class="gt_row gt_left">drat</td>
<td class="gt_row gt_center">4.08 (3.81, 4.17)</td>
<td class="gt_row gt_center">3.90 (3.35, 3.91)</td>
<td class="gt_row gt_center">3.12 (3.07, 3.22)</td></tr>
    <tr><td class="gt_row gt_left">wt</td>
<td class="gt_row gt_center">2.20 (1.88, 2.62)</td>
<td class="gt_row gt_center">3.21 (2.82, 3.44)</td>
<td class="gt_row gt_center">3.75 (3.53, 4.01)</td></tr>
    <tr><td class="gt_row gt_left">qsec</td>
<td class="gt_row gt_center">18.90 (18.56, 19.95)</td>
<td class="gt_row gt_center">18.30 (16.74, 19.17)</td>
<td class="gt_row gt_center">17.18 (16.10, 17.55)</td></tr>
    <tr><td class="gt_row gt_left">vs</td>
<td class="gt_row gt_center">10 (91%)</td>
<td class="gt_row gt_center">4 (57%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left">am</td>
<td class="gt_row gt_center">8 (73%)</td>
<td class="gt_row gt_center">3 (43%)</td>
<td class="gt_row gt_center">2 (14%)</td></tr>
    <tr><td class="gt_row gt_left">gear</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
<td class="gt_row gt_center">1 (9.1%)</td>
<td class="gt_row gt_center">2 (29%)</td>
<td class="gt_row gt_center">12 (86%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">8 (73%)</td>
<td class="gt_row gt_center">4 (57%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
<td class="gt_row gt_center">2 (18%)</td>
<td class="gt_row gt_center">1 (14%)</td>
<td class="gt_row gt_center">2 (14%)</td></tr>
    <tr><td class="gt_row gt_left">carb</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">1</td>
<td class="gt_row gt_center">5 (45%)</td>
<td class="gt_row gt_center">2 (29%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">2</td>
<td class="gt_row gt_center">6 (55%)</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">4 (29%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">3 (21%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">4 (57%)</td>
<td class="gt_row gt_center">6 (43%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">1 (14%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">1 (7.1%)</td></tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="4">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Median (IQR); n (%)
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table>
</div>

it’s arguments are not super handy though:

``` r
mtcars %>% 
  tbl_summary(by = cyl) %>%
  modify_header(label ~ "**Key Variables**")
```

<div id="edqmgskuov" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#edqmgskuov .gt_table {
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

#edqmgskuov .gt_heading {
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

#edqmgskuov .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#edqmgskuov .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#edqmgskuov .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#edqmgskuov .gt_col_headings {
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

#edqmgskuov .gt_col_heading {
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

#edqmgskuov .gt_column_spanner_outer {
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

#edqmgskuov .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#edqmgskuov .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#edqmgskuov .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#edqmgskuov .gt_group_heading {
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

#edqmgskuov .gt_empty_group_heading {
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

#edqmgskuov .gt_from_md > :first-child {
  margin-top: 0;
}

#edqmgskuov .gt_from_md > :last-child {
  margin-bottom: 0;
}

#edqmgskuov .gt_row {
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

#edqmgskuov .gt_stub {
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

#edqmgskuov .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#edqmgskuov .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#edqmgskuov .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#edqmgskuov .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#edqmgskuov .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#edqmgskuov .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#edqmgskuov .gt_footnotes {
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

#edqmgskuov .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#edqmgskuov .gt_sourcenotes {
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

#edqmgskuov .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#edqmgskuov .gt_left {
  text-align: left;
}

#edqmgskuov .gt_center {
  text-align: center;
}

#edqmgskuov .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#edqmgskuov .gt_font_normal {
  font-weight: normal;
}

#edqmgskuov .gt_font_bold {
  font-weight: bold;
}

#edqmgskuov .gt_font_italic {
  font-style: italic;
}

#edqmgskuov .gt_super {
  font-size: 65%;
}

#edqmgskuov .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Key Variables</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>4</strong>, N = 11<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>6</strong>, N = 7<sup class="gt_footnote_marks">1</sup></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>8</strong>, N = 14<sup class="gt_footnote_marks">1</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">mpg</td>
<td class="gt_row gt_center">26.0 (22.8, 30.4)</td>
<td class="gt_row gt_center">19.7 (18.6, 21.0)</td>
<td class="gt_row gt_center">15.2 (14.4, 16.2)</td></tr>
    <tr><td class="gt_row gt_left">disp</td>
<td class="gt_row gt_center">108 (79, 121)</td>
<td class="gt_row gt_center">168 (160, 196)</td>
<td class="gt_row gt_center">350 (302, 390)</td></tr>
    <tr><td class="gt_row gt_left">hp</td>
<td class="gt_row gt_center">91 (66, 96)</td>
<td class="gt_row gt_center">110 (110, 123)</td>
<td class="gt_row gt_center">192 (176, 241)</td></tr>
    <tr><td class="gt_row gt_left">drat</td>
<td class="gt_row gt_center">4.08 (3.81, 4.17)</td>
<td class="gt_row gt_center">3.90 (3.35, 3.91)</td>
<td class="gt_row gt_center">3.12 (3.07, 3.22)</td></tr>
    <tr><td class="gt_row gt_left">wt</td>
<td class="gt_row gt_center">2.20 (1.88, 2.62)</td>
<td class="gt_row gt_center">3.21 (2.82, 3.44)</td>
<td class="gt_row gt_center">3.75 (3.53, 4.01)</td></tr>
    <tr><td class="gt_row gt_left">qsec</td>
<td class="gt_row gt_center">18.90 (18.56, 19.95)</td>
<td class="gt_row gt_center">18.30 (16.74, 19.17)</td>
<td class="gt_row gt_center">17.18 (16.10, 17.55)</td></tr>
    <tr><td class="gt_row gt_left">vs</td>
<td class="gt_row gt_center">10 (91%)</td>
<td class="gt_row gt_center">4 (57%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left">am</td>
<td class="gt_row gt_center">8 (73%)</td>
<td class="gt_row gt_center">3 (43%)</td>
<td class="gt_row gt_center">2 (14%)</td></tr>
    <tr><td class="gt_row gt_left">gear</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
<td class="gt_row gt_center">1 (9.1%)</td>
<td class="gt_row gt_center">2 (29%)</td>
<td class="gt_row gt_center">12 (86%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">8 (73%)</td>
<td class="gt_row gt_center">4 (57%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
<td class="gt_row gt_center">2 (18%)</td>
<td class="gt_row gt_center">1 (14%)</td>
<td class="gt_row gt_center">2 (14%)</td></tr>
    <tr><td class="gt_row gt_left">carb</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">1</td>
<td class="gt_row gt_center">5 (45%)</td>
<td class="gt_row gt_center">2 (29%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">2</td>
<td class="gt_row gt_center">6 (55%)</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">4 (29%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">3 (21%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">4 (57%)</td>
<td class="gt_row gt_center">6 (43%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">6</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">1 (14%)</td>
<td class="gt_row gt_center">0 (0%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">8</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">0 (0%)</td>
<td class="gt_row gt_center">1 (7.1%)</td></tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="4">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Median (IQR); n (%)
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table>
</div>

just include a few variables (this is important, tbl_summary gives you
medians rather than means by default for continuous variables, to get it
to give you means, include ths code below):

``` r
mtcars %>% 
  tbl_summary(by = cyl,
              include = c(cyl, mpg, vs, gear),
              statistic = list(all_continuous() ~ "{mean} ({sd})",
                               all_categorical() ~ "{n} / {N} ({p}%)")) %>% 
  add_p() %>% 
  modify_header(label ~ "**Key Variables**")
```

<div id="zkbnptwlku" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#zkbnptwlku .gt_table {
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

#zkbnptwlku .gt_heading {
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

#zkbnptwlku .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#zkbnptwlku .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#zkbnptwlku .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zkbnptwlku .gt_col_headings {
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

#zkbnptwlku .gt_col_heading {
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

#zkbnptwlku .gt_column_spanner_outer {
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

#zkbnptwlku .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#zkbnptwlku .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#zkbnptwlku .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#zkbnptwlku .gt_group_heading {
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

#zkbnptwlku .gt_empty_group_heading {
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

#zkbnptwlku .gt_from_md > :first-child {
  margin-top: 0;
}

#zkbnptwlku .gt_from_md > :last-child {
  margin-bottom: 0;
}

#zkbnptwlku .gt_row {
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

#zkbnptwlku .gt_stub {
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

#zkbnptwlku .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zkbnptwlku .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#zkbnptwlku .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zkbnptwlku .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#zkbnptwlku .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#zkbnptwlku .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zkbnptwlku .gt_footnotes {
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

#zkbnptwlku .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#zkbnptwlku .gt_sourcenotes {
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

#zkbnptwlku .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#zkbnptwlku .gt_left {
  text-align: left;
}

#zkbnptwlku .gt_center {
  text-align: center;
}

#zkbnptwlku .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#zkbnptwlku .gt_font_normal {
  font-weight: normal;
}

#zkbnptwlku .gt_font_bold {
  font-weight: bold;
}

#zkbnptwlku .gt_font_italic {
  font-style: italic;
}

#zkbnptwlku .gt_super {
  font-size: 65%;
}

#zkbnptwlku .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
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
    <tr><td class="gt_row gt_left">mpg</td>
<td class="gt_row gt_center">26.7 (4.5)</td>
<td class="gt_row gt_center">19.7 (1.5)</td>
<td class="gt_row gt_center">15.1 (2.6)</td>
<td class="gt_row gt_center"><0.001</td></tr>
    <tr><td class="gt_row gt_left">vs</td>
<td class="gt_row gt_center">10 / 11 (91%)</td>
<td class="gt_row gt_center">4 / 7 (57%)</td>
<td class="gt_row gt_center">0 / 14 (0%)</td>
<td class="gt_row gt_center"><0.001</td></tr>
    <tr><td class="gt_row gt_left">gear</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"><0.001</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">3</td>
<td class="gt_row gt_center">1 / 11 (9.1%)</td>
<td class="gt_row gt_center">2 / 7 (29%)</td>
<td class="gt_row gt_center">12 / 14 (86%)</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">4</td>
<td class="gt_row gt_center">8 / 11 (73%)</td>
<td class="gt_row gt_center">4 / 7 (57%)</td>
<td class="gt_row gt_center">0 / 14 (0%)</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">5</td>
<td class="gt_row gt_center">2 / 11 (18%)</td>
<td class="gt_row gt_center">1 / 7 (14%)</td>
<td class="gt_row gt_center">2 / 14 (14%)</td>
<td class="gt_row gt_center"></td></tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="5">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Mean (SD); n / N (%)
          <br />
        </p>
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>2</em>
          </sup>
           
          Kruskal-Wallis rank sum test; Fisher's exact test
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table>
</div>

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

<div id="zzdqontamt" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#zzdqontamt .gt_table {
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

#zzdqontamt .gt_heading {
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

#zzdqontamt .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#zzdqontamt .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#zzdqontamt .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zzdqontamt .gt_col_headings {
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

#zzdqontamt .gt_col_heading {
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

#zzdqontamt .gt_column_spanner_outer {
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

#zzdqontamt .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#zzdqontamt .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#zzdqontamt .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#zzdqontamt .gt_group_heading {
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

#zzdqontamt .gt_empty_group_heading {
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

#zzdqontamt .gt_from_md > :first-child {
  margin-top: 0;
}

#zzdqontamt .gt_from_md > :last-child {
  margin-bottom: 0;
}

#zzdqontamt .gt_row {
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

#zzdqontamt .gt_stub {
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

#zzdqontamt .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zzdqontamt .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#zzdqontamt .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zzdqontamt .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#zzdqontamt .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#zzdqontamt .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zzdqontamt .gt_footnotes {
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

#zzdqontamt .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#zzdqontamt .gt_sourcenotes {
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

#zzdqontamt .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#zzdqontamt .gt_left {
  text-align: left;
}

#zzdqontamt .gt_center {
  text-align: center;
}

#zzdqontamt .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#zzdqontamt .gt_font_normal {
  font-weight: normal;
}

#zzdqontamt .gt_font_bold {
  font-weight: bold;
}

#zzdqontamt .gt_font_italic {
  font-style: italic;
}

#zzdqontamt .gt_super {
  font-size: 65%;
}

#zzdqontamt .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
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
    <tr><td class="gt_row gt_left">MPG</td>
<td class="gt_row gt_center">26.7 (4.5)</td>
<td class="gt_row gt_center">19.7 (1.5)</td>
<td class="gt_row gt_center">15.1 (2.6)</td>
<td class="gt_row gt_center"><0.001</td></tr>
    <tr><td class="gt_row gt_left">VS</td>
<td class="gt_row gt_center">10 / 11 (91%)</td>
<td class="gt_row gt_center">4 / 7 (57%)</td>
<td class="gt_row gt_center">0 / 14 (0%)</td>
<td class="gt_row gt_center"><0.001</td></tr>
    <tr><td class="gt_row gt_left">GEAR</td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"></td>
<td class="gt_row gt_center"><0.001</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Three</td>
<td class="gt_row gt_center">1 / 11 (9.1%)</td>
<td class="gt_row gt_center">2 / 7 (29%)</td>
<td class="gt_row gt_center">12 / 14 (86%)</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Four</td>
<td class="gt_row gt_center">8 / 11 (73%)</td>
<td class="gt_row gt_center">4 / 7 (57%)</td>
<td class="gt_row gt_center">0 / 14 (0%)</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Five</td>
<td class="gt_row gt_center">2 / 11 (18%)</td>
<td class="gt_row gt_center">1 / 7 (14%)</td>
<td class="gt_row gt_center">2 / 14 (14%)</td>
<td class="gt_row gt_center"></td></tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="5">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          Mean (SD); n / N (%)
          <br />
        </p>
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>2</em>
          </sup>
           
          Kruskal-Wallis rank sum test; Fisher's exact test
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table>
</div>

## 3. Cleaning and tidying data

### dplyr::mutate()

#### case_when:

Simply assigning NAs will give you error and
[here](https://github.com/tidyverse/dplyr/issues/3202) is why and also a
great solution.

``` r
foo <- mtcars %>% 
  mutate(cyl = case_when(cyl == 4 ~ 1,
                         cyl == 6 ~ NA_real_,
                         TRUE ~ 0 ))
```

#### recode:

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
