
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![Travis-CI Build
Status](https://travis-ci.org/rich-iannone/stationary.svg?branch=master)](https://travis-ci.org/rich-iannone/stationary)

# stationary <img src="man/figures/logo.svg" align="right" height="250px" />

Get hourly meteorological data from met stations all over the world.

## Examples

Let’s get some met data from La Guardia Airport\! (the ID value for that
one is `"725030-14732"`). This station has a pretty long history
(starting operations in 1973) but we’ll just grab data from the years of
2017 and 2018.

``` r
library(stationary)

met_data <- 
  get_isd_station_data(
    station_id = "725030-14732",
    startyear = 2017,
    endyear = 2018
  )
```

``` r
met_data
#> # A tibble: 28,254 x 9
#>    id    time                   wd    ws ceil_hgt  temp dew_point
#>    <chr> <dttm>              <int> <dbl>    <int> <dbl>     <dbl>
#>  1 7250… 2016-12-31 19:00:00   200   7.7       NA   6.7      -2.8
#>  2 7250… 2016-12-31 19:51:00   220   5.1     2743   6.7      -3.9
#>  3 7250… 2016-12-31 20:51:00   210   5.1     3048   6.7      -4.4
#>  4 7250… 2016-12-31 21:51:00   200   5.1     2591   6.7      -4.4
#>  5 7250… 2016-12-31 22:00:00   200   5.1       NA   6.7      -4.4
#>  6 7250… 2016-12-31 22:51:00   210   5.1     2134   7.2      -4.4
#>  7 7250… 2016-12-31 23:51:00   230   5.7     2286   7.2      -4.4
#>  8 7250… 2016-12-31 23:59:00    NA  NA         NA  NA        NA  
#>  9 7250… 2016-12-31 23:59:00    NA  NA         NA  NA        NA  
#> 10 7250… 2017-01-01 00:51:00   230   4.6     1402   7.8      -3.9
#> # … with 28,244 more rows, and 2 more variables: atmos_pres <dbl>,
#> #   rh <dbl>
```

There are lots of stations and we at least need an identifier to access
met data. We can examine station metadata using the
`get_station_metadata()` function (which has those ID values in the
first column). Let’s get all of the stations located in Norway.

``` r
stations_norway <- 
  get_station_metadata() %>%
  filter(country == "NO")

stations_norway
#> # A tibble: 405 x 15
#>    id    usaf  wban  name  country state icao    lat   lon  elev begin_date
#>    <chr> <chr> <chr> <chr> <chr>   <chr> <chr> <dbl> <dbl> <dbl> <date>    
#>  1 0100… 0100… 99999 BOGU… NO      <NA>  ENRS   NA   NA     NA   2001-09-27
#>  2 0100… 0100… 99999 JAN … NO      <NA>  ENJA   70.9 -8.67   9   1931-01-01
#>  3 0100… 0100… 99999 ROST  NO      <NA>  <NA>   NA   NA     NA   1986-11-20
#>  4 0100… 0100… 99999 SORS… NO      <NA>  ENSO   59.8  5.34  48.8 1986-11-20
#>  5 0100… 0100… 99999 BRIN… NO      <NA>  <NA>   61.4  5.87 327   1987-01-17
#>  6 0100… 0100… 99999 RORV… NO      <NA>  <NA>   64.8 11.2   14   1987-01-16
#>  7 0100… 0100… 99999 FRIGG NO      <NA>  ENFR   60.0  2.25  48   1988-03-20
#>  8 0100… 0100… 99999 VERL… NO      <NA>  <NA>   80.0 16.2    8   1986-11-09
#>  9 0100… 0100… 99999 HORN… NO      <NA>  <NA>   77   15.5   12   1985-06-01
#> 10 0100… 0100… 99999 NY-A… NO      <NA>  ENAS   78.9 11.9    8   1973-01-01
#> # … with 395 more rows, and 4 more variables: end_date <date>,
#> #   begin_year <int>, end_year <int>, tz_name <chr>
```

This table can be greatly reduced to isolate the stations of interest.
For example, another use of `dplyr::filter()` could be used to get only
high-altitude stations (above 1000 meters).

``` r
norway_high_elev <-
  stations_norway %>% 
  dplyr::filter(elev > 1000)

norway_high_elev
#> # A tibble: 12 x 15
#>    id    usaf  wban  name  country state icao    lat   lon  elev begin_date
#>    <chr> <chr> <chr> <chr> <chr>   <chr> <chr> <dbl> <dbl> <dbl> <date>    
#>  1 0122… 0122… 99999 MANN… NO      <NA>  <NA>   62.4  7.77 1294  2010-03-15
#>  2 0123… 0123… 99999 HJER… NO      <NA>  <NA>   62.2  9.55 1012  2010-09-07
#>  3 0134… 0134… 99999 MIDT… NO      <NA>  <NA>   60.6  7.27 1162  2011-11-25
#>  4 0135… 0135… 99999 FINS… NO      <NA>  <NA>   60.6  7.53 1208  2003-03-30
#>  5 0135… 0135… 99999 FINS… NO      <NA>  <NA>   60.6  7.5  1224  1973-01-02
#>  6 0135… 0135… 99999 SAND… NO      <NA>  <NA>   60.2  7.48 1250  2004-01-07
#>  7 0136… 0136… 99999 JUVV… NO      <NA>  <NA>   61.7  8.37 1894  2009-06-26
#>  8 0136… 0136… 99999 SOGN… NO      <NA>  <NA>   61.6  8    1413  1979-03-01
#>  9 0137… 0137… 99999 KVIT… NO      <NA>  <NA>   61.5 10.1  1028  1973-01-01
#> 10 0143… 0143… 99999 MIDT… NO      <NA>  <NA>   59.8  6.98 1081  1973-01-01
#> 11 0144… 0144… 99999 BLAS… NO      <NA>  <NA>   59.3  6.87 1105. 1973-01-01
#> 12 0146… 0146… 99999 GAUS… NO      <NA>  <NA>   59.8  8.65 1804. 2014-06-05
#> # … with 4 more variables: end_date <date>, begin_year <int>,
#> #   end_year <int>, tz_name <chr>
```

The station IDs from the tibble can be transformed into a vector of
station IDs with `dplyr::pull()`.

``` r
norway_high_elev %>% pull(id)
#>  [1] "012200-99999" "012390-99999" "013460-99999" "013500-99999"
#>  [5] "013510-99999" "013520-99999" "013620-99999" "013660-99999"
#>  [9] "013750-99999" "014330-99999" "014400-99999" "014611-99999"
```

Suppose you’d like to collect several years of met data from a
particular station and get only a listing of parameters that meet some
criterion. Here’s an example of obtaining temperatures above 15 degrees
Celsius from the high-altitude `"JUVVASSHOE"` station in Norway:

``` r
station_data <- 
  get_station_metadata() %>%
  filter(name == "JUVVASSHOE") %>%
  pull(id) %>%
  get_isd_station_data(startyear = 2011, endyear = 2019)

high_temp_data <-
  station_data %>%
  select(time, wd, ws, temp) %>% 
  filter(temp > 16) %>%
  mutate(temp_f = (temp * (9/5)) + 32) %>%
  arrange(desc(temp_f))
```

``` r
high_temp_data
#> # A tibble: 50 x 5
#>    time                   wd    ws  temp temp_f
#>    <dttm>              <int> <dbl> <dbl>  <dbl>
#>  1 2019-07-26 15:00:00   160     5  18.5   65.3
#>  2 2019-07-26 17:00:00   210     3  18.4   65.1
#>  3 2019-07-26 18:00:00   180     2  18.3   64.9
#>  4 2019-07-26 16:00:00   180     4  18.2   64.8
#>  5 2014-07-23 16:00:00   270     2  17.6   63.7
#>  6 2019-07-26 14:00:00   150     4  17.5   63.5
#>  7 2014-07-23 17:00:00   300     4  17.3   63.1
#>  8 2019-07-28 16:00:00   130     6  17.3   63.1
#>  9 2014-07-23 18:00:00   280     3  17.2   63.0
#> 10 2018-07-04 15:00:00   340     2  17.2   63.0
#> # … with 40 more rows
```

There can actually be a lot of additional met data beyond wind speed,
temperatures, etc. It can vary greatly depending on the selected
station. These additional categories are denoted ‘two-letter + digit’
identifiers (e.g., `AA1`, `GA1`, etc.). Within each category are
numerous variables (coded as `[identifer]_[index]`). Here is the
complete list of the different
parameters:

| Category | Identifier | Column Name                                                               |
| -------- | ---------- | ------------------------------------------------------------------------- |
| `AA1`    | `aa1_1`    | `aa1_liq_precip_period_quantity`                                          |
| `AA1`    | `aa1_2`    | `aa1_liq_precip_depth_dimension`                                          |
| `AA1`    | `aa1_3`    | `aa1_liq_precip_condition_code`                                           |
| `AB1`    | `ab1_1`    | `aa1_liq_precip_quality_code`                                             |
| `AB1`    | `ab1_2`    | `ab1_liq_precip_monthly_depth_dimension`                                  |
| `AB1`    | `ab1_3`    | `ab1_liq_precip_monthly_condition_code`                                   |
| `AB1`    | `ab1_4`    | `ab1_liq_precip_monthly_quality_code`                                     |
| `AC1`    | `ac1_1`    | `ac1_precip_obs_history_duration_code`                                    |
| `AC1`    | `ac1_2`    | `ac1_precip_obs_history_characteristic_code`                              |
| `AC1`    | `ac1_3`    | `ac1_precip_obs_history_quality_code`                                     |
| `AD1`    | `ad1_1`    | `ad1_liq_precip_greatest_amt_24h_month_depth_dimension`                   |
| `AD1`    | `ad1_2`    | `ad1_liq_precip_greatest_amt_24h_month_condition_code`                    |
| `AD1`    | `ad1_3`    | `ad1_liq_precip_greatest_amt_24h_month_dates`                             |
| `AD1`    | `ad1_4`    | `ad1_liq_precip_greatest_amt_24h_month_quality_code`                      |
| `AE1`    | `ae1_1`    | `ae1_liq_precip_number_days_amt_month__01inch`                            |
| `AE1`    | `ae1_2`    | `ae1_liq_precip_number_days_amt_month__01inch_quality_code`               |
| `AE1`    | `ae1_3`    | `ae1_liq_precip_number_days_amt_month__10inch`                            |
| `AE1`    | `ae1_4`    | `ae1_liq_precip_number_days_amt_month__10inch_quality_code`               |
| `AE1`    | `ae1_5`    | `ae1_liq_precip_number_days_amt_month__50inch`                            |
| `AE1`    | `ae1_6`    | `ae1_liq_precip_number_days_amt_month__50inch_quality_code`               |
| `AE1`    | `ae1_7`    | `ae1_liq_precip_number_days_amt_month_1_00inch`                           |
| `AE1`    | `ae1_8`    | `ae1_liq_precip_number_days_amt_month_1_00inch_quality_code`              |
| `AG1`    | `ag1_1`    | `ag1_precip_est_obs_discrepancy_code`                                     |
| `AG1`    | `ag1_2`    | `ag1_precip_est_obs_est_water_depth_dimension`                            |
| `AH1`    | `ah1_3`    | `ah1_liq_precip_max_short_dur_month_period_quantity`                      |
| `AH1`    | `ah1_4`    | `ah1_liq_precip_max_short_dur_month_depth_dimension`                      |
| `AH1`    | `ah1_5`    | `ah1_liq_precip_max_short_dur_month_condition_code`                       |
| `AH1`    | `ah1_6`    | `ah1_liq_precip_max_short_dur_month_end_date_time`                        |
| `AH1`    | `ah1_7`    | `ah1_liq_precip_max_short_dur_month_quality_code`                         |
| `AI1`    | `ai1_1`    | `ai1_liq_precip_max_short_dur_month_period_quantity`                      |
| `AI1`    | `ai1_2`    | `ai1_liq_precip_max_short_dur_month_depth_dimension`                      |
| `AI1`    | `ai1_3`    | `ai1_liq_precip_max_short_dur_month_condition_code`                       |
| `AI1`    | `ai1_4`    | `ai1_liq_precip_max_short_dur_month_end_date_time`                        |
| `AI1`    | `ai1_5`    | `ai1_liq_precip_max_short_dur_month_quality_code`                         |
| `AJ1`    | `aj1_1`    | `aj1_snow_depth_dimension`                                                |
| `AJ1`    | `aj1_2`    | `aj1_snow_depth_condition_code`                                           |
| `AJ1`    | `aj1_3`    | `aj1_snow_depth_quality_code`                                             |
| `AJ1`    | `aj1_4`    | `aj1_snow_depth_equiv_water_depth_dimension`                              |
| `AJ1`    | `aj1_5`    | `aj1_snow_depth_equiv_water_condition_code`                               |
| `AJ1`    | `aj1_6`    | `aj1_snow_depth_equiv_water_quality_code`                                 |
| `AK1`    | `ad1_1`    | `ak1_snow_depth_greatest_depth_month_depth_dimension`                     |
| `AK1`    | `ad1_2`    | `ak1_snow_depth_greatest_depth_month_condition_code`                      |
| `AK1`    | `ak1_3`    | `ak1_snow_depth_greatest_depth_month_dates_occurrence`                    |
| `AK1`    | `ak1_4`    | `ak1_snow_depth_greatest_depth_month_quality_code`                        |
| `AL1`    | `al1_1`    | `al1_snow_accumulation_period_quantity`                                   |
| `AL1`    | `al1_2`    | `al1_snow_accumulation_depth_dimension`                                   |
| `AL1`    | `al1_3`    | `al1_snow_accumulation_condition_code`                                    |
| `AL1`    | `al1_4`    | `al1_snow_accumulation_quality_code`                                      |
| `AM1`    | `am1_1`    | `am1_snow_accumulation_greatest_amt_24h_month_depth_dimension`            |
| `AM1`    | `am1_2`    | `am1_snow_accumulation_greatest_amt_24h_month_condition_code`             |
| `AM1`    | `am1_3`    | `am1_snow_accumulation_greatest_amt_24h_month_dates_occurrence_1`         |
| `AM1`    | `am1_4`    | `am1_snow_accumulation_greatest_amt_24h_month_dates_occurrence_2`         |
| `AM1`    | `am1_5`    | `am1_snow_accumulation_greatest_amt_24h_month_dates_occurrence_3`         |
| `AM1`    | `am1_6`    | `am1_snow_accumulation_greatest_amt_24h_month_quality_code`               |
| `AN1`    | `an1_1`    | `an1_snow_accumulation_month_period_quantity`                             |
| `AN1`    | `an1_2`    | `an1_snow_accumulation_month_depth_dimension`                             |
| `AN1`    | `an1_3`    | `an1_snow_accumulation_month_condition_code`                              |
| `AN1`    | `an1_4`    | `an1_snow_accumulation_month_quality_code`                                |
| `AO1`    | `ao1_1`    | `ao1_liq_precip_period_quantity_minutes`                                  |
| `AO1`    | `ao1_2`    | `ao1_liq_precip_depth_dimension`                                          |
| `AO1`    | `ao1_3`    | `ao1_liq_precip_condition_code`                                           |
| `AO1`    | `ao1_4`    | `ao1_liq_precip_quality_code`                                             |
| `AP1`    | `ap1_1`    | `ap1_15_min_liq_precip_hpd_gauge_value_45_min_prior`                      |
| `AP1`    | `ap1_2`    | `ap1_15_min_liq_precip_hpd_gauge_value_30_min_prior`                      |
| `AP1`    | `ap1_3`    | `ap1_15_min_liq_precip_hpd_gauge_value_15_min_prior`                      |
| `AP1`    | `ap1_4`    | `ap1_15_min_liq_precip_hpd_gauge_value_at_obs_time`                       |
| `AU1`    | `au1_1`    | `au1_present_weather_obs_intensity_code`                                  |
| `AU1`    | `au1_2`    | `au1_present_weather_obs_descriptor_code`                                 |
| `AU1`    | `au1_3`    | `au1_present_weather_obs_precipitation_code`                              |
| `AU1`    | `au1_4`    | `au1_present_weather_obs_obscuration_code`                                |
| `AU1`    | `au1_5`    | `au1_present_weather_obs_other_weather_phenomena_code`                    |
| `AU1`    | `au1_6`    | `au1_present_weather_obs_combination_indicator_code`                      |
| `AU1`    | `au1_7`    | `au1_present_weather_obs_quality_code`                                    |
| `AW1`    | `aw1_1`    | `aw1_present_weather_obs_aut_weather_report_1`                            |
| `AW1`    | `aw1_2`    | `aw1_present_weather_obs_aut_weather_report_2`                            |
| `AW1`    | `aw1_3`    | `aw1_present_weather_obs_aut_weather_report_3`                            |
| `AW1`    | `aw1_4`    | `aw1_present_weather_obs_aut_weather_report_4`                            |
| `AX1`    | `ax1_1`    | `ax1_past_weather_obs_atmos_condition_code`                               |
| `AX1`    | `ax1_2`    | `ax1_past_weather_obs_quality_manual_atmos_condition_code`                |
| `AX1`    | `ax1_3`    | `ax1_past_weather_obs_period_quantity`                                    |
| `AX1`    | `ax1_4`    | `ax1_past_weather_obs_period_quality_code`                                |
| `AY1`    | `ay1_1`    | `ay1_past_weather_obs_manual_occurrence_identifier`                       |
| `AY1`    | `ay1_2`    | `ay1_past_weather_obs_quality_manual_atmos_condition_code`                |
| `AY1`    | `ay1_3`    | `ay1_past_weather_obs_period_quantity`                                    |
| `AY1`    | `ay1_4`    | `ay1_past_weather_obs_period_quality_code`                                |
| `AZ1`    | `az1_1`    | `az1_past_weather_obs_aut_occurrence_identifier`                          |
| `AZ1`    | `az1_2`    | `az1_past_weather_obs_quality_aut_atmos_condition_code`                   |
| `AZ1`    | `az1_3`    | `az1_past_weather_obs_period_quantity`                                    |
| `AZ1`    | `az1_4`    | `az1_past_weather_obs_period_quality_code`                                |
| `CB1`    | `cb1_1`    | `cb1_subhrly_obs_liq_precip_2_sensor_period_quantity`                     |
| `CB1`    | `cb1_2`    | `cb1_subhrly_obs_liq_precip_2_sensor_precip_liq_depth`                    |
| `CB1`    | `cb1_3`    | `cb1_subhrly_obs_liq_precip_2_sensor_qc_quality_code`                     |
| `CB1`    | `cb1_4`    | `cb1_subhrly_obs_liq_precip_2_sensor_flag_quality_code`                   |
| `CF1`    | `cf1_1`    | `cf1_hrly_fan_speed_rate`                                                 |
| `CF1`    | `cf1_2`    | `cf1_hrly_fan_qc_quality_code`                                            |
| `CF1`    | `cf1_3`    | `cf1_hrly_fan_flag_quality_code`                                          |
| `CG1`    | `cg1_1`    | `cg1_subhrly_obs_liq_precip_1_sensor_precip_liq_depth`                    |
| `CG1`    | `cg1_2`    | `cg1_subhrly_obs_liq_precip_1_sensor_qc_quality_code`                     |
| `CG1`    | `cg1_3`    | `cg1_subhrly_obs_liq_precip_1_sensor_flag_quality_code`                   |
| `CH1`    | `ch1_1`    | `ch1_hrly_subhrly_rh_temp_period_quantity`                                |
| `CH1`    | `ch1_2`    | `ch1_hrly_subhrly_temp_avg_air_temp`                                      |
| `CH1`    | `ch1_3`    | `ch1_hrly_subhrly_temp_qc_quality_code`                                   |
| `CH1`    | `ch1_4`    | `ch1_hrly_subhrly_temp_flag_quality_code`                                 |
| `CH1`    | `ch1_5`    | `ch1_hrly_subhrly_rh_avg_rh`                                              |
| `CH1`    | `ch1_6`    | `ch1_hrly_subhrly_rh_qc_quality_code`                                     |
| `CH1`    | `ch1_7`    | `ch1_hrly_subhrly_rh_flag_quality_code`                                   |
| `CI1`    | `ci1_1`    | `ci1_hrly_rh_temp_min_hrly_temp`                                          |
| `CI1`    | `ci1_2`    | `ci1_hrly_rh_temp_min_hrly_temp_qc_quality_code`                          |
| `CI1`    | `ci1_3`    | `ci1_hrly_rh_temp_min_hrly_temp_flag_quality_code`                        |
| `CI1`    | `ci1_4`    | `ci1_hrly_rh_temp_max_hrly_temp`                                          |
| `CI1`    | `ci1_5`    | `ci1_hrly_rh_temp_max_hrly_temp_qc_quality_code`                          |
| `CI1`    | `ci1_6`    | `ci1_hrly_rh_temp_max_hrly_temp_flag_quality_code`                        |
| `CI1`    | `ci1_7`    | `ci1_hrly_rh_temp_std_dev_hrly_temp`                                      |
| `CI1`    | `ci1_8`    | `ci1_hrly_rh_temp_std_dev_hrly_temp_qc_quality_code`                      |
| `CI1`    | `ci1_9`    | `ci1_hrly_rh_temp_std_dev_hrly_temp_flag_quality_code`                    |
| `CI1`    | `ci1_10`   | `ci1_hrly_rh_temp_std_dev_hrly_rh`                                        |
| `CI1`    | `ci1_11`   | `ci1_hrly_rh_temp_std_dev_hrly_rh_qc_quality_code`                        |
| `CI1`    | `ci1_12`   | `ci1_hrly_rh_temp_std_dev_hrly_rh_flag_quality_code`                      |
| `CN1`    | `cn1_1`    | `cn1_hrly_batvol_sensors_transm_avg_voltage`                              |
| `CN1`    | `cn1_2`    | `cn1_hrly_batvol_sensors_transm_avg_voltage_qc_quality_code`              |
| `CN1`    | `cn1_3`    | `cn1_hrly_batvol_sensors_transm_avg_voltage_flag_quality_code`            |
| `CN1`    | `cn1_4`    | `cn1_hrly_batvol_full_load_avg_voltage`                                   |
| `CN1`    | `cn1_5`    | `cn1_hrly_batvol_full_load_avg_voltage_qc_quality_code`                   |
| `CN1`    | `cn1_6`    | `cn1_hrly_batvol_full_load_avg_voltage_flag_quality_code`                 |
| `CN1`    | `cn1_7`    | `cn1_hrly_batvol_datalogger_avg_voltage`                                  |
| `CN1`    | `cn1_8`    | `cn1_hrly_batvol_datalogger_avg_voltage_qc_quality_code`                  |
| `CN1`    | `cn1_9`    | `cn1_hrly_batvol_datalogger_avg_voltage_flag_quality_code`                |
| `CN2`    | `cn2_1`    | `cn2_hrly_diagnostic_equipment_temp`                                      |
| `CN2`    | `cn2_2`    | `cn2_hrly_diagnostic_equipment_temp_qc_quality_code`                      |
| `CN2`    | `cn2_3`    | `cn2_hrly_diagnostic_equipment_temp_flag_quality_code`                    |
| `CN2`    | `cn2_4`    | `cn2_hrly_diagnostic_geonor_inlet_temp`                                   |
| `CN2`    | `cn2_5`    | `cn2_hrly_diagnostic_geonor_inlet_temp_qc_quality_code`                   |
| `CN2`    | `cn2_6`    | `cn2_hrly_diagnostic_geonor_inlet_temp_flag_quality_code`                 |
| `CN2`    | `cn2_7`    | `cn2_hrly_diagnostic_datalogger_opendoor_time`                            |
| `CN2`    | `cn2_8`    | `cn2_hrly_diagnostic_datalogger_opendoor_time_qc_quality_code`            |
| `CN2`    | `cn2_9`    | `cn2_hrly_diagnostic_datalogger_opendoor_time_flag_quality_code`          |
| `CN3`    | `cn3_1`    | `cn3_hrly_diagnostic_reference_resistor_avg_resistance`                   |
| `CN3`    | `cn3_2`    | `cn3_hrly_diagnostic_reference_resistor_avg_resistance_qc_quality_code`   |
| `CN3`    | `cn3_3`    | `cn3_hrly_diagnostic_reference_resistor_avg_resistance_flag_quality_code` |
| `CN3`    | `cn3_4`    | `cn3_hrly_diagnostic_datalogger_signature_id`                             |
| `CN3`    | `cn3_5`    | `cn3_hrly_diagnostic_datalogger_signature_id_qc_quality_code`             |
| `CN3`    | `cn3_6`    | `cn3_hrly_diagnostic_datalogger_signature_id_flag_quality_code`           |
| `CN4`    | `cn4_1`    | `cn4_hrly_diagnostic_liq_precip_gauge_flag_bit`                           |
| `CN4`    | `cn4_2`    | `cn4_hrly_diagnostic_liq_precip_gauge_flag_bit_qc_quality_code`           |
| `CN4`    | `cn4_3`    | `cn4_hrly_diagnostic_liq_precip_gauge_flag_bit_flag_quality_code`         |
| `CN4`    | `cn4_4`    | `cn4_hrly_diagnostic_doorflag_field`                                      |
| `CN4`    | `cn4_5`    | `cn4_hrly_diagnostic_doorflag_field_qc_quality_code`                      |
| `CN4`    | `cn4_6`    | `cn4_hrly_diagnostic_doorflag_field_flag_quality_code`                    |
| `CN4`    | `cn4_7`    | `cn4_hrly_diagnostic_forward_transmitter_rf_power`                        |
| `CN4`    | `cn4_8`    | `cn4_hrly_diagnostic_forward_transmitter_rf_power_qc_quality_code`        |
| `CN4`    | `cn4_9`    | `cn4_hrly_diagnostic_forward_transmitter_rf_power_flag_quality_code`      |
| `CN4`    | `cn4_10`   | `cn4_hrly_diagnostic_reflected_transmitter_rf_power`                      |
| `CN4`    | `cn4_11`   | `cn4_hrly_diagnostic_reflected_transmitter_rf_power_qc_quality_code`      |
| `CN4`    | `cn4_12`   | `cn4_hrly_diagnostic_reflected_transmitter_rf_power_flag_quality_code`    |
| `CR1`    | `cr1_1`    | `cr1_control_section_datalogger_version_number`                           |
| `CR1`    | `cr1_2`    | `cr1_control_section_datalogger_version_number_qc_quality_code`           |
| `CR1`    | `cr1_3`    | `cr1_control_section_datalogger_version_number_flag_quality_code`         |
| `CT1`    | `ct1_1`    | `ct1_subhrly_temp_avg_air_temp`                                           |
| `CT1`    | `ct1_2`    | `ct1_subhrly_temp_avg_air_temp_qc_quality_code`                           |
| `CT1`    | `ct1_3`    | `ct1_subhrly_temp_avg_air_temp_flag_quality_code`                         |
| `CU1`    | `cu1_1`    | `cu1_hrly_temp_avg_air_temp`                                              |
| `CU1`    | `cu1_2`    | `cu1_hrly_temp_avg_air_temp_qc_quality_code`                              |
| `CU1`    | `cu1_3`    | `cu1_hrly_temp_avg_air_temp_flag_quality_code`                            |
| `CU1`    | `cu1_4`    | `cu1_hrly_temp_avg_air_temp_st_dev`                                       |
| `CU1`    | `cu1_5`    | `cu1_hrly_temp_avg_air_temp_st_dev_qc_quality_code`                       |
| `CU1`    | `cu1_6`    | `cu1_hrly_temp_avg_air_temp_st_dev_flag_quality_code`                     |
| `CV1`    | `cv1_1`    | `cv1_hrly_temp_min_air_temp`                                              |
| `CV1`    | `cv1_2`    | `cv1_hrly_temp_min_air_temp_qc_quality_code`                              |
| `CV1`    | `cv1_3`    | `cv1_hrly_temp_min_air_temp_flag_quality_code`                            |
| `CV1`    | `cv1_4`    | `cv1_hrly_temp_min_air_temp_time`                                         |
| `CV1`    | `cv1_5`    | `cv1_hrly_temp_min_air_temp_time_qc_quality_code`                         |
| `CV1`    | `cv1_6`    | `cv1_hrly_temp_min_air_temp_time_flag_quality_code`                       |
| `CV1`    | `cv1_7`    | `cv1_hrly_temp_max_air_temp`                                              |
| `CV1`    | `cv1_8`    | `cv1_hrly_temp_max_air_temp_qc_quality_code`                              |
| `CV1`    | `cv1_9`    | `cv1_hrly_temp_max_air_temp_flag_quality_code`                            |
| `CV1`    | `cv1_10`   | `cv1_hrly_temp_max_air_temp_time`                                         |
| `CV1`    | `cv1_11`   | `cv1_hrly_temp_max_air_temp_time_qc_quality_code`                         |
| `CV1`    | `cv1_12`   | `cv1_hrly_temp_max_air_temp_time_flag_quality_code`                       |
| `CW1`    | `cw1_1`    | `cw1_subhrly_wetness_wet1_indicator`                                      |
| `CW1`    | `cw1_2`    | `cw1_subhrly_wetness_wet1_indicator_qc_quality_code`                      |
| `CW1`    | `cw1_3`    | `cw1_subhrly_wetness_wet1_indicator_flag_quality_code`                    |
| `CW1`    | `cw1_4`    | `cw1_subhrly_wetness_wet2_indicator`                                      |
| `CW1`    | `cw1_5`    | `cw1_subhrly_wetness_wet2_indicator_qc_quality_code`                      |
| `CW1`    | `cw1_6`    | `cw1_subhrly_wetness_wet2_indicator_flag_quality_code`                    |
| `CX1`    | `cx1_1`    | `cx1_hourly_geonor_vib_wire_total_precip`                                 |
| `CX1`    | `cx1_2`    | `cx1_hourly_geonor_vib_wire_total_precip_qc_quality_code`                 |
| `CX1`    | `cx1_3`    | `cx1_hourly_geonor_vib_wire_total_precip_flag_quality_code`               |
| `CX1`    | `cx1_4`    | `cx1_hourly_geonor_vib_wire_freq_avg_precip`                              |
| `CX1`    | `cx1_5`    | `cx1_hourly_geonor_vib_wire_freq_avg_precip_qc_quality_code`              |
| `CX1`    | `cx1_6`    | `cx1_hourly_geonor_vib_wire_freq_avg_precip_flag_quality_code`            |
| `CX1`    | `cx1_7`    | `cx1_hourly_geonor_vib_wire_freq_min_precip`                              |
| `CX1`    | `cx1_8`    | `cx1_hourly_geonor_vib_wire_freq_min_precip_qc_quality_code`              |
| `CX1`    | `cx1_9`    | `cx1_hourly_geonor_vib_wire_freq_min_precip_flag_quality_code`            |
| `CX1`    | `cx1_10`   | `cx1_hourly_geonor_vib_wire_freq_max_precip`                              |
| `CX1`    | `cx1_11`   | `cx1_hourly_geonor_vib_wire_freq_max_precip_qc_quality_code`              |
| `CX1`    | `cx1_12`   | `cx1_hourly_geonor_vib_wire_freq_max_precip_flag_quality_code`            |
| `CO1`    | `co1_1`    | `co1_network_metadata_climate_division_number`                            |
| `CO1`    | `co1_2`    | `co1_network_metadata_utc_lst_time_conversion`                            |
| `CO1`    | `co1_3`    | `co2_us_network_cooperative_element_id`                                   |
| `CO1`    | `co1_4`    | `co2_us_network_cooperative_time_offset`                                  |
| `ED1`    | `ed1_1`    | `ed1_runway_vis_range_obs_direction_angle`                                |
| `ED1`    | `ed1_2`    | `ed1_runway_vis_range_obs_runway_designator_code`                         |
| `ED1`    | `ed1_3`    | `ed1_runway_vis_range_obs_vis_dimension`                                  |
| `ED1`    | `ed1_4`    | `ed1_runway_vis_range_obs_quality_code`                                   |
| `GA1`    | `ga1_1`    | `ga1_sky_cover_layer_coverage_code`                                       |
| `GA1`    | `ga1_2`    | `ga1_sky_cover_layer_coverage_quality_code`                               |
| `GA1`    | `ga1_3`    | `ga1_sky_cover_layer_base_height`                                         |
| `GA1`    | `ga1_4`    | `ga1_sky_cover_layer_base_height_quality_code`                            |
| `GA1`    | `ga1_5`    | `ga1_sky_cover_layer_cloud_type`                                          |
| `GA1`    | `ga1_6`    | `ga1_sky_cover_layer_cloud_type_quality_code`                             |
| `GD1`    | `gd1_1`    | `gd1_sky_cover_summation_state_coverage_1`                                |
| `GD1`    | `gd1_2`    | `gd1_sky_cover_summation_state_coverage_2`                                |
| `GD1`    | `gd1_3`    | `gd1_sky_cover_summation_state_coverage_quality_code`                     |
| `GD1`    | `gd1_4`    | `gd1_sky_cover_summation_state_height`                                    |
| `GD1`    | `gd1_5`    | `gd1_sky_cover_summation_state_height_quality_code`                       |
| `GD1`    | `gd1_6`    | `gd1_sky_cover_summation_state_characteristic_code`                       |
| `GF1`    | `gf1_1`    | `gf1_sky_condition_obs_total_coverage`                                    |
| `GF1`    | `gf1_2`    | `gf1_sky_condition_obs_total_opaque_coverage`                             |
| `GF1`    | `gf1_3`    | `gf1_sky_condition_obs_total_coverage_quality_code`                       |
| `GF1`    | `gf1_4`    | `gf1_sky_condition_obs_total_lowest_cloud_cover`                          |
| `GF1`    | `gf1_5`    | `gf1_sky_condition_obs_total_lowest_cloud_cover_quality_code`             |
| `GF1`    | `gf1_6`    | `gf1_sky_condition_obs_low_cloud_genus`                                   |
| `GF1`    | `gf1_7`    | `gf1_sky_condition_obs_low_cloud_genus_quality_code`                      |
| `GF1`    | `gf1_8`    | `gf1_sky_condition_obs_lowest_cloud_base_height`                          |
| `GF1`    | `gf1_9`    | `gf1_sky_condition_obs_lowest_cloud_base_height_quality_code`             |
| `GF1`    | `gf1_10`   | `gf1_sky_condition_obs_mid_cloud_genus`                                   |
| `GF1`    | `gf1_11`   | `gf1_sky_condition_obs_mid_cloud_genus_quality_code`                      |
| `GF1`    | `gf1_12`   | `gf1_sky_condition_obs_high_cloud_genus`                                  |
| `GF1`    | `gf1_13`   | `gf1_sky_condition_obs_high_cloud_genus_quality_code`                     |
| `GG1`    | `gg1_1`    | `gg1_below_stn_cloud_layer_coverage`                                      |
| `GG1`    | `gg1_2`    | `gg1_below_stn_cloud_layer_coverage_quality_code`                         |
| `GG1`    | `gg1_3`    | `gg1_below_stn_cloud_layer_top_height`                                    |
| `GG1`    | `gg1_4`    | `gg1_below_stn_cloud_layer_top_height_quality_code`                       |
| `GG1`    | `gg1_5`    | `gg1_below_stn_cloud_layer_type`                                          |
| `GG1`    | `gg1_6`    | `gg1_below_stn_cloud_layer_type_quality_code`                             |
| `GG1`    | `gg1_7`    | `gg1_below_stn_cloud_layer_top`                                           |
| `GG1`    | `gg1_8`    | `gg1_below_stn_cloud_layer_top_quality_code`                              |
| `GH1`    | `gh1_1`    | `gh1_hrly_solar_rad_hrly_avg_solarad`                                     |
| `GH1`    | `gh1_2`    | `gh1_hrly_solar_rad_hrly_avg_solarad_qc_quality_code`                     |
| `GH1`    | `gh1_3`    | `gh1_hrly_solar_rad_hrly_avg_solarad_flag_quality_code`                   |
| `GH1`    | `gh1_4`    | `gh1_hrly_solar_rad_min_solarad`                                          |
| `GH1`    | `gh1_5`    | `gh1_hrly_solar_rad_min_solarad_qc_quality_code`                          |
| `GH1`    | `gh1_6`    | `gh1_hrly_solar_rad_min_solarad_flag_quality_code`                        |
| `GH1`    | `gh1_7`    | `gh1_hrly_solar_rad_max_solarad`                                          |
| `GH1`    | `gh1_8`    | `gh1_hrly_solar_rad_max_solarad_qc_quality_code`                          |
| `GH1`    | `gh1_9`    | `gh1_hrly_solar_rad_max_solarad_flag_quality_code`                        |
| `GH1`    | `gh1_10`   | `gh1_hrly_solar_rad_std_dev_solarad`                                      |
| `GH1`    | `gh1_11`   | `gh1_hrly_solar_rad_std_dev_solarad_qc_quality_code`                      |
| `GH1`    | `gh1_12`   | `gh1_hrly_solar_rad_std_dev_solarad_flag_quality_code`                    |
| `GJ1`    | `gj1_1`    | `gj1_sunshine_obs_duration`                                               |
| `GJ1`    | `gj1_2`    | `gj1_sunshine_obs_duration_quality_code`                                  |
| `GK1`    | `gk1_1`    | `gk1_sunshine_obs_pct_possible_sunshine`                                  |
| `GK1`    | `gk1_2`    | `gk1_sunshine_obs_pct_possible_quality_code`                              |
| `GL1`    | `gl1_1`    | `gl1_sunshine_obs_duration`                                               |
| `GL1`    | `gl1_2`    | `gl1_sunshine_obs_duration_quality_code`                                  |
| `GM1`    | `gm1_1`    | `gm1_solar_irradiance_time_period`                                        |
| `GM1`    | `gm1_2`    | `gm1_solar_irradiance_global_irradiance`                                  |
| `GM1`    | `gm1_3`    | `gm1_solar_irradiance_global_irradiance_data_flag`                        |
| `GM1`    | `gm1_4`    | `gm1_solar_irradiance_global_irradiance_quality_code`                     |
| `GM1`    | `gm1_5`    | `gm1_solar_irradiance_direct_beam_irradiance`                             |
| `GM1`    | `gm1_6`    | `gm1_solar_irradiance_direct_beam_irradiance_data_flag`                   |
| `GM1`    | `gm1_7`    | `gm1_solar_irradiance_direct_beam_irradiance_quality_code`                |
| `GM1`    | `gm1_8`    | `gm1_solar_irradiance_diffuse_irradiance`                                 |
| `GM1`    | `gm1_9`    | `gm1_solar_irradiance_diffuse_irradiance_data_flag`                       |
| `GM1`    | `gm1_10`   | `gm1_solar_irradiance_diffuse_irradiance_quality_code`                    |
| `GM1`    | `gm1_11`   | `gm1_solar_irradiance_uvb_global_irradiance`                              |
| `GM1`    | `gm1_12`   | `gm1_solar_irradiance_uvb_global_irradiance_data_flag`                    |
| `GM1`    | `gm1_13`   | `gm1_solar_irradiance_uvb_global_irradiance_quality_code`                 |
| `GN1`    | `gn1_1`    | `gn1_solar_rad_time_period`                                               |
| `GN1`    | `gn1_2`    | `gn1_solar_rad_upwelling_global_solar_rad`                                |
| `GN1`    | `gn1_3`    | `gn1_solar_rad_upwelling_global_solar_rad_quality_code`                   |
| `GN1`    | `gn1_4`    | `gn1_solar_rad_downwelling_thermal_ir_rad`                                |
| `GN1`    | `gn1_5`    | `gn1_solar_rad_downwelling_thermal_ir_rad_quality_code`                   |
| `GN1`    | `gn1_6`    | `gn1_solar_rad_upwelling_thermal_ir_rad`                                  |
| `GN1`    | `gn1_7`    | `gn1_solar_rad_upwelling_thermal_ir_rad_quality_code`                     |
| `GN1`    | `gn1_8`    | `gn1_solar_rad_par`                                                       |
| `GN1`    | `gn1_9`    | `gn1_solar_rad_par_quality_code`                                          |
| `GN1`    | `gn1_10`   | `gn1_solar_rad_solar_zenith_angle`                                        |
| `GN1`    | `gn1_11`   | `gn1_solar_rad_solar_zenith_angle_quality_code`                           |
| `GO1`    | `go1_1`    | `go1_net_solar_rad_time_period`                                           |
| `GO1`    | `go1_2`    | `go1_net_solar_rad_net_solar_radiation`                                   |
| `GO1`    | `go1_3`    | `go1_net_solar_rad_net_solar_radiation_quality_code`                      |
| `GO1`    | `go1_4`    | `go1_net_solar_rad_net_ir_radiation`                                      |
| `GO1`    | `go1_5`    | `go1_net_solar_rad_net_ir_radiation_quality_code`                         |
| `GO1`    | `go1_6`    | `go1_net_solar_rad_net_radiation`                                         |
| `GO1`    | `go1_7`    | `go1_net_solar_rad_net_radiation_quality_code`                            |
| `GP1`    | `gp1_1`    | `gp1_modeled_solar_irradiance_data_time_period`                           |
| `GP1`    | `gp1_2`    | `gp1_modeled_solar_irradiance_global_horizontal`                          |
| `GP1`    | `gp1_3`    | `gp1_modeled_solar_irradiance_global_horizontal_src_flag`                 |
| `GP1`    | `gp1_4`    | `gp1_modeled_solar_irradiance_global_horizontal_uncertainty`              |
| `GP1`    | `gp1_5`    | `gp1_modeled_solar_irradiance_direct_normal`                              |
| `GP1`    | `gp1_6`    | `gp1_modeled_solar_irradiance_direct_normal_src_flag`                     |
| `GP1`    | `gp1_7`    | `gp1_modeled_solar_irradiance_direct_normal_uncertainty`                  |
| `GP1`    | `gp1_8`    | `gp1_modeled_solar_irradiance_diffuse_normal`                             |
| `GP1`    | `gp1_9`    | `gp1_modeled_solar_irradiance_diffuse_normal_src_flag`                    |
| `GP1`    | `gp1_10`   | `gp1_modeled_solar_irradiance_diffuse_normal_uncertainty`                 |
| `GP1`    | `gp1_11`   | `gp1_modeled_solar_irradiance_diffuse_horizontal`                         |
| `GP1`    | `gp1_12`   | `gp1_modeled_solar_irradiance_diffuse_horizontal_src_flag`                |
| `GP1`    | `gp1_13`   | `gp1_modeled_solar_irradiance_diffuse_horizontal_uncertainty`             |
| `GQ1`    | `gq1_1`    | `gq1_hrly_solar_angle_time_period`                                        |
| `GQ1`    | `gq1_2`    | `gq1_hrly_solar_angle_mean_zenith_angle`                                  |
| `GQ1`    | `gq1_3`    | `gq1_hrly_solar_angle_mean_zenith_angle_quality_code`                     |
| `GQ1`    | `gq1_4`    | `gq1_hrly_solar_angle_mean_azimuth_angle`                                 |
| `GQ1`    | `gq1_5`    | `gq1_hrly_solar_angle_mean_azimuth_angle_quality_code`                    |
| `GR1`    | `gr1_1`    | `gr1_hrly_extraterrestrial_rad_time_period`                               |
| `GR1`    | `gr1_2`    | `gr1_hrly_extraterrestrial_rad_horizontal`                                |
| `GR1`    | `gr1_3`    | `gr1_hrly_extraterrestrial_rad_horizontal_quality_code`                   |
| `GR1`    | `gr1_4`    | `gr1_hrly_extraterrestrial_rad_normal`                                    |
| `GR1`    | `gr1_5`    | `gr1_hrly_extraterrestrial_rad_normal_quality_code`                       |
| `HL1`    | `hl1_1`    | `hl1_hail_size`                                                           |
| `HL1`    | `hl1_2`    | `hl1_hail_size_quality_code`                                              |
| `IA1`    | `ia1_1`    | `ia1_ground_surface_obs_code`                                             |
| `IA1`    | `ia1_2`    | `ia1_ground_surface_obs_code_quality_code`                                |
| `IA2`    | `ia1_3`    | `ia2_ground_surface_obs_min_temp_time_period`                             |
| `IA2`    | `ia1_4`    | `ia2_ground_surface_obs_min_temp`                                         |
| `IA2`    | `ia1_5`    | `ia2_ground_surface_obs_min_temp_quality_code`                            |
| `IB1`    | `ib1_1`    | `ib1_hrly_surface_temp`                                                   |
| `IB1`    | `ib1_2`    | `ib1_hrly_surface_temp_qc_quality_code`                                   |
| `IB1`    | `ib1_3`    | `ib1_hrly_surface_temp_flag_quality_code`                                 |
| `IB1`    | `ib1_4`    | `ib1_hrly_surface_min_temp`                                               |
| `IB1`    | `ib1_5`    | `ib1_hrly_surface_min_temp_qc_quality_code`                               |
| `IB1`    | `ib1_6`    | `ib1_hrly_surface_min_temp_flag_quality_code`                             |
| `IB1`    | `ib1_7`    | `ib1_hrly_surface_max_temp`                                               |
| `IB1`    | `ib1_8`    | `ib1_hrly_surface_max_temp_qc_quality_code`                               |
| `IB1`    | `ib1_9`    | `ib1_hrly_surface_max_temp_flag_quality_code`                             |
| `IB1`    | `ib1_10`   | `ib1_hrly_surface_std_temp`                                               |
| `IB1`    | `ib1_11`   | `ib1_hrly_surface_std_temp_qc_quality_code`                               |
| `IB1`    | `ib1_12`   | `ib1_hrly_surface_std_temp_flag_quality_code`                             |
| `IB2`    | `ib2_1`    | `ib2_hrly_surface_temp_sb`                                                |
| `IB2`    | `ib2_2`    | `ib2_hrly_surface_temp_sb_qc_quality_code`                                |
| `IB2`    | `ib2_3`    | `ib2_hrly_surface_temp_sb_flag_quality_code`                              |
| `IB2`    | `ib2_4`    | `ib2_hrly_surface_temp_sb_std`                                            |
| `IB2`    | `ib2_5`    | `ib2_hrly_surface_temp_sb_std_qc_quality_code`                            |
| `IB2`    | `ib2_6`    | `ib2_hrly_surface_temp_sb_std_flag_quality_code`                          |
| `IC1`    | `ic1_1`    | `ic1_grnd_surface_obs_pan_evap_time_period`                               |
| `IC1`    | `ic1_2`    | `ic1_grnd_surface_obs_pan_evap_wind`                                      |
| `IC1`    | `ic1_3`    | `ic1_grnd_surface_obs_pan_evap_wind_condition_code`                       |
| `IC1`    | `ic1_4`    | `ic1_grnd_surface_obs_pan_evap_wind_quality_code`                         |
| `IC1`    | `ic1_5`    | `ic1_grnd_surface_obs_pan_evap_data`                                      |
| `IC1`    | `ic1_6`    | `ic1_grnd_surface_obs_pan_evap_data_condition_code`                       |
| `IC1`    | `ic1_7`    | `ic1_grnd_surface_obs_pan_evap_data_quality_code`                         |
| `IC1`    | `ic1_8`    | `ic1_grnd_surface_obs_pan_max_water_data`                                 |
| `IC1`    | `ic1_9`    | `ic1_grnd_surface_obs_pan_max_water_data_condition_code`                  |
| `IC1`    | `ic1_10`   | `ic1_grnd_surface_obs_pan_max_water_data_quality_code`                    |
| `IC1`    | `ic1_11`   | `ic1_grnd_surface_obs_pan_min_water_data`                                 |
| `IC1`    | `ic1_12`   | `ic1_grnd_surface_obs_pan_min_water_data_condition_code`                  |
| `IC1`    | `ic1_13`   | `ic1_grnd_surface_obs_pan_min_water_data_quality_code`                    |
| `KA1`    | `ka1_1`    | `ka1_extreme_air_temp_time_period`                                        |
| `KA1`    | `ka1_2`    | `ka1_extreme_air_temp_code`                                               |
| `KA1`    | `ka1_3`    | `ka1_extreme_air_temp_high_or_low`                                        |
| `KA1`    | `ka1_4`    | `ka1_extreme_air_temp_high_or_low_quality_code`                           |
| `KB1`    | `kb1_1`    | `kb1_avg_air_temp_time_period`                                            |
| `KB1`    | `kb1_2`    | `kb1_avg_air_temp_code`                                                   |
| `KB1`    | `kb1_3`    | `kb1_avg_air_temp_air_temp`                                               |
| `KB1`    | `kb1_4`    | `kb1_avg_air_temp_air_temp_quality_code`                                  |
| `KC1`    | `kc1_1`    | `kc1_extreme_air_temp_monthly_code`                                       |
| `KC1`    | `kc1_2`    | `kc1_extreme_air_temp_monthly_condition_code`                             |
| `KC1`    | `kc1_3`    | `kc1_extreme_air_temp_monthly_temp`                                       |
| `KC1`    | `kc1_4`    | `kc1_extreme_air_temp_monthly_date`                                       |
| `KC1`    | `kc1_5`    | `kc1_extreme_air_temp_monthly_temp_quality_code`                          |
| `KD1`    | `kd1_1`    | `kd1_heat_cool_deg_days_time_period`                                      |
| `KD1`    | `kd1_2`    | `kd1_heat_cool_deg_days_code`                                             |
| `KD1`    | `kd1_3`    | `kd1_heat_cool_deg_days_value`                                            |
| `KD1`    | `kd1_4`    | `kd1_heat_cool_deg_days_quality_code`                                     |
| `KE1`    | `ke1_1`    | `ke1_extreme_temp_number_days_max_32f_or_lower`                           |
| `KE1`    | `ke1_2`    | `ke1_extreme_temp_number_days_max_32f_or_lower_quality_code`              |
| `KE1`    | `ke1_3`    | `ke1_extreme_temp_number_days_max_90f_or_higher`                          |
| `KE1`    | `ke1_4`    | `ke1_extreme_temp_number_days_max_90f_or_higher_quality_code`             |
| `KE1`    | `ke1_5`    | `ke1_extreme_temp_number_days_min_32f_or_lower`                           |
| `KE1`    | `ke1_6`    | `ke1_extreme_temp_number_days_min_32f_or_lower_quality_code`              |
| `KE1`    | `ke1_7`    | `ke1_extreme_temp_number_days_min_0f_or_lower`                            |
| `KE1`    | `ke1_8`    | `ke1_extreme_temp_number_days_min_0f_or_lower_quality_code`               |
| `KF1`    | `kf1_1`    | `kf1_hrly_calc_temp`                                                      |
| `KF1`    | `kf1_2`    | `kf1_hrly_calc_temp_quality_code`                                         |
| `KG1`    | `kg1_1`    | `kg1_avg_dp_wb_temp_time_period`                                          |
| `KG1`    | `kg1_2`    | `kg1_avg_dp_wb_temp_code`                                                 |
| `KG1`    | `kg1_3`    | `kg1_avg_dp_wb_temp`                                                      |
| `KG1`    | `kg1_4`    | `kg1_avg_dp_wb_temp_derived_code`                                         |
| `KG1`    | `kg1_5`    | `kg1_avg_dp_wb_temp_quality_code`                                         |
| `MA1`    | `ma1_1`    | `ma1_atmos_p_obs_altimeter_setting_rate`                                  |
| `MA1`    | `ma1_2`    | `ma1_atmos_p_obs_altimeter_quality_code`                                  |
| `MA1`    | `ma1_3`    | `ma1_atmos_p_obs_stn_pressure_rate`                                       |
| `MA1`    | `ma1_4`    | `ma1_atmos_p_obs_stn_pressure_rate_quality_code`                          |
| `MD1`    | `md1_1`    | `md1_atmos_p_change_tendency_code`                                        |
| `MD1`    | `md1_2`    | `md1_atmos_p_change_tendency_code_quality_code`                           |
| `MD1`    | `md1_3`    | `md1_atmos_p_change_3_hr_quantity`                                        |
| `MD1`    | `md1_4`    | `md1_atmos_p_change_3_hr_quantity_quality_code`                           |
| `MD1`    | `md1_5`    | `md1_atmos_p_change_24_hr_quantity`                                       |
| `MD1`    | `md1_6`    | `md1_atmos_p_change_24_hr_quantity_quality_code`                          |
| `ME1`    | `me1_1`    | `me1_geopotential_hgt_isobaric_lvl_code`                                  |
| `ME1`    | `me1_2`    | `me1_geopotential_hgt_isobaric_lvl_height`                                |
| `ME1`    | `me1_3`    | `me1_geopotential_hgt_isobaric_lvl_height_quality_code`                   |
| `MF1`    | `mf1_1`    | `mf1_atmos_p_obs_stp_avg_stn_pressure_day`                                |
| `MF1`    | `mf1_2`    | `mf1_atmos_p_obs_stp_avg_stn_pressure_day_quality_code`                   |
| `MF1`    | `mf1_3`    | `mf1_atmos_p_obs_stp_avg_sea_lvl_pressure_day`                            |
| `MF1`    | `mf1_4`    | `mf1_atmos_p_obs_stp_avg_sea_lvl_pressure_day_quality_code`               |
| `MG1`    | `mg1_1`    | `mg1_atmos_p_obs_avg_stn_pressure_day`                                    |
| `MG1`    | `mg1_2`    | `mg1_atmos_p_obs_avg_stn_pressure_day_quality_code`                       |
| `MG1`    | `mg1_3`    | `mg1_atmos_p_obs_avg_sea_lvl_pressure_day`                                |
| `MG1`    | `mg1_4`    | `mg1_atmos_p_obs_avg_sea_lvl_pressure_day_quality_code`                   |
| `MH1`    | `mh1_1`    | `mh1_atmos_p_obs_avg_stn_pressure_month`                                  |
| `MH1`    | `mh1_2`    | `mh1_atmos_p_obs_avg_stn_pressure_month_quality_code`                     |
| `MH1`    | `mh1_3`    | `mh1_atmos_p_obs_avg_sea_lvl_pressure_month`                              |
| `MH1`    | `mh1_4`    | `mh1_atmos_p_obs_avg_sea_lvl_pressure_month_quality_code`                 |
| `MK1`    | `mk1_1`    | `mk1_atmos_p_obs_max_sea_lvl_pressure_month`                              |
| `MK1`    | `mk1_2`    | `mk1_atmos_p_obs_max_sea_lvl_pressure_date_time`                          |
| `MK1`    | `mk1_3`    | `mk1_atmos_p_obs_max_sea_lvl_pressure_quality_code`                       |
| `MK1`    | `mk1_4`    | `mk1_atmos_p_obs_min_sea_lvl_pressure_month`                              |
| `MK1`    | `mk1_5`    | `mk1_atmos_p_obs_min_sea_lvl_pressure_date_time`                          |
| `MK1`    | `mk1_6`    | `mk1_atmos_p_obs_min_sea_lvl_pressure_quality_code`                       |
| `MV1`    | `mv1_1`    | `mv1_present_weather_obs_condition_code`                                  |
| `MV1`    | `mv1_2`    | `mv1_present_weather_obs_condition_code_quality_code`                     |
| `MW1`    | `mw1_1`    | `mw1_present_weather_obs_manual_occurrence_condition_code`                |
| `MW1`    | `mw1_2`    | `mw1_present_weather_obs_manual_occurrence_condition_code_quality_code`   |
| `OA1`    | `oa1_1`    | `oa1_suppl_wind_obs_type`                                                 |
| `OA1`    | `oa1_2`    | `oa1_suppl_wind_obs_time_period`                                          |
| `OA1`    | `oa1_3`    | `oa1_suppl_wind_obs_speed_rate`                                           |
| `OA1`    | `oa1_4`    | `oa1_suppl_wind_obs_speed_rate_quality_code`                              |
| `OB1`    | `ob1_1`    | `ob1_hly_subhrly_wind_avg_time_period`                                    |
| `OB1`    | `ob1_2`    | `ob1_hly_subhrly_wind_max_gust`                                           |
| `OB1`    | `ob1_3`    | `ob1_hly_subhrly_wind_max_gust_quality_code`                              |
| `OB1`    | `ob1_4`    | `ob1_hly_subhrly_wind_max_gust_flag`                                      |
| `OB1`    | `ob1_5`    | `ob1_hly_subhrly_wind_max_dir`                                            |
| `OB1`    | `ob1_6`    | `ob1_hly_subhrly_wind_max_dir_quality_code`                               |
| `OB1`    | `ob1_7`    | `ob1_hly_subhrly_wind_max_dir_flag`                                       |
| `OB1`    | `ob1_8`    | `ob1_hly_subhrly_wind_max_stdev`                                          |
| `OB1`    | `ob1_9`    | `ob1_hly_subhrly_wind_max_stdev_quality_code`                             |
| `OB1`    | `ob1_10`   | `ob1_hly_subhrly_wind_max_stdev_flag`                                     |
| `OB1`    | `ob1_11`   | `ob1_hly_subhrly_wind_max_dir_stdev`                                      |
| `OB1`    | `ob1_12`   | `ob1_hly_subhrly_wind_max_dir_stdev_quality_code`                         |
| `OB1`    | `ob1_13`   | `ob1_hly_subhrly_wind_max_dir_stdev_flag`                                 |
| `OC1`    | `oc1_1`    | `oc1_wind_gust_obs_speed_rate`                                            |
| `OC1`    | `oc1_2`    | `oc1_wind_gust_obs_speed_rate_quality_code`                               |
| `OE1`    | `oe1_1`    | `oe1_summary_of_day_wind_obs_type`                                        |
| `OE1`    | `oe1_2`    | `oe1_summary_of_day_wind_obs_time_period`                                 |
| `OE1`    | `oe1_3`    | `oe1_summary_of_day_wind_obs_speed_rate`                                  |
| `OE1`    | `oe1_4`    | `oe1_summary_of_day_wind_obs_dir`                                         |
| `OE1`    | `oe1_5`    | `oe1_summary_of_day_wind_obs_time_occurrence`                             |
| `OE1`    | `oe1_6`    | `oe1_summary_of_day_wind_obs_quality_code`                                |
| `RH1`    | `rh1_1`    | `rh1_relative_humidity_time_period`                                       |
| `RH1`    | `rh1_2`    | `rh1_relative_humidity_code`                                              |
| `RH1`    | `rh1_3`    | `rh1_relative_humidity_percentage`                                        |
| `RH1`    | `rh1_4`    | `rh1_relative_humidity_derived_code`                                      |
| `RH1`    | `rh1_5`    | `rh1_relative_humidity_quality_code`                                      |
| `SA1`    | `sa1_1`    | `sa1_sea_surf_temp`                                                       |
| `SA1`    | `sa1_2`    | `sa1_sea_surf_temp_quality_code`                                          |
| `ST1`    | `st1_1`    | `st1_soil_temp_type`                                                      |
| `ST1`    | `st1_2`    | `st1_soil_temp_soil_temp`                                                 |
| `ST1`    | `st1_3`    | `st1_soil_temp_soil_temp_quality_code`                                    |
| `ST1`    | `st1_4`    | `st1_soil_temp_depth`                                                     |
| `ST1`    | `st1_5`    | `st1_soil_temp_depth_quality_code`                                        |
| `ST1`    | `st1_6`    | `st1_soil_temp_soil_cover`                                                |
| `ST1`    | `st1_7`    | `st1_soil_temp_soil_cover_quality_code`                                   |
| `ST1`    | `st1_8`    | `st1_soil_temp_sub_plot`                                                  |
| `ST1`    | `st1_9`    | `st1_soil_temp_sub_plot_quality_code`                                     |
| `UA1`    | `ua1_1`    | `ua1_wave_meas_method_code`                                               |
| `UA1`    | `ua1_2`    | `ua1_wave_meas_wave_period_quantity`                                      |
| `UA1`    | `ua1_3`    | `ua1_wave_meas_wave_height_dimension`                                     |
| `UA1`    | `ua1_4`    | `ua1_wave_meas_quality_code`                                              |
| `UA1`    | `ua1_5`    | `ua1_wave_meas_sea_state_code`                                            |
| `UA1`    | `ua1_6`    | `ua1_wave_meas_sea_state_code_quality_code`                               |
| `UG1`    | `ug1_1`    | `ug1_wave_meas_primary_swell_time_period`                                 |
| `UG1`    | `ug1_2`    | `ug1_wave_meas_primary_swell_height_dimension`                            |
| `UG1`    | `ug1_3`    | `ug1_wave_meas_primary_swell_dir_angle`                                   |
| `UG1`    | `ug1_4`    | `ug1_wave_meas_primary_swell_quality_code`                                |
| `UG2`    | `ug2_1`    | `ug2_wave_meas_secondary_swell_time_period`                               |
| `UG2`    | `ug2_2`    | `ug2_wave_meas_secondary_swell_height_dimension`                          |
| `UG2`    | `ug2_3`    | `ug2_wave_meas_secondary_swell_dir_angle`                                 |
| `UG2`    | `ug2_4`    | `ug2_wave_meas_secondary_swell_quality_code`                              |
| `WA1`    | `wa1_1`    | `wa1_platform_ice_accr_source_code`                                       |
| `WA1`    | `wa1_2`    | `wa1_platform_ice_accr_thickness_dimension`                               |
| `WA1`    | `wa1_3`    | `wa1_platform_ice_accr_tendency_code`                                     |
| `WA1`    | `wa1_4`    | `wa1_platform_ice_accr_quality_code`                                      |
| `WD1`    | `wd1_1`    | `wd1_water_surf_ice_obs_edge_bearing_code`                                |
| `WD1`    | `wd1_2`    | `wd1_water_surf_ice_obs_uniform_conc_rate`                                |
| `WD1`    | `wd1_3`    | `wd1_water_surf_ice_obs_non_uniform_conc_rate`                            |
| `WD1`    | `wd1_4`    | `wd1_water_surf_ice_obs_ship_rel_pos_code`                                |
| `WD1`    | `wd1_5`    | `wd1_water_surf_ice_obs_ship_penetrability_code`                          |
| `WD1`    | `wd1_6`    | `wd1_water_surf_ice_obs_ice_trend_code`                                   |
| `WD1`    | `wd1_7`    | `wd1_water_surf_ice_obs_development_code`                                 |
| `WD1`    | `wd1_8`    | `wd1_water_surf_ice_obs_growler_bergy_bit_pres_code`                      |
| `WD1`    | `wd1_10`   | `wd1_water_surf_ice_obs_growler_bergy_bit_quantity`                       |
| `WD1`    | `wd1_11`   | `wd1_water_surf_ice_obs_iceberg_quantity`                                 |
| `WD1`    | `wd1_12`   | `wd1_water_surf_ice_obs_quality_code`                                     |
| `WG1`    | `wg1_1`    | `wg1_water_surf_ice_hist_obs_edge_distance`                               |
| `WG1`    | `wg1_2`    | `wg1_water_surf_ice_hist_obs_edge_orient_code`                            |
| `WG1`    | `wg1_3`    | `wg1_water_surf_ice_hist_obs_form_type_code`                              |
| `WG1`    | `wg1_4`    | `wg1_water_surf_ice_hist_obs_nav_effect_code`                             |
| `WG1`    | `wg1_5`    | `wg1_water_surf_ice_hist_obs_quality_code`                                |

More information about these variables can be found in [this PDF
document](http://www1.ncdc.noaa.gov/pub/data/ish/ish-format-document.pdf).

To find out which categories are available for a station, set the
`add_data_report` argument of the `get_isd_station_data()` function to
`TRUE`. This will provide a tibble with the available additional
categories with their counts in the dataset.

``` r
# Get information on which additional met data
# fields are available at the Juvvasshoe station
additional_data_fields <-
  get_station_metadata() %>%
  filter(name == "JUVVASSHOE") %>%
  pull(id) %>%
  get_isd_station_data(
    startyear = 2015,
    endyear = 2015,
    add_data_report = TRUE
  )
```

``` r
additional_data_fields
#>   category total_count
#> 1      AJ1         194
#> 2      KA1         715
#> 3      MA1        5769
#> 4      MD1        8083
#> 5      OC1         485
```

For another station, `"BERGEN POINT"`, additional fields are available.
Its `MD1` field category deals with atmospheric pressure change and the
`SA1` category provides sea surface temperature. For `SA1`, the `sa1_1`
and `sa1_2` variables represent the sea surface temperature and it’s
quality code (where `1` is the ideal quality code value). Using
functions from **dplyr** (`select()`, `filter()`, `group_by()`, and
`summarize()`) one can create a table of the mean ambient and
sea-surface temperatures by month from the met data table. The
additional data is included in the met data table by using the
`select_additional_data` argument and specifying the `SA1` category
(multiple categories can be included).

``` r
# Get the average ambient temperature and the
# average sea-surface temperatures (sst) from
# the Bergen Point station for every month
# during 2015
bergen_point_temps <- 
  get_station_metadata() %>%
  filter(name == "BERGEN POINT") %>%
  pull(id) %>%
  get_isd_station_data(
    startyear = 2015,
    endyear = 2015,
    select_additional_data = "SA1"
  ) %>%
  select(month, temp, sa1_1, sa1_2) %>%
  filter(sa1_2 == 1) %>%
  group_by(month) %>%
  summarize(
    avg_temp = mean(temp, na.rm = TRUE),
    avg_sst = mean(sa1_1, na.rm = TRUE)
  )

bergen_point_temps
#> # A tibble: 12 × 3
#>    month    avg_temp   avg_sst
#>    <dbl>       <dbl>     <dbl>
#> 1      1 -0.05040928  4.192490
#> 2      2 -0.33406940  2.091724
#> 3      3  5.64884726  4.855947
#> 4      4 11.48056976  9.221271
#> 5      5 17.74683453 14.573022
#> 6      6 21.57782041 19.815111
#> 7      7 25.48305209 23.666417
#> 8      8 25.68534610 25.087288
#> 9      9 22.35404858 23.689652
#> 10    10 13.83142077 17.038115
#> 11    11 10.65389507 13.158805
#> 12    12 10.03515850 10.445389
```

## Installation

To install the development version of **stationary**, use the following:

``` r
remotes::install_github("rich-iannone/stationary")
```

## Code of Conduct

Please note that this project is released with a [Contributor Code of
Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree
to abide by its terms.

## License

MIT © Richard Iannone
