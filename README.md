# Social Sentiment Analysis for Building ETF
This is the final project on text mining class(Course code：10920ISS507300) at National Tsing Hua University.
Our teammates include YU-JU Chou, TZU-CHUN Hung, and Yun-Ting Lee. 

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
  - Equally-weighted portfolio
  - Risk parity portfolio

## Text Processing
### Web crawler
We need to analyze the data from 2018 to 2020 on the PTT stock board and all the listed stocks' and over-the-counter stocks' lists on the Taiwan Stock Exchange. 

For crawling PTT, we used Beautiful Soup and the request package. At the meanwhile, we write a function that can let us crawl PTT web pages between a specific number of pages. After crawling each post on one page, it will continue to crawl on the next page until the end page number we set.

For crawling the stock list, we use requests and pandas package to extract the table tag in the web page. The crawled stock names and stock codes contain garbled characters. For example:{‘1101\u3000台泥’}, so data needs to be cleaned up later.
### Natural language processing
- **Data cleaning**
  
  we start to clean the listed stocks and OTC stocks lists we crawled. Because we only need the stock name and stock code, so we select the column of "有價證券代號及名稱". Then use the re function and filter function to save the stock name and stock code into different lists.
  In addition, in the process of web crawling, posts that have been removed will be crawled, so we need to filter that.
  
- **Word segmentation**

  We can use Jieba to do the word segmentation. And because Jieba’s original dictionary is simplified Chinese. And To make segmentation more effective, we downloaded the traditional Chinese version as a dictionary. And also we add the Stock name into the dictionary. After that, we use jieba.cut() function to segment each article title.

- **Corporate Filter**

  We compare the words with the stock names we collected. And remain the article that contains the stock names.

## Build ETF Index
- **Name Frequency**

  We decided to build a dictionary that consists of words related to bearish trend such like 空、下跌、不如預期、調降… and etc.(down_lt = ['利空','空','空頭','跌','下跌','調降','下修','不如預期']) and we excluded titles that contain above-mentioned keywords, which we thought it may reflect the investors give the impression that prices of these companies would go down. After filtering the titles that contained company name, we counted the number of names that appeared in these remaining titles for each company.
  
- **Setting rules** 

  We assume that the higher frequency a company being mentioned refers to more discussion about their current states, which may lead to higher trading volume, so as the price. So, we ranked the top five stocks each month to feature in our portfolio.
  
  In order to replicate the concept of Buzz ETF, it weight tickers by the highest 15% and decreases progressively. Considering we have 5 stocks in our portfolio each time period, we take Arithmetic sequence from 15% to 3%. And then we multiplied it by a number to make its total weight add up to 100%. So, we set the rules that holdings are weighted by approximately 33.3%, 26.6%, 20.0%, 13.3%, 6.6%

- **Build ETF index** 
  The process would like: we collect the news titles for every month, do the preprocessing mentioned above, and it would return five target stocks for us to buy at the first day in next months.

  The graph as following is the ETF we built according to the aforementioned rules. We could notice that the performance of this portfolio did not meet our expectations.  
 
  ![buzz_etf_tw - 複製](https://user-images.githubusercontent.com/84038124/136065721-144b50fd-f0a7-4248-8c9d-1cf0cfef1aca.png)

## Investment
We then invest five target stocks we mentioned before and build portfolios with the other two different weighting methods, trying to improve our performance of the portfolio, and two of them would also be compared to our benchmark, which is Taiwan Capitalization Weighted Stock Index 臺灣加權股價指數（0000）
- **Equally-weighted portfolio**

  We built equally-weighted portfolio underlies on same assets as prior portfolio, and we found the performance being improved but still not quite good.
  
  ![port_equal_weight](https://user-images.githubusercontent.com/84038124/136070584-5ec1b4ea-807f-46fc-bceb-23c0360db4ef.png)

- **Risk parity portfolio**

  The main concept of risk parity is that risk parity adjusts asset allocations to the same risk level. In a traditional portfolio, we usually have 60/40 for Stocks and Bonds.
  
  But let’s take the American market, for example, the volatility of stocks is 15%, as for bonds, only 5%. It derived that it would be almost 90% of risk exposure comes from stocks.

  As a result, we can take risk parity to allocate investment capital on a risk-weighted basis to optimally diversify investments. Finally, this is our portfolio applying rick parity. We can see that we do further improve our performance with sharpe ratio from 0.06 to 0.38, though it still not outperforms the benchmark.
  
  ![port_risk_parity](https://user-images.githubusercontent.com/84038124/136072711-1f9f24c4-03e3-4cf7-b710-c8df65246388.png)

## Conclusion
According to the results from the backtesting, our portfolio has positive return.  Also, we certificated the Risk parity portfolio will perform better than 1/n-weighted portfolio. But both aren’t better than the benchmark.
 
And, The indicator we caught from PTT Stock article titles is lagged index.
Since we rebalance the portfolio monthly, the stock price fluctuations caused by the online social media volume may have been reflected.
 
And we think that The corpus data from PTT Stock article titles might not sufficient. Sometimes we would invest the stock with less than 20 discussion volume. So, in the future, We could collect data from more social media, such as Dcard, Facebook, CMoney and Mobile01.

