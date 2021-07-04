# Aletheia's Insider Trading API
Insider trading is the trading of a public company's stock or other securities based on material, nonpublic information about the company. In the eyes of the Securities Exchange Commission (SEC), an *insider* is considered a director, officer, or major shareholder (greater than 10% ownership). 

Since insider trading gives insiders a decisive competitive edge over comparitvely non-informed traders, there are laws and regulations in place to prevent abuse. In the United States, the Securities Exchange Commission is the governing body who regulates and enforces these anti-manipulation laws. One of the primary methods the SEC uses to regulate insider trading is requiring every insider to report their trades (buys, sells, grants, transfers, etc.) to the SEC through [Form 3, Form 4, and Form 5 filings](https://www.sec.gov/files/forms-3-4-5.pdf).

Thousands of insider transactions are reported to the SEC each day. These filings are publicly available at the [SEC's website](https://www.sec.gov/edgar/searchedgar/companysearch.html) but are obscure, difficult to access, and not the easiest to interpret.

## Programmatic Access to Insider Trading Data
I created [Aletheia](https://aletheiaapi.com/) to collect these SEC filings, extract the transaction data, organize it into a meaningful structure, and provide access through a universally accessible web API ("Application Programming Interface").

*(diagram showing insider trading process flow)*

The diagram above depicts how Aletheia's servers perform the acquisition of data, processing of data, storing, and making it available through the REST API.

Full documentation for Aletheia's Insider Trading endpoints can be found [on the documentation page](https://aletheiaapi.com/docs). 

I will be highlighting the primary features of Aletheia's [*Latest Transactions*](https://aletheiaapi.com/docs/#latest-transactions) endpoint, which is the service that provides on-demand insider trading data.

## JSON Format
All of Aletheia's endpoints provides data in JSON (Javascript Object Notation) format, including the Insider Trading endpoint "*Latest Transactions"*. This commonly-used format makes it very easy for developers to manipulate data and use it in their application.

## Scope to a Company or Insider
You can request insider trades for any company or any individual. This means that you can request data that meets the following scenarios, for example:
- **All insider trades of Microsoft ($MSFT) common stock** - This will return trades of Microsoft stock by *any* insider.
- **All insider trades of Microsoft ($MSFT) common stock by CEO Satya Nadella**
- **All insider trades of Elon Musk (CIK 1494730)** - This will result in Elon Musk's trades of any company he is considered an insider of.

As you can see from the examples above, you can make your request company-centric, insider-centric, or a combination of both!

Use the *issuer* and *owner* parameters of the *Latest Transactions* endpoint to filter to transactions only occuring for a specific company or insider.

## Filter to a Particular Date
Since it became mandatory to report insider trading to the SEC, each company has amassed thousands of filings reporting these events. On average, 1,162 insider transactions per company in the S&P 500 from 2010-2019. Aletheia allows you to focus on transactions during a specific date in time.

Use the *before* parameter of the *Latest Transactions* endpoint to filter to transactions only occuring before a certain date.

## Filter to a Security Type
Insiders are required to report transactions of both equity-based and derivative-based securities. Aletheia provides the ability to filter to only one type of security. This is convenient as investors are more commonly interested in the equity-based securities (common stock).

Use the *securitytype* parameter of the *Latest Transactions* endpoint to filter to a particular security type.

## Filter to a Transaction Type
There are twenty different transaction types that insiders can report. Ranging from an open market purchase to a exercise of out-of-the-money derivative, there seems to be no shortage of ways an insider can acquire or dispose of securities.

You can filter the returned response to transactions of only a certain type - for example, only purchases on the public market. Or perhaps you have interest in stock sales. Or perhaps a stock grant or award that was given to the insider by the company? You can easily narrow down the results to specifically what you are interested in seeing.

Use the *transactiontype* parameter of the *Latest Transactions* endpoint to filter to a specific transaction type.

## Receive All Contextual Data in One Request
There is no shortage of peripheral data related to each transaction - the full name of the insider making the trade, name of the company, company's trading symbol, any associated title that the insider holds at the company, etc. 

You can request all of this information from Aletheia and have this returned to you *in the same call*.

Use the *cascade* paramter of the *Latest Transactions* endpoint to return all contextual data in the same response.

## It is FREE!
You can use all of Aletheia's API services for absolutely no cost. 
Set up a [free developer account] to begin using the service today. Visit Aletheia's [API documentation page](https://aletheiaapi.com/docs/) to quickly get up and running with the service.