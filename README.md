# pfx_parser

John Choiniere wrote this program a couple years ago for Beyond The Boxscore, [and you can read his article about setting it up here](https://www.beyondtheboxscore.com/2015/9/24/9374949/a-new-python-based-pitchf-x-parser-scraper), but since he's seemingly very busy doing analytics stuffs for the Mariners, I don't think that he's had time to revise and update his scraper. I've taken the liberty of updating it for use in 2018, as well as adding a couple other features.

## What does each file do?
**pfx_parser_csv.py** is a webscraper that scrapes the MLB Gameday API for Pitchcast data. Prior to the 2017 season, Pitchcast data was generated by Pitchf/x, a product of Sportvision. For the 2017 season onwards, Pitchcast data is generated by Trackman, which is a component of MLB's Statcast system.

A more complete picture of MLB pitch-by-pitch data can be obtained by scraping MLB's Statcast system directly, [for which I would reccomend Bill Petti's BaseballR package for R](https://billpetti.github.io/2018-02-19-build-statcast-database-rstats/), which is frequently updated.

Such a complete picture does not exist at the minor league level, but nevertheless, a picture exists - scorers manually locate pitches in the strikezone and where batted balls fall, and this data is collected and represented in the database. At high minor league levels, such as AAA, this data is reasonably accurate, if nowhere near as precise as Statcasts' data.

pfx_parser_csv.py prompts the user to input the league which they wish to scrape and the time frame that they wish to scrape. If a user does not specify a start date, the scraper starts scraping with the first games for which there is Pitchf/x data on Gameday, 2008. If a user does not specify an end date, the scraper ends scraping with the day before the day on which the scraper is run.

Play-by-play data for these events is then exported to atbat_table.csv, and pitch-by-pitch data for these events is exported to pitch_table.csv. For multiple years worth of data, these files can become quite large, so it is reccomended that in dealing with data longer than a month that the information be imported to a database.

**pitch_dup_remove.py** and **atbat_dup_remove.py** exist as debugging tools. Should the scraper fail due to memory issues or other issues in the middle of scraping, you may wish to restart scraping at the same date that the scraper was at in the middle of failing, which may generate additional data for events already scraped. For example, if the scraper fails in the middle of scraping games on June 1st, 2018, if the scraper is restarted and asked to look at games from June 1st, 2018, any data already scraped from June 1st will be duplicated. Running these scrapes removes duplicated values from the generated csv files.

**table_creation.sql** is a schema for setting up a database with the information in both atbat_table.csv and pitch_table.csv.

## What's new with this fork?

I've performed a couple updates to this fork, including:

* Fixed the scraper to work with MLB's revised Gameday format
* Scrapes pitch data for Minor Leagues

## What still is left to be done?
I am planning on implementing the following functionalities:

* Scrapes batted ball data for Minor Leagues

I am open to suggestions, so if there is additional functionality that people wish to have regarding this scraper, please let me know. You can find me on twitter @John_Edwards_

## What is required?
This code is written in Python 3, and requires installation of the BeautifulSoup4 and LXML packages. For more information on installing these packages, see [Choiniere's article.](https://www.beyondtheboxscore.com/2015/9/24/9374949/a-new-python-based-pitchf-x-parser-scraper)
