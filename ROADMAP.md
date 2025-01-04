#ROADMAP.md:

1. Project Setup
Tech Stack Decisions
Programming Language: Python is recommended for data cleaning, backtesting, and integration with Interactive Brokers.
Libraries & Frameworks:
Data: pandas, numpy, requests
Backtesting & Analysis: pandas, matplotlib, possibly backtrader or custom logic
APIs: requests for HTTP calls, yfinance for historical data, openinsider scraping or an API if available
Dashboard: Could use a lightweight web framework like Flask or FastAPI. Combine with V0 for styling (as planned).
Notifications: For texting, consider Twilio’s free trial (or an equivalent free service if possible); for email, use a free SMTP service or something like Gmail’s API.
Hosting & Collaboration: Replit is your environment, but you may consider GitHub for version control, or just rely on Replit’s built-in Git-like features.
Initial File Structure
A possible structure in your Replit project:
bash
Copy code
.
├── data/
│   ├── openinsider_raw.csv
│   └── cleaned_data_5y.csv
├── notebooks/
│   ├── cleaning.ipynb
│   └── analysis.ipynb
├── src/
│   ├── main.py         # Dashboard & main entrypoint
│   ├── backtest.py     # Backtesting logic
│   ├── utils.py        # Utility functions
│   ├── openinsider.py  # Scraper / data fetch from openinsider
│   └── broker.py       # Interactive Brokers integration
├── requirements.txt
├── ROADMAP.md
└── README.md


Version Control & Environment Setup
Replit: Use the secrets manager for storing API keys/tokens (e.g., Gmail, Twilio, IBKR).
Dependencies: Keep track in requirements.txt (e.g., pandas==x.x.x, requests==x.x.x, yfinance==x.x.x).

2. Data Collection & Cleaning
2.1 Gather Raw Insider Data
Download from OpenInsider: You have a CSV (openinsider_raw.csv) from 2013 to September 2024.
Plan:
Filter by date: Since you only plan to use the last 5 years, slice the CSV to keep data from ~2018 onward.
Check currency: Remove entries that are not in USD or that have suspicious formatting.
Submission Lag: Remove entries where the difference between trade date and file date is longer than 3 days.
Title Column Normalization: Standardize titles like Chief Exec. Off., CEO, EVP, etc., so that your data is consistent. Create a small map or dictionary to unify them (e.g., “CEO”, “CFO”, “EVP”, “Director”, etc.).
Dollar Amount: Focus on buys of 250k or more. Create subsets also for 500k+, 750k+, 1M+ as separate test sets.
Outliers: Remove or flag trades that show extreme discrepancies (e.g., the stock’s average price over the next 23 days is 10× or 1/10 of the purchase price for no reason). Keep a record of removed entries.
2.2 Historical Price Data (Yahoo Finance)
Price Data: For each insider trade in your cleaned dataset, fetch the next 23 trading days of price data from Yahoo Finance.
Store these in either local CSV files or in memory if you do it on-the-fly.
Calculate performance metrics over the 23-day period:
% Growth = (Price on Day 23 – Price on Day 1) / Price on Day 1
Possibly track daily changes to see volatility or highest intraday peaks.
2.3 Final Datasets for Backtesting
Output: You’ll create 3–4 “final” CSVs:
_5y.csv – all insider trades from the last 5 years.
_2y.csv – last 2 years.
_1y.csv – last 1 year.
Possibly segmented by trade size (250k+, 500k+, 1M+, etc.)

3. Backtesting & Analysis
3.1 Establishing the Trading Rules
Core Logic:
Buy on the day after the filing date.
Sell at either:
Stop Loss threshold (e.g., −X% from entry)
Take Profit threshold (e.g., +Y% from entry)
Or 23 trading days later if neither threshold is hit
Record final P/L, ROI, and TTM (time to maturity).
Parameter Tuning:
Systematically try different stop loss (e.g., −5%, −10%) and take profit (e.g., +10%, +15%, +20%) combos.
Evaluate performance metrics:
Win Rate (% of trades that are profitable)
Average Gain (on winning trades) and Average Loss (on losing trades)
Expected Value per trade
Max Drawdown
3.2 Feature Importance & Strategy Refinement
Insider Title: Compare how CEOs vs. CFOs vs. Directors, etc. perform.
Sector / Industry: Check if certain sectors historically have higher insider-based alpha.
Volume & Position Size: See if insider buys with higher volumes have more predictive value.
Timing: Investigate if TTM is predictable. Possibly discover if certain titles close more quickly in profit.
3.3 Finalize the Strategy
Selection Criteria:
Choose best insider signals (e.g., title, sector, volume, etc.).
Choose best thresholds for stop loss / take profit.
Keep track of TTM stats to refine your notifications.
Validation:
Run 5-year, 2-year, and 1-year tests to see if the strategy is robust in different time frames.
If needed, do a forward test on the last 6 months to see if the strategy still holds.

4. Algorithm & Real-Time Monitoring
4.1 Real-Time Scraper
Implementation:
Frequency: Check OpenInsider every X minutes for new trades.
Parsing: Convert the newly posted data into the same cleaned format you used in backtesting (title normalization, etc.).
Filtering: Only include trades that meet your threshold criteria (dollar amount ≥ 250k, 3-day filing window, etc.).
Decision Engine:
For each new insider trade, calculate or retrieve real-time stock price.
Use your backtest results to produce a confidence interval or expected ROI.
If it meets the threshold of your final strategy, prepare to send a trade alert to the dashboard and your phone/email.
4.2 Integration with Interactive Brokers
IBKR API:
Use Interactive Brokers Python API (via ib_insync or official IB API).
Store your IBKR credentials as environment variables in Replit’s secret manager.
Workflow:
Algorithm proposes a trade → system texts/emails you → if you approve, system executes the order in IBKR.
Monitor the order’s fill status.
Maintain real-time data for stop loss/take profit triggers.
4.3 Notifications & Approvals
Text & Email:
Twilio (or alternative free SMS) for texting.
Gmail API or your own SMTP for emails.
Links in the message:
Approve or Reject trade link.
Link to the dashboard for more detailed info.
Google OAuth:
Use Google’s OAuth free tier to let you sign in with Gmail.
Once signed in, you can navigate to the dashboard.
The system can also store your phone number for SMS alerts.

5. Dashboard Development
5.1 Core Features
Pick Feed: Real-time table of potential insider trades, showing:
Insider name & title
Company & ticker
Date/Time of trade filing
Dollar amount & volume
Confidence score or predicted ROI
Action Buttons (Approve, Reject, More Info)
Analytics Panels:
Overall Win Rate: % of trades that ended profitably since the algorithm started.
ROI: In both % and $ for:
All time
Current year
Current month
1W, 3D, 1D
Open Positions: Show current trades, their P/L, time remaining until 23-day maturity.
Closed Positions: Historical trades with final P/L and TTM.
Quick Links:
Twitter search, Reddit search, Yahoo Finance, and Reuters for each stock symbol, so you can see any buzz around the ticker.
5.2 UI/UX
Using V0:
Use the framework’s built-in components to build tables, charts, and forms.
Keep it mobile-responsive since you might want to check on your phone.
Free Hosting:
Replit’s built-in web server can serve your Flask/FastAPI app.
Or host on a free platform like Render or Vercel if Replit’s limitations are reached.

6. Testing & Deployment
6.1 End-to-End Testing
Unit Tests: Check your data cleaning, scraping, and basic transformations.
Backtest Validation: Confirm your final strategy’s results match your previous notebooks’ analysis.
Live Paper Trading: Test with a simulated IBKR or paper trading account to ensure real-time decisions, notifications, and approvals all function correctly.
6.2 Deployment Steps
Set Up Replit Always-On (if possible) or add a cron job-like approach to run the scraper regularly.
Configure Secrets: IBKR credentials, Twilio, Gmail OAuth in Replit environment variables.
Monitor: Keep logs for scraping errors, missed trades, or potential data anomalies.

7. Maintenance & Future Enhancements
Refine Criteria: Continue analyzing which positions (CEO vs. CFO) or which sectors are most predictive.
Add Machine Learning: Optionally use ML classification/regression models to predict the success of each insider trade.
Performance Monitoring: Build a quick daily or weekly summary report emailed or texted to you.
Expand to Other Data Sources: e.g., Whale Wisdom for hedge fund holdings, or Finra for short data.
Community & Social: Possibly incorporate social sentiment from Twitter or Reddit to refine your picks further.

Conclusion
This roadmap lays out a clear path to:
Clean & backtest insider trading data with robust filtering and analysis.
Discover the best combination of insider attributes, stop loss/take profit triggers, and other variables to maximize ROI.
Deploy a real-time scraper connected to Interactive Brokers for actual trade execution—with your final approval.
Manage & Track all your stats, trades, and performance in a user-friendly, mobile-responsive dashboard.

