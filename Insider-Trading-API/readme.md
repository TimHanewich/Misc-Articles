# Aletheia's Insider Trading API
Insider trading is the trading of a public company's stock or other securities based on material, nonpublic information about the company. In the eyes of the Securities Exchange Commission (SEC), an *insider* is considered a director, officer, or major shareholder (greater than 10% ownership). 

Since insider trading gives insiders a decisive competitive edge over comparitvely non-informed traders, there are laws and regulations in place to prevent abuse. In the United States, the Securities Exchange Commission is the governing body who regulates and enforces these anti-manipulation laws. One of the primary methods the SEC uses to regulate insider trading is requiring every insider to report their trades (buys, sells, grants, transfers, etc.) to the SEC through [Form 3, Form 4, and Form 5 filings](https://www.sec.gov/files/forms-3-4-5.pdf).

Tens of thousands of insider transactions are reported each day and flow through the SEC's EDGAR database. These filings are publicly available at the [SEC's website](https://www.sec.gov/edgar/searchedgar/companysearch.html) but are obscure, difficult to access, and not the easiest to interpret.

## Programmatic Access to Insider Trading Data
I created [Aletheia](https://aletheiaapi.com/) to collect these SEC filings, extract the transaction data, organize it into a meaningful structure, and provide access through a universally accessible web API ("Application Programming Interface").

*(diagram showing insider trading process flow)*

Full documentation for Aletheia's Insider Trading endpoints can be found [on the documentation page](https://aletheiaapi.com/docs). 

I will be highlighting the primary features of Aletheia's [*Latest Transactions*](https://aletheiaapi.com/docs/#latest-transactions) endpoint, which is the service that provides on-demand insider trading data.

## JSON Format
All of Aletheia's endpoints provides data in JSON (Javascript Object Notation) format, including the Insider Trading endpoint "*Latest Transactions"*. This commonly-used format makes it very easy for developers to manipulate data.

## Scope to a Company or Insider
You can request insider trades for any company or any individual. This means that you can request data that meets the following scenarios, for example:
- **All insider trades of Microsoft ($MSFT) common stock** - This will return trades of Microsoft stock by *any* insider.
- **All insider trades of Microsoft ($MSFT) common stock by CEO Satya Nadella**
- **All insider trades of Elon Musk (CIK 1494730)** - This will result in Elon Musk's trades of any company he is considered an insider of.

As you can see from the examples above, you can make your request company-centric, insider-centric, or a combination of both!