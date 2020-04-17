# Measuring COVID-19 testing 

This is just an ongoing personal R project focused on working with COVID-19 testing across the world, albeit with a very strong focus in Latin America.

## What you need

As of today, to run this you just need R and a couple basic libraries (Tidyverse, lubridate).

Today = 16 April 2020.

## What's in

You'll see a couple folders: scripts and data. They contain what they seem to contain (scripts to obtain the data, and necessary input / resulting output).

The basic script **tests.R** just deals with and transforms the data. It is pretty self-explanatory. It also features two dummy ggplot generators at the end of it.

As for the data...

**tests.csv** is a modified extraction from Our World In Data's amazing compilation, available [here](https://ourworldindata.org/grapher/total-tests-per-thousand-since-5th-death). Modified in what sense? I have added/sligthly modified data for Argentina (before 3 April) and Colombia. Argentina's data was offered by the country's Healthcare Ministry on April the 3rd, and [here](https://www.infobae.com/politica/2020/04/03/los-tests-de-coronavirus-bajo-la-lupa-cuantos-se-hicieron-cual-es-el-porcentaje-de-casos-positivos-y-que-lugar-del-ranking-mundial-ocupa-la-argentina/) is the source who gathered the info. As for Colombia, the national health authority updates the accumulated figure of performed tests every day. A group of journalists, academics and students at the Faculty of Communications keeps track of the figure since March the 21st, and [they are kind enough to offer it to everyone who needs using it](https://www.unisabanamedios.com/datos-tiempo-real-coronavirus).

**cases.csv** and **deaths.csv** are just extractions from Johns Hopkins University's effort to keep a consolidated, daily updated dataset of the pandemic across the world. Original data and scripts are available [in their own repository](https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data).

All data runs up to 15-16 April 2020.


## What will be in (hopefully)


I'll try and keep the test dataset updated! But I can't promise anything due to the irregularity of original sources (i.e. national health authorities across the world).


## Author

Jorge Galindo - [@JorgeGalindo](https://twitter.com/jorgegalindo)


## License

Free to use however you see fit! But I wish you read some specialized literature before getting too excited about trends, etc. I'm no epidemiologist myself, that's why I know you'll need some extra knowledge in case you are neither. I hope you can provide something useful to the public debate. I'm sure you will :)