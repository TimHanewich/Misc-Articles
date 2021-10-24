# Death Cross Webhook Now Available by Aletheia

A *Death Cross* in finance occurs when a stock's short term moving average sinks below it's long term moving average. Typically this is a *50-day* moving average for short-term and a *200-day* moving average for long term. The death cross is a tecnical chart pattern which may indicate the potential for a major selloff of a stock. This metric along has proven to be a reliable indicator of many severe bear markets of the past century.

For both opportunistic traders and day traders alike, it is important to be aware of death cross events shortly after they occur. This allows investors to stay ahead of the crowd with their trades or prepare for an upcoming buying opportunity.

[Aletheia](https://aletheiaapi.com) now provides a [Death Cross Webhook](https://aletheiaapi.com/docs/#death-cross-webhook) that will notify your web service or application upon a Death Cross event occuring. 

## Both the Bearish and Bullish Cross Type
Aletheia's Death Cross Webhook is capable of notifying you of both *bearish* and *bullish* death crosses.
- A *bearish* death cross is when the 50-day moving average sinks below the 200-day moving average (a bearish indicator)
- A *bullish death cross occurs when the 50-day moving average rises above the 200-day moving average (returning to bullish territory)

## Granular Subscription Specification
When subscribing to the Death Cross Webhook, you are able to fine-tune your subscription to only a specific stock or only a specific death cross type. For example, if you only wanted to be notified of events in which $TSLA sinks into bearish territory (the short term MA sinks below the long term MA), this is possible! Or perhaps you only want to be notified when $GE returns to bullish territory (when the short term MA rises above the long term MA). 

Alternatively, you can not specify a stock or death cross type and simple be notified of all death cross events across the market.

## Subscribing to and Consuming the Death Cross Webhook Service
The documentation [on Aletheia's documentation page](https://aletheiaapi.com/docs/#death-cross-webhook) explains the subscription process very well. It is a simple three step process: 1) subscribe your endpoint to the service, 2) receive HTTP post calls when death crosses occur, and 3) unsubscribe your endpoint when you wish to unsubscribe. That's it!

Example notification payload (body of POST request to your web service):
```
{
    "Stock": "ALLE",
    "CrossType": 0
}
```
In the example above, the stock you are being notified about is $ALLE, Allegion. A **CrossType** of 0 is a bearish cross while a **CrossType** of 1 is a bullish cross (short term moving average rose above long term).