# Social Sentiment Analysis for Building ETF
This is the final project on text mining class(Course code：10920ISS507300) at National Tsing Hua University.
Our teammate include YU-JU Chou, TZU-CHUN Hung, and Yun-Ting Lee. 

## Introduction
In the investment area, volume is one of the important indicators affecting stocks prices. For investors who lack experience, how to choose target stocks is a difficult topic.
The first ETF which used the NLP technique to builds its index was launched this year. It looks like more and more people apply the news and social media data in the financial area. Social media discussions become one of the risks that affect stock prices.

Buzz ETF invests in the top 75 firms of the largest U.S. stock with highest discussions volume on social media. 
Since only the U.S. stocks on the shortlist, we also wanna know would it be possible to issue in Taiwan.
so we decided to use the concept of the Buzz ETF, build an index by using the stocks in the TW market.

## Outline
- Text processing
  - Web crawler
  - NLP
  - Corporate Filter
- Build ETF index
  - Name Frequency 
  - Rules Setting
  - Build ETF index
- Investment
  - Portfolio
  - Rules Setting
  - Backtesting

## Text Processing
### Web crawler
We need to analyze the data from 2018 to 2020 on the PTT stock board and all the listed stocks' and over-the-counter stocks' lists on the Taiwan Stock Exchange. 

For crawling PTT, we used Beautiful Soup and the request package. At the meanwhile, we write a function that can let us crawl PTT web pages between a specific number of pages. After crawling each post on one page, it will continue to crawl on the next page until the end page number we set.

For crawling the stock list, we use requests and pandas package to extract the table tag in the web page. The crawled stock names and stock codes contain garbled characters. For example:{‘1101\u3000台泥’}, so data needs to be cleaned up later.
### Natural Language Processing
* Data Cleaning\n
we start to clean the listed stocks and OTC stocks lists we crawled. Because we only need the stock name and stock code, so we select the column of "有價證券代號及名稱". Then use the re function and filter function to save the stock name and stock code into different lists.
In addition, in the process of web crawling, posts that have been removed will be crawled, so we need to filter that.

