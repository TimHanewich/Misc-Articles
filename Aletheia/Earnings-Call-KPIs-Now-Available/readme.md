# Key Performance Indicators Now Available in Aletheia's "Earnings Calls" API Endpoint

IMG: https://www.completecontroller.com/wp-content/uploads/Key-Performance-Indicators-Complete-Controller-1.jpg

[Aletheia](https://aletheiaapi.com/)'s *Earnings Call* endpoint, providing Earnings Call transcripts over a free-to-use HTTP API, can also now provide **Key Performance Indicators**.

Each earnings call transcript is now being **analyzed by a sophisticated Natural Language Processing (NLP) engine** during the processing stage. A proprietary algorithm has been developed to **contextual** the content in each earnings call. The algorithm searches for line items, product lines, key metrics, and more, that were stated by management as having increased, decreased, reached a point, etc. Upon finding these examples, the algorithm parses the plain remark into three components: the *subject* (what is being talked about), the *status* (what happened to the subject), and the *value* (the dollar figure or percentage which the subject is being measured by).

When requested via Aletheia's [EarningsCall](https://aletheiaapi.com/docs/#earnings-call) endpoint, the Key Performance Indicator data will be included in the JSON response. The following as an example of how the *KeyPerformanceIndicator* property will be appended to the *SpokenRemark* array, listing the detected KPI's for this particular spoken remark:

```
{
    "SequenceNumber": 40,
    "Remark": "We also saw better-than-expected growth in large long-term Azure contracts against a very strong prior-year comparable. Nuance benefited bookings by roughly five points. Our on-premises transactional licensing revenue across both the Office and Server businesses was more negatively impacted than expected due to the transition from our open licensing program to our cloud solution provider program. Commercial remaining performance obligation increased 32% and 34% constant currency to $155 billion, with a roughly equivalent split which we that we'll recognize within and the portion beyond the next 12 months.",
    "SpokenBy": {
        "Id": "36d420f6-7924-4451-9735-4c2f7400afce",
        "Name": "Amy Hood",
        "Title": "Chief Financial Officer",
        "IsExternal": false
    },
    "KeyPerformanceIndicators": [
        {
            "subject": "commercial remaining performance obligation",
            "status": "1",
            "value": 0.32,
            "valueIsPercent": true,
            "subjectOffset": 400,
            "subjectLength": 43,
            "statusOffset": 444,
            "statusLength": 9,
            "valueOffset": 454,
            "valueLength": 3
            },
        {
            "subject": "commercial remaining performance obligation",
            "status": "1",
            "value": 155000000000.0,
            "valueIsPercent": false,
            "subjectOffset": 400,
            "subjectLength": 43,
            "statusOffset": 444,
            "statusLength": 9,
            "valueOffset": 487,
            "valueLength": 12
        }
    ]
}
```

For each KPI in the response, data about the **subject**, **status**, and **value** will be provided in multiple formats:

- **subject** - the plain text of the subject that was referred to during the call.
- **status** - code indicating the action/what happened to the subject.
    - **0** = "equals", "is", etc. For example, "revenue is $500,000."
    - **1** = "increased"
    - **2** = "decreased"
    - **3** = "Is greater than", "surpassed", "exceeded", "passed", etc.
    - **4** = "Is less than", "fell below", "sunk below"
    - **5** = "produced", "generated", "created", "caused". For example, "iPad contributed $100M in revenue."
- **value** - the dollar figure, percentage, quantity, that is mentioned. If a percentage is mentioned, it will be in a percentage format (i.e. 55% would be 0.55).
- **valueIsPercent** - if the value is a percentage, this will be true. If not, this will be false.
- **subjectOffset** - start location of the subject in the spoken remark.
- **subjectLength** - length (number of characters) of the subject.
- **statusOffset** - start location of the phrase that determined the status property in the spoken remark.
- **statusLength** - length of the phrase that determined the status property in the spoken remark.
- **valueOffset** - start location of the figure (number or percentage) that determined the value property in the spoken remark.
- **valueLength** - length of the figure (number or percentage) that determined the value property in the spoken remark.

Using the offset and length properties above, one could easily highlight the specific KPI components in the spoken remark in a graphical user interface.

## Free and Available over REST API
The service is free to use for private developers and is available over a REST API, meaning it is accessible from virtually any programming language or development stack. [Register for an account](https://aletheiaapi.com/login/) to start using the service now!