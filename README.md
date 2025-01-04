# OpenInsiderApp (10/9/2024)
Idea: 
Large scale purchases of stock shares/options by executives within their own company drives the share price of a given stock up in a given short term window. 
"Insider Trading" is deemed legal if the information that prompted the purchases is public and the trade is filed within a 2-day window of the trade.
SEC Form 4 filings are the government's way of tracking insider trading, and Open Insider is a website database that does this, pulled from the EDGAR database. Data ranges back to 2013. http://openinsider.com/

This project specifically monitors purchases of 500k or more from the CEO, CFO, or Director of a given company with a share price of 1$-75$.
Reason: There are 2 types of insiders: those that buy a lot and those that know what is going on in the company, and I am trying to find the optimal mix. I chose the CEO, CFO, and Dir because they are all big picture people that probably have the best "hunches" on which direction the company is going. 
It aims to copy these specific insider buys as they are filed an hold the shares/options with a proposed take profit point of 5% with a time to maturity of 30 days. 
Based on a backtest dating back to 2013, the insider buys track a stock's growth simulated opening a position on the open price of the day after the filing date (including buys made after hours) and then simulating selling the stock at the highest high achieved over the next 21 trading days. This will allow me to aggregate the average of the return I can possible expect over a month long period and how soon (TTM) I can expect to see what growth.

Components:
  Dashboard: I am looking to do an ASP.NET MVC framework to display stock picks as well as varying levels of probability of success based on correlatons I hope to find in the data upon further inspection. The view will be a home page with the most current stock recommendations along with their current progression along the 30 day timeline and what change has happened and what appreciation can be expected. This will allow someone to hypothetically get in on a trade as soon as possible and understand how much they can expect to gain with respect to time.
  Database: I downloaded every entry from the OpenInsider website based on X project from GitHub into a CSV file, and then formatted it in SQL using SSMS. Then I exported it to Excel where I cleaned the data to only include my desired search criteria, which is every purchase of 500k or more with a lag (gap between trade and filing date) of 3 or less and a Ticker price of $1-$75. The goal is then to pull historical data using the STOCKHISTORY function within excel to pull the next month (21 entries roughly) of data and start compiling a confidence interval.
  Brokerage: For this exercise, I will pass the MVC recommended stocks to my paper money IBKR brokerage account to automatically trade for me. I did this because I feel like there is more profit to be gained when a stock hits its presumed high while also being able to manually interupt the algorithm and trade at any time. By the way, this is very effective if your portfolio is vastly limited to the PDT rule for portfolios with equity < 25k. Additionally IBKR ALLOWS YOU TO PLACE TRADES AND HAVE THEM FILLED AFTER HOURS. This is important because the vast majority of these filings occur within 6 hours of the market shutting down and hardly ever occur within trading hours.

Backtest:
Ultimately the results for the backtest will be conclusive/inconclusive with the following statistics:

  AVG Return:
  
  AVG TTM: How long does it take to hit 5%? 10%? Is there a better number?
  
  Cumulative Return: Total return for the portfolio
  
  AVG Sharpe: (Return - RFR)/(Standard deviation of Portfolio return)
  
  Max Draw: greatest dollar amount lost on a given trade
  
  Max Draw%: greatest % lost on a trade
  
  #Trades: number of trades
  
  Trade win ratio: How often was I right with respect to hitting my target in 30 days or less?


Notes and Ideas:
UNDERSTAND RIGHT NOW THAT THIS EXPERIMENT IS A SIMPLE X TO Y RELATIONSHIP (INSIDER PLACES TRADE, STOCK GOES UP)
Will be leaning heavily in these supplemental data points to further add probability to the success of a given trade:
-I want to pay special attention to what I call "clusters", or multiple buys of the same stock in a 3-5 day range. In my empirical research, these stocks have the largest jumps the soonest. Here are the 2 most recent clusters (BHVN and LUV):

  BHVN:
    
  <img width="1824" alt="image" src="https://github.com/user-attachments/assets/5bf7e78f-a411-4760-b186-79ba5a059781">
      
  Total Time: 3 trading days
      
  ROI: 19.89%
      
  LUV:
    
  <img width="1920" alt="image" src="https://github.com/user-attachments/assets/bd4819c9-4e46-433b-82bf-15d8310f2b48">
      
  Total Time: 1 trading day
      
  ROI: 7.04%
      
-Value of the investment with respect to total portfolio value (For instance Bill gates can buy $20 million of shares of a given stock, and I would consider that trade to have less liability than Joe Smith who invests $500k into a stock and it is 0.5 of his total portfolio. Would think this means he is more invested in the trade -> higher likelihood the stock rises over the month long period.)
-Why a month long holding period? Many companies block their insiders from buying/selling stock for a month long time period with respect to earnings, big news, stock splitting, etc. These "black windows" prevent insiders from pumping and dumping stock around big news for huge profits like I am trying to do. That being said, I want to know:
-How close the trade is to a company landmark in stock history, specifically quarterly earnings and 52-week highs and lows. What are the expectations laid out in the shareholder meeting?
-How does the industry/sector the stock is in measure up to the stock? Near competitors? Not sure how to source data for this.
-Maybe I filter stocks with an acceptable threshold of average volume and designate ones that fall short as risky.
-Sentiment analysis. Using popular news sources, twitter/reddit/youtube, combined with maybe an analyzed script of shareholder meeting expectations to give the stock more confidence before picking

-How do these supplemetnal data sources affect the projected return of a stock over 90 days?
-I would LOVE to figure out how to do options trading with this strategy. I firmly believe that is where the real money is, I just don't know how to jump through the after hours hoop, and I don't know how to limit losses.
-What would an adequate stop loss for this strategy be? I am really struggling with this because some of these stocks are incredibly volatile and setting a small stop loss would in all likelihood stop you out the first day.

Concluding Remarks:
I would be incredibly happy with a statistical average of 7% return over 30 days with 60% confidence when I place a trade purely based on the X to Y relationship. This will give a lot of room to develop varying levels of confidence with supplementary data.
Looking into the future, I would love to do options trading with this as that is where the money is at, I just need to learn more.
I would love to put these picks, historical stats, and level of accuracy and confidence quantified onto the internet and monetize these picks. I think the mark I need to hit for that is somewhere in the 75-85% range. I feel like people will value correctness over expected return because they can always scale up.
