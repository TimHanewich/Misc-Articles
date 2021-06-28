# Introducing the Insider Trading Webhook on Aletheia API

This past week I brought [Aletheia's new Insider Trading Webhook service](https://aletheiaapi.com/docs/#insider-trading-webhook) to general availability. This webhook provides any developer of any technology the ability to register their unique URL endpoint. The service is free to use! In this article I will be covering this new webhook and it's abilities.

## Specific Subscription Registration
The new Insider Trading webhook provides you the ability to be specific with what types of insider trading activity you would like to be notified of. 

Insider trades only within Facebook ($FB)? Buys only? Publicly traded common stock or derivative contracts? Stock grants? Or a combination of all criteria? All of this is possible with Aletheia's flexible registration process. You can be as specific as you want, or simply ask to be notified of **all** insider trades at **any** company.

As a developer, you probably know that the first step in subscribing to webhook is to provide your URL endpoint to the service.  You will specify these details during the **webhook registration** step.

```
POST https://api.aletheiaapi.com/SubscribeToInsiderTradingWebhook
{
    "endpoint":"https://www.myservice.net/myendpoint",
    "issuer": "MSFT",
    "owner": 1513142,
    "transactionType": 1,
    "securityType": 0
}
```
The code snipped above depicts the webhook registration POST call to the Aletheia API service. As seen above, you can easily specify what trades you would like to be notified of. The above body is asking for trades of **Microsoft ($MSFT)** stock by **Satya Nadella**, the company's CEO, (SEC-assigned CIK ID 1513142), and only **sales** of **equity-based** securities.

If you don't want to be as narrow as any of these specifications, simply set the property values as **null** or just **omit them entirely**. For example, if you omit the *issuer* property, you will get insider transaction notifications for any transaction, regardless of what company it is for.

The available *transactionType* and *securityType* values can be found [here](https://aletheiaapi.com/docs/#latest-transactions).

## Detailed Insider Trading Data Payload
Following registration, you will start receiving calls from the Aletheia webhook when insider trades occur that meet your subscription criteria. Below is an example the data payload that would be supplied to you:
```
{
  "SecEntities": [
    {
      "Cik": 6845,
      "Name": "APOGEE ENTERPRISES, INC.",
      "TradingSymbol": "APOG"
    },
    {
      "Cik": 1159191,
      "Name": "JOHNSON GARY ROBERT",
      "TradingSymbol": null
    }
  ],
  "SecFilings": [
    {
      "Id": "de3bc575-5aca-45d7-9b30-f26551a789e4",
      "FilingUrl": "https://www.sec.gov/Archives/edgar/data/1159191/000120919121043441/0001209191-21-043441-index.htm",
      "AccessionP1": 1209191,
      "AccessionP2": 21,
      "AccessionP3": 43441,
      "FilingType": 0,
      "ReportedOn": "2021-06-25T00:00:00",
      "Issuer": 6845,
      "_Issuer": null,
      "Owner": 1159191,
      "_Owner": null
    }
  ],
  "HeldOfficerPositions": [
    {
      "Id": "a2eefaeb-848d-4b62-84ba-9e27c63738e7",
      "Officer": 1159191,
      "Company": 6845,
      "PositionTitle": "Treasurer & Sr Vice President",
      "ObservedOn": "de3bc575-5aca-45d7-9b30-f26551a789e4"
    }
  ],
  "SecurityTransactionHoldings": [
    {
      "Id": "904207c6-5f6a-4e82-a7dc-0a4972b86e25",
      "FromFiling": "de3bc575-5aca-45d7-9b30-f26551a789e4",
      "_FromFiling": null,
      "EntryType": 0,
      "QuantityOwnedFollowingTransaction": 41294,
      "DirectIndirect": 0,
      "SecurityTitle": "Common Stock",
      "SecurityType": 0,
      "AcquiredDisposed": 1,
      "Quantity": 2430,
      "PricePerSecurity": 41.9792,
      "TransactionDate": "2021-06-25T00:00:00",
      "TransactionCode": 1,
      "ConversionOrExercisePrice": null,
      "ExercisableDate": null,
      "ExpirationDate": null,
      "UnderlyingSecurityTitle": null,
      "UnderlyingSecurityQuantity": null
    }
  ]
}
```
- The SecEntities property lists the entities related in these transactions. This will contain the company (with a trading symbol) and insider that made this transaction.
- The SecFilings property lists the SEC filings that these insider transactions originated from. This way you can go back to the SEC filing yourself!
- The HeldOfficerPositions property will list the titles (if any) that are carried by the insider. For example, if the insider was titled "CEO" in the filing document, you would see that held title here.
- The SecurityTransactionHoldings lists the insider trades (or reported transactions) that that were reported in the filing and therefore triggered your webhook subscription.

The ID properties provides you the ability to trace back an associate between a transaction and an SEC filing, an owner and a position title, a relationship between SEC filing and a person/company, etc.

## Easily Unsubscribe
It is extremely easy to formerly unsubscribe your endpoint from the webhook! When you subscribed to the webhook, a URL that is specific to your endpoint will be provided in the headers. This property is labeled "Location" and will look like this:
```
https://api.aletheiaapi.com/UnsubscribeWebhook?id=dd150c44-c42a-4cdd-92f5-b78a23b36c72
```
Simply make a **DELETE** call to this specified URL to have your webhook subscription removed.
Lost that request? No problem! Make a call to the same endpoint but instead provide your subscribed URL to the *endpoint* property. For example:
```
https://api.aletheiaapi.com/UnsubscribeWebhook?endpoint=https://www.myservice.net/myendpoint
```

## Instantanous Notifications
This service is guarenteed to give you an edge when it comes to being in the know about insider trading activity. Our notifications of insider trading is near-instanteneous. You will be notified of new insider trades within seconds of the trades being reported to the Securities Exchange Commission.

## The service is **FREE**
You can use this webhook service at no cost. You can register for a developer account [here](https://aletheiaapi.com/login/) and start consuming [Aletheia's API service](https://aletheiaapi.com/docs/) for free within minutes.

## Air-Tight Documentation
Aletheia also provides descriptive documentation for each endpoint service, and that includes the Insider Trading Webhook. You can find the documentation for the insider trading webhook [here](https://aletheiaapi.com/docs/#insider-trading-webhook). The documentation describes the three steps to using the service and provides sample payloads.