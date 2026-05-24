I would like to build a personal finance system that keeps track of all of my accounts, all of the transactions in my accounts, and all of the money in my accounts so I can have a centralized view of what is there, what can be invested in, and how we can make better decisions about my finances in terms of budgets, investments, and growing the money. So I want to make this system.

In order to make this system, I want it to be a deployed cloud application, which means it can be made in Next.js. This is not going to be a public application, so we're going to run this in Cloudflare Workers to make sure we don't incur any costs because we will be the only people using this application. However, we may need a back-end to do some API scraping or some scraping to ensure that we get the accounts and the details of them.

The best way to move forward would be to implement integrations of existing API, and this is what I have found so far. These are the accounts I use:

- **Santander (Current Account)** - There is an Open Banking API that can be used but it's heavily regulated so it won't be possible to get access to that. We might need to make a scraper to build it
- **Barclays (Current Account)** - Same as Santander. We might need to build a scraper to make this work.
- **Monzo (Current account)** - This is where I put money to experiment with things and latest tech, which can be useful to build a business. See [[Monzo API Reference]] which is the Monzo's API documentation which can be used to integrate Monzo.
- **Starling (Current Account)** - This is my current everyday account that I use for day to day expenses. This has an extensive API that can be used [[Starling API Documentation]]
- **Mettle (Current account)** - This is the account that I use for coding.kitty and any side hustles I'm doing. This comes under [[Natwest - Account & Transaction  Product Documentation]] . But scraping may be required.
- **Trading 212 (Investment Account)** - This is where I invest in stocks and shares. An API is available here: [[Trading 212 Public API]]
- **InvestEngine** - This is where my main portfolio of ETFs is and I invest monthly in this account. There is no API for this as of yet so scraping or manual import might be required.
- **NS&I (Bonds Investment)** - I invest monthly here to have a more safer investment in Bonds. There is no API for this too, so scraping or manual import might be required.
- **Lloyd Credit Card (Credit Card)** - I also have a Lloyds Credit Card which I use it to do my day to day spending. There's no API so scraping or manual import might be needed. Limit on this card is £4,500.
- **Amex Credit Card (Credit Card)** - I also have an American Express credit card with £9,990 limit which I also use for day to day spending. There's no API so scraping or manual import might be needed.
- **Barclaycard (Credit Card)** - I also have a Barclay card with £1,200 limit, and I don't really use this other than some automatic payments. There's no API so scraping or manual import might be required.


Above are all the places where my money lives, along with some of the places where it's offline. There is an offline vault in my house where I keep some stores of value, such as gold and cash in pounds. I want to make sure the system can also allow that to be entered manually.

I also have a car that I use, which is valuable, so I want to add it as an asset that contributes to my net worth.

To start with the system, I need to build out the foundations and structure for storing and managing this data effectively, so that in the future I can use it to make decisions on what to do next. Let's try to build it out.

### Features Required
**Spending Overview** - 
- Build a **Spending Overview** dashboard that shows:
    - Total spending per category (grouped into Needs / Wants / Investments if you define a mapping)
    - Month‑over‑month and year‑over‑year trends
    - Largest single transactions (to catch one‑off regrets like the PS5)
- Add **threshold alerts**: e.g., “Phone bill > £60 this month – check contract” or “Utility spend up 20% vs last year”.
- For **recurring subscriptions**, detect merchants that appear regularly and show a list of all active subscriptions with monthly cost. A spike (like the roaming pass) would immediately stand out.

**Income Tracker**
- Distinguish **income** transactions (positive amounts from salary, side‑hustle, interest, dividends) and aggregate them by source.
- Show a **Revenue mix** chart (salary, YouTube, dividends, bank interest).
- Set income targets and track against them.
- Use the aggregated net cash flow (income – spending) to suggest **excess cash** that could be moved to higher‑yield savings, invested in bonds, or used to overpay the student loan, just like Liam did.


**Networth calculation and tracking overtime**
- **Asset aggregation:** The system already knows the current balance of each bank/investment account (from API/scraper). Add a manual‑entry module for **offline assets** (gold, cash stash, car value, property equity, student loan balance) with a date stamp.
- **Snapshot engine:** Schedule a function (cron trigger in Cloudflare Workers) to record a net worth snapshot every month (or quarter) into a separate table: `net_worth_history (date, total_assets, total_liabilities, net_worth)`.
- Display a **net worth history chart** with projections (simple linear trend) and highlight major changes (bonus, market jumps, loan repayments).
- Use the ratio `(Investments + Savings) / Total Assets` to check if you’re meeting your goal (Liam targets >20% into future investments).

**Decision‑support engine (the “what’s next” part)**
- Build a **recommendation engine** that runs after each net worth snapshot or yearly review:
    - If cash in current accounts > 6 months of expenses, suggest moving surplus to InvestEngine/Trading212/NS&I.
    - If student loan interest > expected investment return, suggest overpaying the loan (as Liam did).
    - Compare your utility/phone bills against market benchmarks (if you integrate a price comparison API) and suggest switching.