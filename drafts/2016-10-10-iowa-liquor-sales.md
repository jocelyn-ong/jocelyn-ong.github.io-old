---
tags:
  - DSI
  - collaboration
  - projects
---
{% include toc %}
Tagline: 1-liner summary of this post

## Introduction
This is Project 3 from the Data Science Immersive at General Assembly and we'll be working with the Iowa Liquor Sales dataset. This is also our first group project, and I'll be collaborating with [JP](jpfreeley.github.io) and [Joshua]().

For our project, we'll be looking at the data from a market research perspective:

> A liquor store owner in Iowa is looking to expand to new locations and has hired you to investigate the market data for potential new locations. The business owner is interested in the details of the best model you can fit to the data so that his team can evaluate potential locations for a new storefront.

## About the dataset

### How we obtained the data
The dataset we'll be using can be downloaded directly from the [data.iowa.gov](https://data.iowa.gov/Economy/Iowa-Liquor-Sales/m3tr-qhgy){:target:'_blank'} website. It's a large dataset and to facilitate working with the data, we'll be looking at this [dataset](https://drive.google.com/file/d/0Bx2SHQGVqWaseDB4QU9ZSVFDY2M/view?usp=sharing){:target='_blank'}, which is 10% of a filtered version of the original dataset.

On a side note, the slightly filtered version can be found [here version](https://www.dropbox.com/sh/pf5n5sgfgiri3i8/AACkaMeL_i_WgZ00rpxOOcysa?dl=0){:target='_blank'}.

### What's in the data
According to [data.iowa.gov](https://data.iowa.gov/Economy/Iowa-Liquor-Sales/m3tr-qhgy){:target:'_blank'}:

>This dataset contains the spirits purchase information of Iowa Class “E” liquor licensees by product and date of purchase...

>Class E liquor license, for grocery stores, liquor stores, convenience stores, etc., allows commercial establishments to sell liquor for off-premises consumption in original unopened containers.

Here's what the raw data looks like when we read it using Pandas:


|Column name |Data type |Description |
|---|---|---|
|Date |String |Date of liquor order |
|Store Number |Integer | Unique number assigned to each store |
|City |String |City where the store is located |
|Zip Code |String |Zip code where the store is located |
|County Number |Float |Iowa county number for the county where the store is located |
|County |String |County where the store is located |
|Category |Float |Category code associated with the liquor ordered |
|Category Name |String |Category of the liquor ordered |
|Vendor Number |Integer |The vendor number of the company for the brand of liquor ordered |
|Item Number |Integer |Item number for the individual liquor product ordered |
|Item Description |String |Description of the individual liquor product ordered |
|Bottle Volume (ml) |Integer |Volume of each liquor bottle in milliliters |
|State Bottle Cost |String |The amount that Alcoholic Beverages Division paid for each bottle of liquor ordered |
|State Bottle Retail |String |The amount the store paid for each bottle of liquor ordered |
|Bottles Sold |Integer |The number of bottles of liquor ordered by the store |
|Sale (Dollars) |String |Total cost of liquor order (number of bottles multiplied by the state bottle retail) |
|Volume Sold (Liters) |Float |Total volume of liquor ordered in liters (i.e. (Bottle Volume (ml) x Bottles Sold)/1,000) |
|Volume Sold (Gallons) |Float |Total volume of liquor ordered in gallons (i.e. (Bottle Volume (ml) x Bottles Sold)/3785.411784) |

## What do we want to do
To recap, we want to use the data to help a liquor store owner decide where are the best locations for a new storefront.

To do this, we would want to know:

- Annual sales volume (number of bottles) by location
- Monthly sales volume by location
- Liquor cost by location

## Data munging/ data wrangling
- Methods used
- Code?
- Go column by column
[![alt]({{ site.url }}{{ site.baseurl }}/images/bio-photo.jpg)]({{ site.url }}{{ site.baseurl }}/images/bio-photo.jpg)

## Modeling: generating the model and drawing conclusions

## Round up
