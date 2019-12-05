compiled
================
Ava Hamilton
12/4/2019

## read in data

``` r
nyc <- read_csv(file = "./p8105nyc_311_100k.csv") %>% 
    janitor::clean_names()
```

    ## Warning: 13949 parsing failures.
    ##    row                   col           expected                        actual                      file
    ## 100085 taxi_pick_up_location 1/0/T/F/TRUE/FALSE WEST   50 STREET AND BROADWAY './p8105nyc_311_100k.csv'
    ## 100150 taxi_pick_up_location 1/0/T/F/TRUE/FALSE JFK Airport                   './p8105nyc_311_100k.csv'
    ## 100172 taxi_pick_up_location 1/0/T/F/TRUE/FALSE 625 EAST 14 STREET MANHATTAN  './p8105nyc_311_100k.csv'
    ## 100215 taxi_pick_up_location 1/0/T/F/TRUE/FALSE Other                         './p8105nyc_311_100k.csv'
    ## 100268 taxi_pick_up_location 1/0/T/F/TRUE/FALSE Other                         './p8105nyc_311_100k.csv'
    ## ...... ..................... .................. ............................. .........................
    ## See problems(...) for more details.

``` r
nyc_tidy <- nyc %>%   
    filter(borough != "Unspecified") %>% 
    separate(closed_date, 
             into = c("closed_month","closed_day","closed_year"), 
             sep = "\\/" ) %>%
    separate(closed_year, 
             into = c("closed_year","closed_time"), 
             sep = " ") %>% 
    mutate(
        created_year = as.numeric(created_year),
        created_month = as.numeric(created_month),
        created_day = as.numeric(created_day),
        city = as.factor(city),
        status = as.factor(status),
        borough = as.factor(borough),
        agency = as.factor(agency),
        complaint_type = as.factor(complaint_type),
        community_board = as.factor(community_board),
        open_complaint = ifelse(status == "Closed", yes = 0, no = 1),
        # open_complaint = ifelse(is.na(closed_year),  yes = 1, no = 0),
        complaint_simp = case_when(
            str_detect(complaint_type, 
                       regex("street", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("sidewalk", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("curb", ignore_case = TRUE)) ~ "Street Condition",
            str_detect(complaint_type, 
                       regex("noise", ignore_case = TRUE)) ~ "Noise",
            str_detect(complaint_type, 
                       regex("heat", ignore_case = TRUE)) ~ "Heat",
            str_detect(complaint_type, 
                       regex("water", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("leak", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("plumbing", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("boiler", ignore_case = TRUE)) ~ "Water/plumbing",
            str_detect(complaint_type, 
                       regex("paint", ignore_case = TRUE)) ~ "Paint/Plaster",
            str_detect(complaint_type, 
                       regex("asbestos", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("lead", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("hazard", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("mold", ignore_case = TRUE)) ~ "Hazard Material",
            str_detect(complaint_type, 
                       regex("elevator", ignore_case = TRUE)) 
            |str_detect(complaint_type, 
                        regex("maintenance", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("electric", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("stairs", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("window", ignore_case = TRUE)) 
            |str_detect(complaint_type, 
                        regex("appliance", ignore_case = TRUE)) ~ "Maintenance",
            str_detect(complaint_type, 
                       regex("sanita", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("rodent", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("dirty", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("sew", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("standing water", ignore_case = TRUE)) ~ "Sanitation",
            str_detect(complaint_type, 
                       regex("tree", ignore_case = TRUE)) ~ "Tree",
            str_detect(complaint_type, 
                       regex("parking", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("car", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("drive", ignore_case = TRUE))
            |str_detect(complaint_type,  
                        regex("vehicle", ignore_case = TRUE))
            |str_detect(complaint_type,  
                        regex("traffic", ignore_case = TRUE)) ~ "Car/Traffic",
            str_detect(complaint_type, 
                       regex("air", ignore_case = TRUE)) ~ "Air Quality",
            str_detect(complaint_type, 
                       regex("collection", ignore_case = TRUE)) ~ "Collection",
            str_detect(complaint_type, 
                       regex("homeless", ignore_case = TRUE))
            |str_detect(complaint_type, 
                        regex("panhandling", ignore_case = TRUE)) ~ "Homeless"),
        health_complaint = ifelse(
            complaint_simp %in% c("Heat", "Water/Plumbing", "Hazard Material", "Sanitation", "Air Quality"), yes = 1, no = 0),
        complaint_simp = as.factor(complaint_simp),
        open_health_complaint = case_when(
            open_complaint == 1 & health_complaint == 1 ~ 1,
            open_complaint == 0 | health_complaint == 0 ~ 0
        ),
        # openCorr = ifelse(status == "Closed", yes = 0, no = 1),
        status = as.factor(status)
    )
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 489924 rows [1, 2, 3,
    ## 4, 5, 6, 7, 8, 9, 11, 12, 13, 14, 15, 16, 17, 18, 20, 21, 22, ...].

Adding community district data

``` r
inc_df = read_csv("./Med_income_2017.csv") %>% 
    janitor::clean_names() %>% 
    mutate(
        pop_1000s = round(total_population/1000, 0),
        inc_1000s = round(median_income/1000, 1),
        income_bracket = case_when(
            median_income >= 20000 & median_income <= 30000 ~ "20k-30k",
            median_income > 30000 & median_income <= 40000 ~ "30-40k",
            median_income > 40000 & median_income <= 50000 ~ "40-50k",
            median_income > 50000 & median_income <= 60000 ~ "50-60k",
            median_income > 60000 & median_income <= 70000 ~ "60-70k",
            median_income > 70000 & median_income <= 80000 ~ "70-80k",
            median_income > 80000 & median_income <= 90000 ~ "80-90k",
            median_income > 90000 & median_income <= 100000 ~ "90-100k",
            median_income > 100000 & median_income <= 125000 ~ "100-125k",
            median_income > 125000 & median_income <= 150000 ~ "125k+",
        ),
        income_bracket = as.factor(income_bracket)
    )

# adding income to data and removing any observations that do not have a specific community board to link to income
add_inc = left_join(nyc_tidy, inc_df, by = "community_board") %>% 
    filter(is.na(median_income) == FALSE) %>% 
    mutate(
        year_fac = as.factor(created_year)
    )
```

    ## Warning: Column `community_board` joining factor and character vector, coercing
    ## into character vector
