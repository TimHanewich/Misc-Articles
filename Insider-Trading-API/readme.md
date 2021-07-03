# Aletheia's Insider Trading API
Insider trading is the trading of a public company's stock or other securities based on material, nonpublic information about the company. In the eyes of the Securities Exchange Commission (SEC), an *insider* is considered a director, officer, or major shareholder (greater than 10% ownership). 

Since insider trading gives insiders a decisive competitive edge over comparitvely non-informed traders, there are laws and regulations in place to prevent abuse. In the United States, the Securities Exchange Commission is the governing body who regulates and enforces these anti-manipulation laws. One of the primary methods the SEC uses to regulate insider trading is requiring every insider to report their trades (buys, sells, grants, transfers, etc.) to the SEC through [Form 3, Form 4, and Form 5 filings](https://www.sec.gov/files/forms-3-4-5.pdf).

Tens of thousands of insider transactions are reported each day and flow through the SEC's EDGAR database. These filings are publicly available at the [SEC's website](https://www.sec.gov/edgar/searchedgar/companysearch.html) but are obscure, difficult to access, and not the easiest to interpret.

## Programmatic Access to Insider Trading Data
I created [Aletheia](https://aletheiaapi.com/) to collect these SEC filings, extract the transaction data, organize it into a meaningful structure, and provide access through a universally accessible web API ("Application Programming Interface").

