rm(list = ls())

library(tidyverse)

#Extract & organize test data -- please look at readme.md to understand where it comes from
initial_tests_df <- read.csv("data/tests.csv", sep=",")

latam_vector <- c("Argentina", "Bolivia", "Brazil", "Chile", "Colombia", "Costa Rica", "Cuba", "Dominican Republic", "Ecuador", "El Salvador", "Guatemala", "Honduras", "Mexico", "Nicaragua", "Panama", "Paraguay", "Peru", "Uruguay", "Venezuela")
goodseries_vector <- c("Argentina", "Bolivia", "Chile", "Colombia", "Costa Rica", "Ecuador", "Mexico", "Peru", "Panama", "Paraguay")

initial_tests_df <- initial_tests_df %>%
  mutate (latam=case_when(
    Entity %in% latam_vector ~ TRUE,
  ),
    goodseries=case_when(
    Entity %in% goodseries_vector ~ TRUE
  )
  )

#Merge with cases & deaths data! It comes from https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data

initial_cases_df <- read.csv("data/cases.csv", sep=",")

cases_df <- initial_cases_df %>%
  mutate (  goodseries=case_when(
    Country.Region %in% goodseries_vector ~ TRUE
        ),
    country=as.character(Country.Region)
    ) %>%
  filter(goodseries == TRUE) %>%
  select(country,dates,cases)

initial_deaths_df <- read.csv("data/deaths.csv", sep=",")

deaths_df <- initial_deaths_df %>%
  mutate (  goodseries=case_when(
    Country.Region %in% goodseries_vector ~ TRUE
  ),
  dates=date,
  country=as.character(Country.Region)
  ) %>%
  filter(goodseries == TRUE) %>%
  select(country,dates,deaths)

casesdeaths_df <- left_join(deaths_df, cases_df) %>%
  mutate(
    dates=as.Date(dates,"%Y-%m-%d")
  ) %>%
  select(country, dates, deaths, cases)

tests_df <- initial_tests_df %>%
  mutate (  goodseries=case_when(
    Entity %in% goodseries_vector ~ TRUE
  ),
  country=as.character(Entity),
  dates=as.Date(Date, "%B %d, %Y"),
  tests_thousand=Total.tests.per.thousand
  ) %>%
  filter(goodseries == TRUE) %>%
  select(country,dates,tests_thousand)

tests_df <- left_join(tests_df, casesdeaths_df)

tests_deaths_df <- tests_df %>% 
  filter(deaths>0) %>%
  group_by(country) %>%
  mutate(days_since_1stdeath = row_number())

tests_cases_df <- tests_df %>% 
  filter(cases>0) %>%
  group_by(country) %>%
  mutate(days_since_1stcase = row_number())

#Produce a neat wide df

tests_df_wide <- tests_deaths_df %>% 
  select(country, days_since_1stdeath, tests_thousand) %>%
  pivot_wider(names_from = country, values_from = tests_thousand)

write.csv(tests_df_wide, "data/testswide.csv")

#Produce a different df including absolute numbers

testsabs_deaths_df <- tests_deaths_df %>% 
  mutate(pop=case_when(
        country == "Argentina"  ~  44500000,
        country == "Bolivia"  ~  11350000,
        country == "Chile"  ~  18730000,
        country == "Colombia"  ~  48200000,
        country == "Costa Rica"  ~  4999000,
        country == "Ecuador"  ~  17080000,
        country == "Mexico"  ~  126200000,
        country == "Panama"  ~  4177000,
        country == "Paraguay"  ~  6956000,
        country == "Peru"  ~  31995000
        )
      ) %>%
  mutate(tests=tests_thousand*pop/1000,
         cases_tests=cases/tests,
         rollmean_casestests=rollmean(cases_tests,3,na.pad=TRUE, align="right"),
         cases_tests_character=as.character(round(cases_tests*100)),
         cases_tests_character=paste0(cases_tests_character,"%"))

  


##-------- Some dummy graphs in case they're needed

test_curves <- ggplot(data = tests_cases_df, mapping = aes(x = days_since_1stcase, y = tests_thousand , colour = country)) + 
  geom_line(size=1.5)+
  scale_y_log10() +
  theme(panel.background = element_blank(),
        legend.position="none",
        plot.background = element_rect(fill="#ffffff"),
        panel.grid.major = element_line(colour="#eeeeee", size=.5),
        panel.grid.minor = element_line(colour="#eeeeee", size=.25),
        axis.text = element_text(family = "Helvetica Neue", 
                                 color="#999999",
                                 size=20
        ),
        axis.text.x = element_text(margin=margin(5,5,10,5,"pt")),
        axis.title = element_text(size = 20, 
                                  family="Helvetica Neue",
                                  color="#444444"
        ),
        plot.title = element_text(family = "Helvetica Neue", 
                                  color="#444444", 
                                  face="bold", 
                                  size=35, 
                                  hjust=0.25,
                                  margin = margin(b = 30),
        ),
        plot.margin = unit(c(1,1,1,1), "cm")
  )

test_ratios <- ggplot(data = testsabs_deaths_df)+
  geom_line(aes(x = days_since_1stdeath, y = cases_tests , colour = country), size = 1.75)+
  gghighlight(use_direct_label = FALSE,
              unhighlighted_params = list(size = 1, colour = alpha("grey", 0.95))) +
  facet_wrap(~ country, ncol = 2) +
  scale_x_continuous(1,41, breaks = seq(1, 41, by = 10))+
  scale_y_continuous(0,0.4, breaks = seq(0, 0.4, by = 0.1), expand = c(0, 0))+
  scale_color_viridis(discrete = TRUE, begin=0.4, end = 0.4)+
  geom_hline(yintercept=0, color = "#000000", size = 0.75) +
  geom_vline(xintercept=1, color = "#000000", size = 0.75) +
  theme(panel.background = element_rect(fill="#fafafa"),
        axis.ticks = element_blank(),
        legend.position="none",
        strip.background = element_blank(),
        strip.text.x = element_blank(),
        plot.background = element_rect(fill="#ffffff"),
        panel.grid.major = element_line(colour="#dddddd", size=.5),
        panel.grid.minor = element_line(colour="#dddddd", size=.5),
        panel.spacing = unit(4, "lines"),
        axis.text = element_blank(),
        axis.title = element_blank()
  )