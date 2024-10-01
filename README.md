# OpenInsiderApp
Idea: 
"Insider Trading" in the form of purchases of stock shares/options exchange drives the share price of a given stock up in a given short term window. 
"Insider Trading" is deemed legal if the information that prompted the purchases is public and the trade is filed within a 2-day window of the trade.
SEC Form 4 filings are the government's way of tracking insider trading, and Open Insider is a website database that does this, pulled from the EDGAR database. Data ranges back to 2013.
This project specifically monitors purchases of 500k or more from the CEO, CFO, or Director of a given company with a share price of 5$-50$.
CEO, CFO, and Dir are all big picture people that probably have the best "hunches" on which direction the company is going
It aims to copy insider buys as they are filed an hold the shares/options with a take profit point of 5% with a time to maturity of 30 days. 
Based on a backtest dating back to 2013, the insider buys track a stock's growth simulated opening a position on the open price of the day after the filing date (including buys made after hours) and then simulating selling the stock at the highest high achieved over the next 21 trading days. This allowed me to aggregate the average of the return I can possible expect over a month long period and how soon (TTM) I can expect to see what growth.

Stats:

Components:
  Dashboard: I used an MVC framework because that's what I am learning in school. The view is a home page with the most current stock recommendations along with their     current progression along the average TTM. This will allow someone to hypothetically get in on a trade as soon as possible and understand how much they can expect to gain.
  Database: I downloaded every entry from the OpenInsider website based on X project from GitHub into a CSV file, and then formatted it in SQL using SMSS. Then I exported it to Excel where I cleaned the data to only include my desired search criteria, which is every purchase of 500k or more with a lag (gap between trade and filing date) of 3 or less and a Ticker price o f $5-$50, and then the following month of yahoo finance data. This filter shrinked the data from roughly 600,000 rows to 16600.
  Python: I had to connect a python script to a button in excel to scrape the YF data. This resulted in a great deal of shrinkage with many of these companies being delisted or not having sufficient data (16600->)
  IBKR: For this exercise, I connected the MVC recommended stocks to my paper money IBKR account to automatically trade for me. I did this because I feel like there is more profit to be gained when a stock hits its presumed high while also being able to manually interupt the algorithm and trade at any time. By the way, this is very effective if your portfolio is vastly limited to the PDT rule for portfolios with equity < 25k.

Backtest:
Ultimately the results for the backtest were conclusive/inconclusive with the following statistics:
  AVG Return:
  AVG TTM:
  Cumulative Return:
  AVG Sharpe:
  Max Draw:
  Max Draw%:
  #Trades:
  Trade win ratio:

Instructions:

Notable Findings:
