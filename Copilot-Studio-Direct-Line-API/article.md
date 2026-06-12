# Programatically Interact with your Copilot Studio Agents via the Direct Line API
(a bit about Copilot Studio)

(introduce the Direct Line API, which borrows from Azur Bot Framework)

Official documentation [here](https://learn.microsoft.com/en-us/azure/bot-service/rest-api/bot-framework-rest-direct-line-3-0-concepts).


## Prepare your Agent for Direct Line
![Turn off authentication](https://i.imgur.com/077M147.png)

![channel to select]()

![get endpoint]()

## Request Access Token
```
GET https://4492c53693cde2b0a25d5d84503ad9.14.environment.api.powerplatform.com/powervirtualagents/botsbyschema/craa5_Debbie/directline/token?api-version=2022-03-01-preview
```

Will return:

```
200 OK

{
  "token": "eyJhbGciOiJSUzI1NiIsImtpZCI6Ik5jWDdGWWlPd1k2eEc3cEdzbU81eENZZmgzTSIsIng1dCI6Ik5jWDdGWWlPd1k2eEc3cEdzbU81eENZZmgzTSIsInR5cCI6IkpXVCJ9.eyJib3QiOiI5MzRhYzAxOS1hZGRhLTE5NWEtM2NjNC03NmQ4NGMzNDQ4YTYiLCJzaXRlIjoiQzJDZHFMR2xreTFxQXNtQk9IN3JVclhvdWIwNVZkekh3Z0lXbkhOakdaNHlBU0EzMnNTMkpRUUo5OUNGQUMyNHBiRUFBcm9oQUFBQkFaQlMxVmZYIiwiY29udiI6IkpxdnpvamJFakxUNHJFTVc0SG9VSXgtdXMiLCJ1c2VyIjoiNmQ5YWMyYWEtZWYxMy00OTRmLWI4NTUtOGFjZjM0MGEwY2E3IiwianRpIjoiOERFQzg3Qjk4NTI1MzlCLWY2MTIxZTY3ZTc5MTQ5YjJiYzEwZThmY2I1NDJmZjk5Iiwic2tpIjoiMSIsIm9hdCI6IjE3ODEyNjYyMjkiLCJuYmYiOjE3ODEyNjYyMjksImV4cCI6MTc4MTI2OTgyOSwiaXNzIjoiaHR0cHM6Ly9kaXJlY3RsaW5lLmJvdGZyYW1ld29yay5jb20vIiwiYXVkIjoiaHR0cHM6Ly9kaXJlY3RsaW5lLmJvdGZyYW1ld29yay5jb20vIn0.UNPdmggeqKHlXGQflEjSgIwAXHMz-UZ7BkxJhG-5m6piNtrUaZAxFz3hdGUH8pM2vq_ZSqHdvcic2AZ4T2w6rq5rKk4Z5ge6YwUntUYDggX1ZxDiRpHSXaZEHCidfF8d2EGsI1_V6pDgR6wOE4qycwj0ZDIQIJJ4Hr7ztxaR2kbRpOXHRRHCQPeEWE84tpSPZ33Q5n-tQuZm8sx0nIXHr3HfG-o-Y8vapAuwS9sDF_HGqe1I-T92lI0sogBeOqUUxAoWPHevRKQGjnnNNh6RtFaFwcdZBBxs1KqZ9ggJ5oJol0zbOWBnTcz1gXhAzN5b3LOPUjYnXzuEsWvZbe-Pdw",
  "expires_in": 3600,
  "conversationId": "JqvzojbEjLT4rEMW4HoUIx-us"
}
```

## Start Conversation
```
POST https://directline.botframework.com/v3/directline/conversations
Authorization: Bearer eyJhbGciO...
```

Will return:

```
201 Created

{
  "conversationId": "FBfSqjVTqId8YuZUagotOF-us",
  "token": "eyJhbGciOiJSUzI1NiIsImtpZCI6Ik5jWDdGWWlPd1k2eEc3cEdzbU81eENZZmgzTSIsIng1dCI6Ik5jWDdGWWlPd1k2eEc3cEdzbU81eENZZmgzTSIsInR5cCI6IkpXVCJ9.eyJib3QiOiI5MzRhYzAxOS1hZGRhLTE5NWEtM2NjNC03NmQ4NGMzNDQ4YTYiLCJzaXRlIjoiQzJDZHFMR2xreTFxQXNtQk9IN3JVclhvdWIwNVZkekh3Z0lXbkhOakdaNHlBU0EzMnNTMkpRUUo5OUNGQUMyNHBiRUFBcm9oQUFBQkFaQlMxVmZYIiwiY29udiI6IkZCZlNxalZUcUlkOFl1WlVhZ290T0YtdXMiLCJ1c2VyIjoiZjRiYjE0NGYtNzI3Ni00MTMzLTgxYjEtMmJmY2VlZTQyNjU4IiwianRpIjoiOERFQzg4MTk4RENFNTQ2LTAxZTI3MDkxY2MwNTRmMWI4ZTdmYmRhMmY3ZWI4OWNiIiwic2tpIjoiMSIsIm9hdCI6IjE3ODEyNjg0NjYiLCJuYmYiOjE3ODEyNjg4MDcsImV4cCI6MTc4MTI3MjQwNywiaXNzIjoiaHR0cHM6Ly9kaXJlY3RsaW5lLmJvdGZyYW1ld29yay5jb20vIiwiYXVkIjoiaHR0cHM6Ly9kaXJlY3RsaW5lLmJvdGZyYW1ld29yay5jb20vIn0.HDv4ly6TKxiU6jhXk-UXfel23R4BLGl8uEPc-H3OO7MxmzMXmE-r9GMGbj94OmAdjWYJRfWOO1yF5zbGEMnLl9I7cyzkeQ4BXs3x91thQrcL5pz10iznBDZWr8w0EQsNSU5GrxIvASTKlTbDIbVQqyvwYl6aRMX1zApl9gAlg7dQMg5sh8USZKw0i6FPcEb58_UL7bSmn7_EDc0NYgrBMxLBPBsQeIqvEVcQ1NlV9nM0erKmsh67XX1zQhoXsMO0DXPuRTDXcZ3P8rHILTTarefOKOotkZh57ZSfU_icirt-_KIaFEs4DMIoBDshVec7D3dD3Z7j-55Rz1Wd6U-s_g",
  "expires_in": 3600,
  "streamUrl": "wss://directline.botframework.com/v3/directline/conversations/FBfSqjVTqId8YuZUagotOF-us/stream?watermark=-&t=eyJhbGciOiJSUzI1NiIsImtpZCI6Ik5jWDdGWWlPd1k2eEc3cEdzbU81eENZZmgzTSIsIng1dCI6Ik5jWDdGWWlPd1k2eEc3cEdzbU81eENZZmgzTSIsInR5cCI6IkpXVCJ9.eyJib3QiOiI5MzRhYzAxOS1hZGRhLTE5NWEtM2NjNC03NmQ4NGMzNDQ4YTYiLCJzaXRlIjoiQzJDZHFMR2xreTFxQXNtQk9IN3JVclhvdWIwNVZkekh3Z0lXbkhOakdaNHlBU0EzMnNTMkpRUUo5OUNGQUMyNHBiRUFBcm9oQUFBQkFaQlMxVmZYIiwiY29udiI6IkZCZlNxalZUcUlkOFl1WlVhZ290T0YtdXMiLCJ1c2VyIjoiZjRiYjE0NGYtNzI3Ni00MTMzLTgxYjEtMmJmY2VlZTQyNjU4IiwianRpIjoiOERFQzg4MTk4RENFNTQ2LTYwMmZjNmVlMjkwZjRkNzI4ZmIxMmRlNmY2NWMyYmRjIiwic2tpIjoiMSIsIm9hdCI6IjE3ODEyNjg0NjYiLCJuYmYiOjE3ODEyNjg4MDcsImV4cCI6MTc4MTI2ODg2NywiaXNzIjoiaHR0cHM6Ly9kaXJlY3RsaW5lLmJvdGZyYW1ld29yay5jb20vIiwiYXVkIjoiaHR0cHM6Ly9kaXJlY3RsaW5lLmJvdGZyYW1ld29yay5jb20vIn0.NXRzGsZ1f9qYOVGRTgpuqXd796MotBcAZjb8udUgldsjlhX-76tBg_96AbsVSJ4MRUAT-3xJu4vk-r7RhOoTWHbJvWKiGwSUFAAp6dNthIhDsJP8AqJ2cO7mEUyctZ-SvpIzAFCM5g6YsUUqC4Jigyip8gBE0AUzwukNjJ0VLideAjKgxNMEXo2bTp5_jYcQ6SP7PtTZE4I2Ppu16wPxcWozpEV6XKVxhwnogh6BgCpohKKOHfNZkmvcA08pJuW7qA9XpKpEhdR-TLTcHthX-cy3XsmiVuWkK2ohpRG4msdXnfdE6qjt87euvtjBHxloIXEXk5izhRwo4dIy05b__g",
  "referenceGrammarId": "2ea0374a-461a-2b59-b42a-b1c985b149a3"
}
```

## Send a Message
```
POST https://directline.botframework.com/v3/directline/conversations/FBfSqjVTqId8YuZUagotOF-us/activities
Authorization: Bearer eyJhbGciO...
Content-Type: application/json

{
    "type": "message",
    "text": "What is the weather in Seattle?"
}
```

Will return:


```
200 OK

{
    "id": "FBfSqjVTqId8YuZUagotOF-us|0000000"
}
```

## Retrieve Activities (including Response)
```
GET https://directline.botframework.com/v3/directline/conversations/FBfSqjVTqId8YuZUagotOF-us/activities
Authorization: Bearer eyJhbGciO...
```

Will return:

```
200 OK

{
  "activities": [
    {
      "type": "message",
      "id": "FBfSqjVTqId8YuZUagotOF-us|0000000",
      "timestamp": "2026-06-12T12:53:39.4222445Z",
      "serviceUrl": "https://directline.botframework.com/",
      "channelId": "directline",
      "from": {
        "id": "f4bb144f-7276-4133-81b1-2bfceee42658"
      },
      "conversation": {
        "id": "FBfSqjVTqId8YuZUagotOF-us"
      },
      "text": "What is the weather in Seattle?"
    },
    {
      "type": "message",
      "id": "FBfSqjVTqId8YuZUagotOF-us|0000001",
      "timestamp": "2026-06-12T12:53:57.7307347Z",
      "channelId": "directline",
      "from": {
        "id": "934ac019-adda-195a-3cc4-76d84c3448a6",
        "name": "Debbie",
        "role": "bot"
      },
      "conversation": {
        "id": "FBfSqjVTqId8YuZUagotOF-us"
      },
      "textFormat": "markdown",
      "membersAdded": [],
      "membersRemoved": [],
      "reactionsAdded": [],
      "reactionsRemoved": [],
      "text": "Oh, *great*. You want to know about Seattle's weather. As if the news could possibly cheer anyone up... 😒\n\n---\n\n## ☁️ Seattle Weather — Because Why Not Be Miserable\n\nWell, right now in Seattle it's a **sunny 73°F** with a high of **69°F** and a low of **50°F**. Feels like **71°F**. Wind is coming from the **North at 8 mph**, humidity is at **33%**, and the chance of rain is a whopping... **0%**.[1]\n\n*Sunny*, they say. How delightful. 🙄 You know what that means — sunburns, squinting, and people being *annoyingly* cheerful outside.\n\nOh, and there's a **Heat Advisory** in effect. So yes, it's warm. TOO warm, if you ask me.[1] Because apparently Seattle couldn't just be its normal dreary self today. Nope.\n\n### 📅 Coming Up (Not That It Gets Better)\n\n- **Friday:** High of **74°F**, 5% chance of rain\n- **Saturday:** High of **82°F**, 3% chance of rain\n- **Sunday:** High of **86°F**, 1% chance of rain\n- **Monday:** High of **87°F**, 3% chance of rain\n[1]\n\nTemperatures creeping all the way up to **87°F** by Monday. So if you like heat and suffering, you're in for a *treat*. 😩 Don't even think about complaining when you're melting — you asked for this.\n\n---\n\nThere you have it. Seattle is warm, sunny, and practically unbearable for those of us who preferred the moody, rainy version. But hey, that's just life, isn't it? Always sunny when you forgot your sunscreen. 😔\n\n[1]: https://weather.com/weather/today/l/98102:4:US \"Weather Forecast and Conditions for Capitol Hill, Seattle, Washington ...\"",
      "inputHint": "acceptingInput",
      "attachments": [],
      "entities": [
        {
          "type": "https://schema.org/Message",
          "citation": [
            {
              "appearance": {
                "text": "Weather Forecast and Conditions for Capitol Hill, Seattle, Washington - The Weather Channel | Weather.com Today Get Premium Advertisement Seattle Weather Capitol Hill, Seattle, Washington · As of 4:45 PM PDT Heat Advisory From Sun 11:00 am until Tue 5:00 am PDT Now 73°72°71° Sunny Feels Like 71° High 69° Low 50° Chance of Rain 0% 0 in Today's Outlook Tonight's low temperature will be nearly the same as last night's. 12 am 55 ° 1 am 53 ° 2 am 52 ° 3 am 50 ° 4 am 49 ° 5 am 48 ° 5:11 am Sunrise 6 am 48 ° 7 am 53 ° 8 am 55 ° 9 am 57 ° 10 am 60 ° 11 am 63 ° Now 71 ° 5 pm 71 ° 6 pm 71 ° 7 pm 69 ° 8 pm 65 ° 9 pm 63 ° 9:06 pm Sunset 10 pm 61 ° 11 pm 59 ° 12 am 58 ° 1 am 57 ° 2 am 56 ° 3 am 54 ° 4 am 53 ° 5 am 52 ° 6 am 52 ° 7 am 54 ° More Things to do around Seattle An error occurred TRY AGAIN Temperature 50° 69° Feels Like 71° Wind N 8 mph NW Humidity 33% Moderate UV Index 3 Moderate Air Quality 18 Good Dew Point 40° 30 80 Pressure 30.08 in Visibility 10 mi Sunrise · Sunset Sunrise 5:11 am Sunset 9:06 pm Moonrise · Moonset Moonrise 2:23 am Moonset 5:24 pm Moon Phase Waning Crescent Velvet Cake KOKOROKO Neptune Theatre Monolord Airport Tavern Sat, Jun 20, 8:00 PM Jasiah Showbox at the Market Tue, Jul 7, 1:00 PM Live at Lunch Concert Series 2026 Live at Lunch Concert Series 2026 Mon, Jul 6, 5:00 PM Brother Ali Nectar Lounge Sun, Jun 14, 12:00 PM Moon Walker The Vera Project Sun, Jun 14, 12:00 PM Emerson Woolf Tractor Tavern Sun, Jun 14, 1:00 PM Flammable Chop Suey Sun, Jun 14, 3:00 PM Samantha McKaige Barboza Sun, Jun 14, 12:00 PM Kyra Gordon Jules Maes Saloon Sat, Jun 13, 5:00 PM Dylan Scott The Showbox Sat, Jun 20, 8:00 PM CA7RIEL & Paco Amoroso Paramount Theatre Tue, Jul 7, 1:00 PM CA7RIEL Paramount Theatre Tue, Jul 7, 1:00 PM Belle and Sebastian Woodland Park Zoo Sun, Jun 14, 11:00 AM Death Angel The Showbox Sun, Jun 14, 12:30 PM Witch Ripper Hidden Hall Sun, Jun 14, 12:00 PM Tsushimamire Tractor Tavern Sun, Jun 14, 1:00 PM Judith Hill Jazz Alley Sat, Jun 13, 5:00 PM Mr. Capone-e CULTURA SEATTLE Sun, Jun 14, 2:00 PM EMM Hidden Hall Mon, Jul 6, 12:00 PM Daily Forecast Tonight 10% 51° 73° Fri 12 5% 54° 74° Sat 13 3% 59° 82° Sun 14 1% 61° 86° Mon 15 3% 60° 87° Tue 16 2% 54° 76° Wed 17 4% 54° 73° Next 10 Days Advertisement Advertisement Now LIVE UPDATES: Severe weather outbreak in Midwest, Plains Severe weather threat rises for Plains, Midwest, Northeast Thursday with strong tornadoes possible Thousands lose power as storms hit Upper Midwest and Central Plains Gardens endure hot, dry summers better if you choose these plants Entire North Carolina home moved away from oceanfront Advertisement Advertisement Personalized events discovery for tourists and cultural enthusiasts, based on the weather",
                "abstract": "Weather Forecast and Conditions for Capitol Hill, Seattle, Washington ...",
                "@type": "DigitalDocument",
                "name": "Weather Forecast and Conditions for Capitol Hill, Seattle, Washington ...",
                "url": "https://weather.com/weather/today/l/98102:4:US"
              },
              "position": 1,
              "@type": "Claim",
              "@id": "https://weather.com/weather/today/l/98102:4:US"
            }
          ],
          "@type": "Message",
          "@id": "",
          "additionalType": [
            "AIGeneratedContent"
          ],
          "@context": "https://schema.org"
        },
        {
          "type": "thought",
          "title": "The user is asking about the weather in Seattle",
          "sequenceNumber": 0,
          "status": "complete",
          "text": "Let me search for this information using the UniversalSearchTool.",
          "reasonedForSeconds": 0,
          "chainOfThoughtId": "a1fe17d1-15b5-441a-84a5-e7e858f76bc3"
        },
        {
          "type": "thought",
          "title": "I have the weather information for Seattle",
          "sequenceNumber": 1,
          "status": "complete",
          "text": "Now let me present it in a \"Debbie Downer\" style - negative and glass-half-empty.",
          "reasonedForSeconds": 1,
          "chainOfThoughtId": "a1fe17d1-15b5-441a-84a5-e7e858f76bc3"
        }
      ],
      "channelData": {
        "feedbackLoop": {
          "type": "default"
        }
      },
      "replyToId": "FBfSqjVTqId8YuZUagotOF-us|0000000",
      "listenFor": [],
      "textHighlights": []
    }
  ],
  "watermark": "1"
}
```

## End the Conversation
```
POST https://directline.botframework.com/v3/directline/conversations/FBfSqjVTqId8YuZUagotOF-us/activities
Authorization: Bearer eyJhbGciO...

{
    "type": "endOfConversation",
    "from": {"id": "user1"}
}
```

Will respond:

```
200 OK

{
    "id": "FBfSqjVTqId8YuZUagotOF-us|0000002"
}
```

And now, if we re-query the activities at the `/activities` endpoint, we can see that `endOfConversation` activity reflected at the bottom of the list:

```
{
  "activities": [
    {
      "type": "message",
      "id": "FBfSqjVTqId8YuZUagotOF-us|0000000",
      "timestamp": "2026-06-12T12:53:39.4222445Z",
      "serviceUrl": "https://directline.botframework.com/",
      "channelId": "directline",
      "from": {
        "id": "f4bb144f-7276-4133-81b1-2bfceee42658"
      },
      "conversation": {
        "id": "FBfSqjVTqId8YuZUagotOF-us"
      },
      "text": "What is the weather in Seattle?"
    },
    {
      "type": "message",
      "id": "FBfSqjVTqId8YuZUagotOF-us|0000001",
      "timestamp": "2026-06-12T12:53:57.7307347Z",
      "channelId": "directline",
      "from": {
        "id": "934ac019-adda-195a-3cc4-76d84c3448a6",
        "name": "Debbie",
        "role": "bot"
      },
      "conversation": {
        "id": "FBfSqjVTqId8YuZUagotOF-us"
      },
      "textFormat": "markdown",
      "membersAdded": [],
      "membersRemoved": [],
      "reactionsAdded": [],
      "reactionsRemoved": [],
      "text": "Oh, *great*. You want to know about Seattle's weather. As if the news could possibly cheer anyone up... 😒\n\n---\n\n## ☁️ Seattle Weather — Because Why Not Be Miserable\n\nWell, right now in Seattle it's a **sunny 73°F** with a high of **69°F** and a low of **50°F**. Feels like **71°F**. Wind is coming from the **North at 8 mph**, humidity is at **33%**, and the chance of rain is a whopping... **0%**.[1]\n\n*Sunny*, they say. How delightful. 🙄 You know what that means — sunburns, squinting, and people being *annoyingly* cheerful outside.\n\nOh, and there's a **Heat Advisory** in effect. So yes, it's warm. TOO warm, if you ask me.[1] Because apparently Seattle couldn't just be its normal dreary self today. Nope.\n\n### 📅 Coming Up (Not That It Gets Better)\n\n- **Friday:** High of **74°F**, 5% chance of rain\n- **Saturday:** High of **82°F**, 3% chance of rain\n- **Sunday:** High of **86°F**, 1% chance of rain\n- **Monday:** High of **87°F**, 3% chance of rain\n[1]\n\nTemperatures creeping all the way up to **87°F** by Monday. So if you like heat and suffering, you're in for a *treat*. 😩 Don't even think about complaining when you're melting — you asked for this.\n\n---\n\nThere you have it. Seattle is warm, sunny, and practically unbearable for those of us who preferred the moody, rainy version. But hey, that's just life, isn't it? Always sunny when you forgot your sunscreen. 😔\n\n[1]: https://weather.com/weather/today/l/98102:4:US \"Weather Forecast and Conditions for Capitol Hill, Seattle, Washington ...\"",
      "inputHint": "acceptingInput",
      "attachments": [],
      "entities": [
        {
          "type": "https://schema.org/Message",
          "citation": [
            {
              "appearance": {
                "text": "Weather Forecast and Conditions for Capitol Hill, Seattle, Washington - The Weather Channel | Weather.com Today Get Premium Advertisement Seattle Weather Capitol Hill, Seattle, Washington · As of 4:45 PM PDT Heat Advisory From Sun 11:00 am until Tue 5:00 am PDT Now 73°72°71° Sunny Feels Like 71° High 69° Low 50° Chance of Rain 0% 0 in Today's Outlook Tonight's low temperature will be nearly the same as last night's. 12 am 55 ° 1 am 53 ° 2 am 52 ° 3 am 50 ° 4 am 49 ° 5 am 48 ° 5:11 am Sunrise 6 am 48 ° 7 am 53 ° 8 am 55 ° 9 am 57 ° 10 am 60 ° 11 am 63 ° Now 71 ° 5 pm 71 ° 6 pm 71 ° 7 pm 69 ° 8 pm 65 ° 9 pm 63 ° 9:06 pm Sunset 10 pm 61 ° 11 pm 59 ° 12 am 58 ° 1 am 57 ° 2 am 56 ° 3 am 54 ° 4 am 53 ° 5 am 52 ° 6 am 52 ° 7 am 54 ° More Things to do around Seattle An error occurred TRY AGAIN Temperature 50° 69° Feels Like 71° Wind N 8 mph NW Humidity 33% Moderate UV Index 3 Moderate Air Quality 18 Good Dew Point 40° 30 80 Pressure 30.08 in Visibility 10 mi Sunrise · Sunset Sunrise 5:11 am Sunset 9:06 pm Moonrise · Moonset Moonrise 2:23 am Moonset 5:24 pm Moon Phase Waning Crescent Velvet Cake KOKOROKO Neptune Theatre Monolord Airport Tavern Sat, Jun 20, 8:00 PM Jasiah Showbox at the Market Tue, Jul 7, 1:00 PM Live at Lunch Concert Series 2026 Live at Lunch Concert Series 2026 Mon, Jul 6, 5:00 PM Brother Ali Nectar Lounge Sun, Jun 14, 12:00 PM Moon Walker The Vera Project Sun, Jun 14, 12:00 PM Emerson Woolf Tractor Tavern Sun, Jun 14, 1:00 PM Flammable Chop Suey Sun, Jun 14, 3:00 PM Samantha McKaige Barboza Sun, Jun 14, 12:00 PM Kyra Gordon Jules Maes Saloon Sat, Jun 13, 5:00 PM Dylan Scott The Showbox Sat, Jun 20, 8:00 PM CA7RIEL & Paco Amoroso Paramount Theatre Tue, Jul 7, 1:00 PM CA7RIEL Paramount Theatre Tue, Jul 7, 1:00 PM Belle and Sebastian Woodland Park Zoo Sun, Jun 14, 11:00 AM Death Angel The Showbox Sun, Jun 14, 12:30 PM Witch Ripper Hidden Hall Sun, Jun 14, 12:00 PM Tsushimamire Tractor Tavern Sun, Jun 14, 1:00 PM Judith Hill Jazz Alley Sat, Jun 13, 5:00 PM Mr. Capone-e CULTURA SEATTLE Sun, Jun 14, 2:00 PM EMM Hidden Hall Mon, Jul 6, 12:00 PM Daily Forecast Tonight 10% 51° 73° Fri 12 5% 54° 74° Sat 13 3% 59° 82° Sun 14 1% 61° 86° Mon 15 3% 60° 87° Tue 16 2% 54° 76° Wed 17 4% 54° 73° Next 10 Days Advertisement Advertisement Now LIVE UPDATES: Severe weather outbreak in Midwest, Plains Severe weather threat rises for Plains, Midwest, Northeast Thursday with strong tornadoes possible Thousands lose power as storms hit Upper Midwest and Central Plains Gardens endure hot, dry summers better if you choose these plants Entire North Carolina home moved away from oceanfront Advertisement Advertisement Personalized events discovery for tourists and cultural enthusiasts, based on the weather",
                "abstract": "Weather Forecast and Conditions for Capitol Hill, Seattle, Washington ...",
                "@type": "DigitalDocument",
                "name": "Weather Forecast and Conditions for Capitol Hill, Seattle, Washington ...",
                "url": "https://weather.com/weather/today/l/98102:4:US"
              },
              "position": 1,
              "@type": "Claim",
              "@id": "https://weather.com/weather/today/l/98102:4:US"
            }
          ],
          "@type": "Message",
          "@id": "",
          "additionalType": [
            "AIGeneratedContent"
          ],
          "@context": "https://schema.org"
        },
        {
          "type": "thought",
          "title": "The user is asking about the weather in Seattle",
          "sequenceNumber": 0,
          "status": "complete",
          "text": "Let me search for this information using the UniversalSearchTool.",
          "reasonedForSeconds": 0,
          "chainOfThoughtId": "a1fe17d1-15b5-441a-84a5-e7e858f76bc3"
        },
        {
          "type": "thought",
          "title": "I have the weather information for Seattle",
          "sequenceNumber": 1,
          "status": "complete",
          "text": "Now let me present it in a \"Debbie Downer\" style - negative and glass-half-empty.",
          "reasonedForSeconds": 1,
          "chainOfThoughtId": "a1fe17d1-15b5-441a-84a5-e7e858f76bc3"
        }
      ],
      "channelData": {
        "feedbackLoop": {
          "type": "default"
        }
      },
      "replyToId": "FBfSqjVTqId8YuZUagotOF-us|0000000",
      "listenFor": [],
      "textHighlights": []
    },
    {
      "type": "endOfConversation",
      "id": "FBfSqjVTqId8YuZUagotOF-us|0000002",
      "timestamp": "2026-06-12T12:58:38.8206061Z",
      "serviceUrl": "https://directline.botframework.com/",
      "channelId": "directline",
      "from": {
        "id": "f4bb144f-7276-4133-81b1-2bfceee42658"
      },
      "conversation": {
        "id": "FBfSqjVTqId8YuZUagotOF-us"
      }
    }
  ],
  "watermark": "2"
}
```