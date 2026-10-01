# Coffee Prices Project Memory

## Project Goal
Exploratory data analysis followed by predictive modeling of coffee price per bag, 
using a hand-built dataset of specialty coffee offerings from top US roasters.

## Dataset
- **File**: `coffeelist1.xlsx`
- **Built by**: Drew Jones, by hand in Excel, December 23, 2024 – January 30, 2025
- **Source**: Listings from the websites of 15 US roasters appearing on Roastful's Top 100 Roasters list
- **Raw**: 167 rows, 20 columns
- **Cleaned** (single-origin only): 124 rows, stored as `coffee_clean`

## Key Variables
| Variable | Description |
|---|---|
| `roaster` | Name of the coffee roaster |
| `name` | Name of the coffee offering |
| `price` | Shelf price in USD |
| `bag_size` | Bag weight in oz |
| `adj_price` | Price normalized to 12 oz: `12 * (price / bag_size)` — **primary analysis variable** |
| `origin_state` | Origin country (single-origin coffees) |
| `origin_region` | Geographic sub-region within origin country |
| `single` | "yes" = single-origin, "no" = blend |
| `farm` | Farm name, farmer, or washing station |
| `importer` | Coffee importer (NA = roaster self-imported) |
| `process` | Processing method (Washed, Natural, Honey, Anaerobic variants, etc.) |
| `decaf` | "yes" = decaffeinated |
| `decaf_process` | Decaffeination method if applicable |
| `varietal1`–`varietal4` | Coffee varietals listed (most coffees have only one) |
| `blend_state1`–`blend_state3` | Origin countries for blends |
| `top100` | "yes" = roaster is on Roastful's Top 100 list |

## Known Data Issues
- 3 observations from **Botz Coffee** have missing `price` and `bag_size` (entire menu was sold out at collection time)
- `process` has some typos/inconsistencies: `"natural"` (should be `"Natural"`), `"Wahsed"` (should be `"Washed"`)
- Secondary varietal columns (`varietal2`–`varietal4`) are highly sparse by design — most coffees are single-variety

## Files
| File | Description |
|---|---|
| `coffeeprices.Rmd` | Original analysis file |
| `coffeeprices.qmd` | Quarto conversion of the Rmd — current working file |
| `coffee_pricing_24.qmd` | Earlier polished version of the analysis |
| `coffee_cline_experiment.qmd` | New EDA take with origin/process visualizations |
| `coffee_price_bonus.qmd` | Bonus analysis |
| `coffeelist1.xlsx` | Raw dataset |

## Standard Setup
```r
library(readxl)
library(tidyverse)
library(skimr)
library(ggthemes)
library(flextable)
library(naniar)

coffee <- read_excel("coffeelist1.xlsx", na = c("NA", "na", ""))
coffee <- coffee |>
  mutate(price = as.numeric(price), bag_size = as.numeric(bag_size))

coffee_clean <- coffee |>
  mutate(adj_price = 12 * (price / bag_size)) |>
  filter(single == "yes")
```
