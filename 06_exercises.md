---
title: 'Weekly Exercises 6'
author: "Will Brazgel"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(gifski)        # for creating the gif (don't need to load this library every time,but need it installed)
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
library(patchwork)     # for nicely combining ggplot2 graphs  
library(gt)            # for creating nice tables
library(rvest)         # for scraping data
library(robotstxt)     # for checking if you can scrape data
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Your first `shiny` app 

  1. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed.
  
[COVID app](https://wbrazgel.shinyapps.io/Exercise6-COVID/)
  
## Warm-up exercises from tutorial

  2. Read in the fake garden harvest data. Find the data [here](https://github.com/llendway/scraping_etc/blob/main/2020_harvest.csv) and click on the `Raw` button to get a direct link to the data.
  

```r
fake_2020_harvest <- read_csv("https://raw.githubusercontent.com/llendway/scraping_etc/main/2020_harvest.csv",
    col_types = cols(weight = col_number()),
    na = "MISSING",
    skip = 2) %>% 
  select(-X1)

View(fake_2020_harvest)
```

  3. Read in this [data](https://www.kaggle.com/heeraldedhia/groceries-dataset) from the kaggle website. You will need to download the data first. Save it to your project/repo folder. Do some quick checks of the data to assure it has been read in appropriately.
  

```r
Groceries_dataset <- read_csv("Groceries_dataset.csv")

View(Groceries_dataset)
```

  4. CHALLENGE(not graded): Write code to replicate the table shown below (open the .html file to see it) created from the `garden_harvest` data as best as you can. When you get to coloring the cells, I used the following line of code for the `colors` argument:

  5. Create a table using `gt` with data from your project or from the `garden_harvest` data if your project data aren't ready.
  

```r
garden_harvest %>% 
  group_by(vegetable) %>%
  mutate(weight_lb = weight*0.0022) %>% 
  summarise(total_weight_lb = sum(weight_lb)) %>%
  gt() %>% 
  tab_header(title = "2020 Garden Harvest Data")
```

<!--html_preserve--><style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#opdeakiwej .gt_table {
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

#opdeakiwej .gt_heading {
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

#opdeakiwej .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#opdeakiwej .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#opdeakiwej .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#opdeakiwej .gt_col_headings {
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

#opdeakiwej .gt_col_heading {
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

#opdeakiwej .gt_column_spanner_outer {
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

#opdeakiwej .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#opdeakiwej .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#opdeakiwej .gt_column_spanner {
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

#opdeakiwej .gt_group_heading {
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

#opdeakiwej .gt_empty_group_heading {
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

#opdeakiwej .gt_from_md > :first-child {
  margin-top: 0;
}

#opdeakiwej .gt_from_md > :last-child {
  margin-bottom: 0;
}

#opdeakiwej .gt_row {
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

#opdeakiwej .gt_stub {
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

#opdeakiwej .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#opdeakiwej .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#opdeakiwej .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#opdeakiwej .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#opdeakiwej .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#opdeakiwej .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#opdeakiwej .gt_footnotes {
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

#opdeakiwej .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#opdeakiwej .gt_sourcenotes {
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

#opdeakiwej .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#opdeakiwej .gt_left {
  text-align: left;
}

#opdeakiwej .gt_center {
  text-align: center;
}

#opdeakiwej .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#opdeakiwej .gt_font_normal {
  font-weight: normal;
}

#opdeakiwej .gt_font_bold {
  font-weight: bold;
}

#opdeakiwej .gt_font_italic {
  font-style: italic;
}

#opdeakiwej .gt_super {
  font-size: 65%;
}

#opdeakiwej .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="opdeakiwej" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="2" class="gt_heading gt_title gt_font_normal" style>2020 Garden Harvest Data</th>
    </tr>
    <tr>
      <th colspan="2" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style></th>
    </tr>
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">vegetable</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">total_weight_lb</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr>
      <td class="gt_row gt_left">apple</td>
      <td class="gt_row gt_right">0.3432</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">asparagus</td>
      <td class="gt_row gt_right">0.0440</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">basil</td>
      <td class="gt_row gt_right">1.0780</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">beans</td>
      <td class="gt_row gt_right">26.4638</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">beets</td>
      <td class="gt_row gt_right">13.6026</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">broccoli</td>
      <td class="gt_row gt_right">2.9458</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">carrots</td>
      <td class="gt_row gt_right">16.8300</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">chives</td>
      <td class="gt_row gt_right">0.0176</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">cilantro</td>
      <td class="gt_row gt_right">0.1144</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">corn</td>
      <td class="gt_row gt_right">12.9822</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">cucumbers</td>
      <td class="gt_row gt_right">43.5182</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">edamame</td>
      <td class="gt_row gt_right">6.0786</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">hot peppers</td>
      <td class="gt_row gt_right">1.4652</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">jalapeño</td>
      <td class="gt_row gt_right">9.8516</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">kale</td>
      <td class="gt_row gt_right">5.9334</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">kohlrabi</td>
      <td class="gt_row gt_right">0.4202</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">lettuce</td>
      <td class="gt_row gt_right">11.5720</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">onions</td>
      <td class="gt_row gt_right">4.0568</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">peas</td>
      <td class="gt_row gt_right">16.9906</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">peppers</td>
      <td class="gt_row gt_right">9.3236</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">potatoes</td>
      <td class="gt_row gt_right">23.8854</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">pumpkins</td>
      <td class="gt_row gt_right">154.3410</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">radish</td>
      <td class="gt_row gt_right">0.9438</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">raspberries</td>
      <td class="gt_row gt_right">1.8546</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">rutabaga</td>
      <td class="gt_row gt_right">29.6780</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">spinach</td>
      <td class="gt_row gt_right">2.0306</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">squash</td>
      <td class="gt_row gt_right">98.8174</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">strawberries</td>
      <td class="gt_row gt_right">1.3024</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">Swiss chard</td>
      <td class="gt_row gt_right">6.8684</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">tomatoes</td>
      <td class="gt_row gt_right">348.1082</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">zucchini</td>
      <td class="gt_row gt_right">99.4994</td>
    </tr>
  </tbody>
  
  
</table></div><!--/html_preserve-->

  
  6. Use `patchwork` operators and functions to combine at least two graphs using your project data or `garden_harvest` data if your project data aren't read.
  

```r
g1 <- garden_harvest %>%
  filter(vegetable %in% c("cucumbers", "beans", "zucchini"), date < mdy("10/1/2020")) %>%
  select(vegetable, weight, date) %>%
  group_by(vegetable, date) %>%
  mutate(weight_lb = weight*0.0022) %>% 
  summarise(total_weight = sum(weight_lb)) %>% 
  ggplot(aes(x = date, y = total_weight, color = vegetable, shape = vegetable))+
  geom_point()+
  geom_line()+
  labs(title = "Daily Growth in Weight (lb) 
       of Vegetables",
       x = "",
       y = "",
       color = "",
       shape = "")+
  scale_y_continuous(n.breaks = 10)+
  scale_colour_viridis_d()+
  theme_minimal()+
  theme(legend.position="top",
        panel.grid.minor.x = element_blank(),
        panel.grid.major.x = element_blank())

g2 <- garden_harvest %>%
  filter(vegetable %in% c("lettuce", "spinach", "kale"), date < mdy("10/1/2020")) %>%
  select(vegetable, weight, date) %>%
  group_by(vegetable, date) %>%
  mutate(weight_lb = weight*0.0022) %>% 
  summarise(total_weight = sum(weight_lb)) %>% 
  ggplot(aes(x = date, y = total_weight, color = vegetable, shape = vegetable))+
  geom_point()+
  geom_line()+
  labs(title = "Daily Growth in Weight (lb)
  of Vegetables",
       x = "",
       y = "",
       color = "",
       shape = "")+
  scale_y_continuous(n.breaks = 10)+
  scale_colour_viridis_d()+
  theme_minimal()+
  theme(legend.position="top",
        panel.grid.minor.x = element_blank(),
        panel.grid.major.x = element_blank())

g1 | g2 + 
  plot_annotation(title = "Daily Growth of Different Vegetables") 
```

![](06_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

  

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
