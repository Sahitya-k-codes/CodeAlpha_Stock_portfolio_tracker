# CodeAlpha_Stock_portfolio_tracker
import yfinance as yf
import pandas as pd

class StockPortfolio:
    def _init_(self):
        self.portfolio = pd.DataFrame(columns=['Ticker', 'Shares', 'Purchase Price'])

    def add_stock(self, ticker, shares, purchase_price):
        self.portfolio = self.portfolio.append({
            'Ticker': ticker, 'Shares': shares, 'Purchase Price': purchase_price
        }, ignore_index=True)

    def remove_stock(self, ticker):
        self.portfolio = self.portfolio[self.portfolio['Ticker'] != ticker]

    def get_real_time_data(self):
        tickers = self.portfolio['Ticker'].tolist()
        prices = {ticker: yf.Ticker(ticker).history(period="1d")['Close'][0] for ticker in tickers}
        return prices

    def portfolio_performance(self):
        real_time_prices = self.get_real_time_data()
        self.portfolio['Current Price'] = self.portfolio['Ticker'].map(real_time_prices)
        self.portfolio['Total Value'] = self.portfolio['Shares'] * self.portfolio['Current Price']
        self.portfolio['Gain/Loss (%)'] = (self.portfolio['Current Price'] - self.portfolio['Purchase Price']) / self.portfolio['Purchase Price'] * 100
        print(self.portfolio)

# Example usage:
portfolio = StockPortfolio()
portfolio.add_stock('AAPL', 10, 150)
portfolio.add_stock('TSLA', 5, 700)

portfolio.portfolio_performance()
