# Introducing Insider Trading Webhooks on Aletheia API

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
The code snipped above depicts the webhook registration POST call to the Aletheia API service. 