# Introducing Aletheia's Earnings Call API
Aletheia's new **Earnings Call** API service was made available for public consumption this week! In this article we will take a closer look at the service, what it can provide to you, and where it is heading.

You can find the documentation for the service on [Aletheia's documentation page](https://aletheiaapi.com/docs).

## The Earnings Call Service
The earnings call API service was built around the capability of providing 1) full earnings call transcripts in their entirety, and 2) cutting through the noise by providing you with the *most notable* quotes from each call.

As of the time of this writing, over 27,000 earnings calls are available for over 3,700 companies going back to 2017. Over 3.1 million spoken remarks have been recorded from over 86,000 executives and have been individually analyzed and extracted of notable sentiment, financial data, guidance, and more.

As mentioned above, the earnings calls are available for consumption in two formats which will be discussed below.

## Search the Earnings Call Database
[Documentation here](https://aletheiaapi.com/docs/#search-earnings-calls)
The first endpoint gives you the ability to search the database for earnings call transcripts that are available for consumption. For example, if you wanted to see what calls are available for Microsoft ($MSFT), you do that here.

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

Now that you know what is available, you can request content for this call with the next two endpoints.

## Earnings Call Transcript Content
[Documentation here](https://aletheiaapi.com/docs/#earnings-call)
This endpoint provides the raw earnings call content. If your desire is to simply access raw earnings call content through an API, this is your endpoint.

Each call to this service will provide you with the earnings call contextual data (what company it corresponds to, the date of the earnings call, etc) and a series of spoken remarks by call participants. Each spoken remark will contain contextual data about who said it, in what sequence it was spoken in, and the remark itself.

Example: `GET https://api.aletheiaapi.com/EarningsCall?company=msft&year=2020&quarter=q1&begin=8&end=11`

```
{
    "Id": "c3a97166-8881-4b4c-bf5d-21b0e24e2f56",
    "Year": 2020,
    "Period": 0,
    "Company": {
        "Id": "5372f27b-929c-45f6-8ae5-7404a59d307f",
        "Name": "Microsoft",
        "TradingSymbol": "MSFT"
    },
    "Remarks": [
        {
            "SequenceNumber": 8,
            "Remark": "Thank you, Mike. And thanks to everyone on the phone for joining. We are off to a strong start in fiscal 2020, delivering $33 billion in revenue this quarter. Our Commercial Cloud business continues to grow at scale, as we work alongside the world's leading companies to help them build their own digital capability. Microsoft provides a differentiated technology stack spanning application infrastructure, data and AI, developer tools and services, security and compliance, business process, productivity, and collaboration.",
            "SpokenBy": {
                "Id": "d1956458-ac06-4c74-89fa-0f499d4faeb5",
                "Name": "Satya Nadella",
                "Title": "Chief Executive Officer",
                "IsExternal": false
            }
        },
        {
            "SequenceNumber": 9,
            "Remark": "For each of these areas -- first, each of these areas represents secular long-term growth opportunity. Second, we are delivering best-in-class innovation and openness in each layer. And third, we offer unparalleled integration and architectural coherence across the entire stack to meet the real world needs of our customers.",
            "SpokenBy": {
                "Id": "d1956458-ac06-4c74-89fa-0f499d4faeb5",
                "Name": "Satya Nadella",
                "Title": "Chief Executive Officer",
                "IsExternal": false
            }
        },
        {
            "SequenceNumber": 10,
            "Remark": "Now I'll briefly highlight how we are accelerating our progress in innovation, starting with Azure. Organizations today need a distributed computing fabric to meet their real world operational sovereignty and regulatory needs. This quarter, we opened new datacenter regions in Germany and Switzerland, and in India, we are bringing the power of Microsoft Cloud to millions of small businesses through our partnership with Jio, one of the largest mobile carriers in the country.",
            "SpokenBy": {
                "Id": "d1956458-ac06-4c74-89fa-0f499d4faeb5",
                "Name": "Satya Nadella",
                "Title": "Chief Executive Officer",
                "IsExternal": false
            }
        },
        {
            "SequenceNumber": 11,
            "Remark": "Every Fortune 500 customer today is on a cloud migration journey, and we are making it faster and easier. Just this week, we announced an extensive go-to market partnership with SAP making Azure the preferred destination for every SAP customer. And our partnerships with VMware and Oracle also bring these ecosystems to our Cloud. We are extending beyond the cloud to the edge, enabling customers to get real-time insights where data is generated, while ensuring security and privacy. And we are seeing traction in every industry from Azure Sphere securely connecting Starbucks coffee machines to Azure Stack enabling scenarios from smart factories and modern compliant banking to mobile healthcare in remote areas.",
            "SpokenBy": {
                "Id": "d1956458-ac06-4c74-89fa-0f499d4faeb5",
                "Name": "Satya Nadella",
                "Title": "Chief Executive Officer",
                "IsExternal": false
            }
        }
    ]
}
```

The above example is requesting spoken remarks #8 through #11 (four total) of Microsoft's Q1 earnings call from Fiscal Year 2020. By manipulating the `begin` and `end` parameters of the request, you can paginate through the earnings call to access the full transcript.

## Earnings Call Highlights
[Documentation here](https://aletheiaapi.com/docs/#earnings-call-highlights)
The final endpoint is for those who want to cut through the noise of the earnings call and see the remarks that specifically are the most notable or important. At the time of transcript processing, Aletheia scans and analyzes every spoken remark and highlights the notable pieces for multiple categories. This means that you can request the most notable remarks regarding *revenue*, *cash flow*, *guidance*, *EPS*, and more.

Example: `GET https://api.aletheiaapi.com/EarningsCallHighlights?company=msft&year=2020&quarter=q1&category=7`

```
[
    {
        "Id": "94a6a131-2717-4dec-a1d4-cc8a2b3a06f0",
        "Remark": "Now back to overall company guidance. We expect COGS of $12.45 billion to $12.65 billion, and operating expenses of $10.8 billion to $10.9 billion. Other income and expense should be approximately $50 million, as interest income is partially offset by interest and finance lease expense. And finally, we expect our Q2 effective tax rate to be slightly above the full year rate of 17%.",
        "SpokenBy": {
            "Id": "e1481a76-0bdb-4c38-939d-dec0e8c7d39f",
            "Name": "Amy E. Hood",
            "Title": "Chief Financial Officer",
            "IsExternal": false
        },
        "Highlights": [
            {
                "BeginPosition": 28,
                "EndPosition": 35,
                "Category": 7,
                "Rating": 6.0
            }
        ]
    },
    {
        "Id": "90d960a6-1f7f-4a11-8578-9c6a18e1b186",
        "Remark": "Now to segment guidance. In Productivity and Business Processes, we expect revenue between $11.3 billion and $11.5 billion, driven by double-digit growth across Office Commercial, Dynamics and LinkedIn. For Intelligent Cloud, we expect revenue between $11.25 billion and $11.45 billion.",
        "SpokenBy": {
            "Id": "e1481a76-0bdb-4c38-939d-dec0e8c7d39f",
            "Name": "Amy E. Hood",
            "Title": "Chief Financial Officer",
            "IsExternal": false
        },
        "Highlights": [
            {
                "BeginPosition": 15,
                "EndPosition": 22,
                "Category": 7,
                "Rating": 6.0
            }
        ]
    },
    {
        "Id": "d759b58b-8cdc-420f-b189-2e9aee48f61d",
        "Remark": "And I think that's probably the unifying theme, quite frankly, of all the questions so far, which is what's next. What's next for us is in the apps and infra go from perhaps first innings to second innings. For data and AI to start the first innings. When it comes to security, compliance, we never participated in this. Guess what, we get to participate in a fairly competitive way now. We've Build, something that didn't even exist a few years ago, which is the workflow cloud. That's a huge opportunity for us. Biz Apps, we are a very competitive and growing footprint. Even when we think about something like Microsoft 365, we never participated in spite of our past success with all the first-line work, and now we get to participate in it. So I see long-term secular growth opportunities and we are going to stay focused on making sure our innovation is competitive in all those layers, we talked about.",
        "SpokenBy": {
            "Id": "d1956458-ac06-4c74-89fa-0f499d4faeb5",
            "Name": "Satya Nadella",
            "Title": "Chief Executive Officer",
            "IsExternal": false
        },
        "Highlights": [
            {
                "BeginPosition": 583,
                "EndPosition": 590,
                "Category": 7,
                "Rating": 1.0
            }
        ]
    },
    {
        "Id": "a706520e-10d2-4e03-997a-44f14365b819",
        "Remark": "One is the hybrid benefits. That is increasingly what is getting customers excited about the Azure choice and the fact that they can renew, knowing that they have the flexibility of both the cloud and the edge, that's definitely driving growth. Second is, we are also gaining share. When you think about what's happening even with the edge, some of the -- our data center addition products are very competitive in the marketplace. And so you see both of those effects but architecturally, we feel well placed. In fact at our Ignite Conference, you will see us even take the next leap forward even in terms of how we think about the architecture inclusive of the application models, programming models on what distributed computing looks like going forward. So we feel well positioned there.",
        "SpokenBy": {
            "Id": "d1956458-ac06-4c74-89fa-0f499d4faeb5",
            "Name": "Satya Nadella",
            "Title": "Chief Executive Officer",
            "IsExternal": false
        },
        "Highlights": [
            {
                "BeginPosition": 613,
                "EndPosition": 620,
                "Category": 7,
                "Rating": 1.0
            }
        ]
    }
]
```

In the above call we are requesting the most notable quotes related to **guidance** from Microsoft's Q1 2020 earnings call. Aletheia will provide the quote, the speaker details, and a list of highlights from that spoken remark. Each highlight indicates the beginning position and ending position of the notable portion of the remark. Each highlight will also contain a *Rating* property; this property serves as a rating of *how strongly* the language processing model believes this to be relevant to the category. 

The **category** parameter is an integer-based ID that represents a category of topics. You can find the categories [on the documentation page](https://aletheiaapi.com/docs/#earnings-call-highlights).