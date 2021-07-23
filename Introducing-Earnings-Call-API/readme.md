# Introducing Aletheia's Earnings Call API
Aletheia's new **Earnings Call** API service was made available for public consumption this week! In this article we will take a closer look at the service, what it can provide to you, and where it is heading.

You can find the documentation for the service on [Aletheia's documentation page](https://aletheiaapi.com/docs).

## Service Basics
The earnings call API service was built around the capability of providing 1) full earnings call transcripts in their entirety, and 2) cutting through the noise by providing you with the *most notable* quotes from each call. I will describe each option available below.

As of the time of this writing, over 27,000 earnings calls are available for over 3,700 companies going back to 2017. Over 3.1 million spoken remarks have been recorded from over 86,000 executives and have been individually analyzed and extracted of notable sentiment, financial data, guidance, and more.

As mentioned above, the earnings calls are available for consumption in two formats which will be discussed below.

## Search the Earnings Call Database
The first endpoint gives you the ability to search the earnings call database for earnings calls that are available for consumption. For example, if you wanted to see what calls are available for Microsoft ($MSFT), you do that here.

Example: `GET https://api.aletheiaapi.com/SearchEarningsCalls?company=msft&year=2020&top=5`

```
[
    {
        "Id": "e2ee094f-e34f-48e8-ae02-332057ca6798",
        "Company": {
            "Id": "5372f27b-929c-45f6-8ae5-7404a59d307f",
            "Name": "Microsoft",
            "TradingSymbol": "MSFT"
        },
        "Period": 3,
        "Year": 2020,
        "HeldAt": "2020-07-22T00:00:00"
    },
    {
        "Id": "d3dbcc49-b774-4055-840b-51e7853d7a84",
        "Company": {
            "Id": "5372f27b-929c-45f6-8ae5-7404a59d307f",
            "Name": "Microsoft",
            "TradingSymbol": "MSFT"
        },
        "Period": 2,
        "Year": 2020,
        "HeldAt": "2020-04-29T00:00:00"
    },
    {
        "Id": "c1d2bba5-d502-44d5-b16d-a9bfb77cfafd",
        "Company": {
            "Id": "5372f27b-929c-45f6-8ae5-7404a59d307f",
            "Name": "Microsoft",
            "TradingSymbol": "MSFT"
        },
        "Period": 1,
        "Year": 2020,
        "HeldAt": "2020-01-29T00:00:00"
    },
    {
        "Id": "c3a97166-8881-4b4c-bf5d-21b0e24e2f56",
        "Company": {
            "Id": "5372f27b-929c-45f6-8ae5-7404a59d307f",
            "Name": "Microsoft",
            "TradingSymbol": "MSFT"
        },
        "Period": 0,
        "Year": 2020,
        "HeldAt": "2019-10-23T00:00:00"
    }
]
```

The request above searched the database for earnings calls for Microsoft ($MSFT) in fiscal year 2020. All four quarters are available!