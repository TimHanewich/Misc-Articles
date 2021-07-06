# Aletheia's Insider Trading API
Insider trading is the trading of a public company's stock or other securities based on material, nonpublic information about the company. In the eyes of the Securities Exchange Commission (SEC), an *insider* is considered a director, officer, or major shareholder (greater than 10% ownership). 

Since insider trading gives insiders a decisive competitive edge over comparatively non-informed traders, there are laws and regulations in place to prevent abuse. In the United States, the Securities Exchange Commission is the governing body who regulates and enforces these anti-manipulation laws. One of the primary methods the SEC uses to regulate insider trading is requiring every insider to report their trades (buys, sells, grants, transfers, etc.) to the SEC through [Form 3, Form 4, and Form 5 filings](https://www.sec.gov/files/forms-3-4-5.pdf).

Thousands of insider transactions are reported to the SEC each day. These filings are publicly available at the [SEC's website](https://www.sec.gov/edgar/searchedgar/companysearch.html) but are obscure, difficult to access, and not the easiest to interpret.

## Programmatic Access to Insider Trading Data
I created [Aletheia](https://aletheiaapi.com/) to collect these SEC filings, extract the transaction data, organize it into a meaningful structure, and provide access through a universally accessible web API ("Application Programming Interface").

*(diagram showing insider trading process flow)*

The diagram above depicts how Aletheia's servers perform the acquisition of data, processing of data, storing, and making it available through the REST API.

Full documentation for Aletheia's Insider Trading endpoints can be found [on the documentation page](https://aletheiaapi.com/docs). 

I will be highlighting the primary features of Aletheia's [*Latest Transactions*](https://aletheiaapi.com/docs/#latest-transactions) endpoint, which is the service that provides on-demand insider trading data.

## JSON Format
All of Aletheia's endpoints provides data in JSON (JavaScript Object Notation) format, including the Insider Trading endpoint "*Latest Transactions"*. This commonly used format makes it very easy for developers to manipulate data and use it in their application.

The following is an example response:
```
[
    {
        "Id":"e3fd7062-5190-43a5-9aa0-26ce8ce2ddea",    // The unique ID of this transaction in the Aletheia database
        "FromFiling":"c42b0232-45d3-48e6-8f27-b97621947d3b",    // The unique ID of the SEC filing in the Aletheia database which this transaction stemmed from
        "EntryType":0,  // 0 = Transaction, 1 = Holding (no changes, just reporting a held asset)
        "QuantityOwnedFollowingTransaction":0.0,    // The quantity of securities the owner owns following this transaction
        "DirectIndirect":1,     // 0 = Direct ownership, 1 = Indirect ownership
        "SecurityTitle":"Common Stock",
        "SecurityType":0,   // 0 = non-derivative (equity based), 1 = derivative (i.e. options contract)
        "AcquiredDisposed":1,   // 0 = acquired, 1 = disposed
        "Quantity":50.0,    // The quantity of securities acquired/disposed of
        "PricePerSecurity":null,    // if applicable, the average acquisition or disposition price of the security
        "TransactionDate":"2021-02-17T00:00:00",    // recorded transaction date
        "TransactionCode":1,    // (see the "transactiontype" parameter above)
        "ConversionOrExercisePrice":null,   // if a derivative security, the exercise price of this contract
        "ExercisableDate":null,     // if a derivative security, the exercisable date of this contract
        "ExpirationDate":null,     // if a derivative security, the expiration date of this contract
        "UnderlyingSecurityTitle":null,     // if a derivative security, the title of the contract's underlying security
        "UnderlyingSecurityQuantity":null   // if a derivative security, the quantity of the contract's underlying security
    },
    {
        "Id":"bf6b3afc-2591-48e6-8dda-0b62903fd38d",
        "FromFiling":"01bf4617-5f19-4319-b378-c16a9d12190a",
        "EntryType":0,
        "QuantityOwnedFollowingTransaction":1508.4543,
        "DirectIndirect":0,
        "SecurityTitle":"Common Stock",
        "SecurityType":0,
        "AcquiredDisposed":0,
        "Quantity":289.0,
        "PricePerSecurity":null,
        "TransactionDate":"2021-02-05T00:00:00",
        "TransactionCode":7,
        "ConversionOrExercisePrice":null,
        "ExercisableDate":null,
        "ExpirationDate":null,
        "UnderlyingSecurityTitle":null,
        "UnderlyingSecurityQuantity":null
    },
    {
        "Id":"d24bc278-dbc6-4d7f-a144-69bdcd81ad38",
        "FromFiling":"d5ba901e-5002-413c-9358-b3445b66ccae",
        "EntryType":0,
        "QuantityOwnedFollowingTransaction":252.47,
        "DirectIndirect":0,
        "SecurityTitle":"Common Stock",
        "SecurityType":0,
        "AcquiredDisposed":0,
        "Quantity":252.47,
        "PricePerSecurity":null,
        "TransactionDate":"2021-02-05T00:00:00",
        "TransactionCode":3,
        "ConversionOrExercisePrice":null,
        "ExercisableDate":null,
        "ExpirationDate":null,
        "UnderlyingSecurityTitle":null,
        "UnderlyingSecurityQuantity":null
    },
]
```

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

Example response with all contextual data included (Elon Musk purchasing $TSLA stock):
```
[
    {
        "Id": "54fc3cf0-edf2-44a1-8cca-7fdeab9fbebb",
        "FromFiling": "19025d3c-0858-400a-bfad-675f759e1ce1",
        "_FromFiling": {
            "Id": "19025d3c-0858-400a-bfad-675f759e1ce1",
            "FilingUrl": "https://www.sec.gov/Archives/edgar/data/1318605/000149473020000001/0001494730-20-000001-index.htm",
            "AccessionP1": 1494730,
            "AccessionP2": 20,
            "AccessionP3": 1,
            "FilingType": 0,
            "ReportedOn": "2020-02-14T00:00:00",
            "Issuer": 1318605,
            "_Issuer": {
                "Cik": 1318605,
                "Name": "TESLA MOTORS INC",
                "TradingSymbol": "TSLA"
            },
            "Owner": 1494730,
            "_Owner": {
                "Cik": 1494730,
                "Name": "Musk Elon",
                "TradingSymbol": null
            }
        },
        "EntryType": 0,
        "QuantityOwnedFollowingTransaction": 34098596.0,
        "DirectIndirect": 1,
        "SecurityTitle": "Common Stock",
        "SecurityType": 0,
        "AcquiredDisposed": 0,
        "Quantity": 13037.0,
        "PricePerSecurity": null,
        "TransactionDate": "2020-02-14T00:00:00",
        "TransactionCode": 0,
        "ConversionOrExercisePrice": null,
        "ExercisableDate": null,
        "ExpirationDate": null,
        "UnderlyingSecurityTitle": null,
        "UnderlyingSecurityQuantity": null
    }
]
```

Use the *cascade* parameter of the *Latest Transactions* endpoint to return all contextual data in the same response.

## It is FREE!
You can use all of Aletheia's API services for absolutely no cost. 
Set up a [free developer account] to begin using the service today. Visit Aletheia's [API documentation page](https://aletheiaapi.com/docs/) to quickly get up and running with the service.