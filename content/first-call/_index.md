---
title: "Make Your First Call"
weight : 15
---

**Write some code that calls your cellphone**

You will need:

* A Vonage account if you don't have one already. Sign up here: <https://dashboard.nexmo.com/sign-up>
* A Vonage phone number to make calls with. You can [check your existing numbers](https://dashboard.nexmo.com/your-numbers) and [buy numbers](https://dashboard.nexmo.com/buy-numbers) on the dashboard.
* An application - you need both the application UUID and the private key file copied somewhere safe. From the [dashboard](https://dashboard.nexmo.com), visit "Your applications" and then "Create a new application". Give your new application a name and choose "Generate public and private key"; your browser will download the private key. Set your application to have Voice capabilities (you can use `example.com` URLs for now, we will update these later) and save it. Once you have created the application, link the Vonage number you will use.
* A number you can phone (probably your cellphone).
* Some sort of working tech stack. Our examples are NodeJS but you should feel free to use whatever technology you know how to make API calls with!

Optional, but recommended:

* The [CLI tool](https://developer.nexmo.com/tools) may be a nicer way to work with the number purchase, application creation, etc.
* The [Server SDK](https://developer.nexmo.com/tools) for your tech stack - we have PHP, Python, Ruby, NodeJS, Java, .NET and a semi-official [Go SDK](https://github.com/nexmo-community/nexmo-go))

Here's the code to get you started, replace the placeholder values in your chosen code:

 * `VONAGE_APPLICATION_PRIVATE_KEY_PATH`: The path to the private key file you saved when creating the application
 * `VONAGE_APPLICATION_ID`: The UUID of your application
 * `VONAGE_NUMBER`: Your Vonage number that the call will be made from. For example `447700900000`.
 * `TO_NUMBER`: The number you would like to call to in [E.164](https://en.wikipedia.org/wiki/E.164) format. For example `447700900001` (note that this _must_ include the dialling code, so if it's a US number, it should start with `1`).

Prepare your dependencies: `npm install @vonage/server-sdk`

Add `"type": "module"` in package.json to enable imports.

```json
{
  "dependencies": {
    "@vonage/server-sdk": "^2.10.8"
  },
  "type": "module"
}
```

```js
import Vonage from "@vonage/server-sdk"

const vonage = new Vonage({
    apiKey: VONAGE_API_KEY,
    apiSecret: VONAGE_API_SECRET,
    applicationId: VONAGE_APPLICATION_ID,
    privateKey: VONAGE_APPLICATION_PRIVATE_KEY_PATH
})

vonage.calls.create({
    to: [{
      type: 'phone',
      number: TO_NUMBER
    }],
    from: {
      type: 'phone',
      number: VONAGE_NUMBER
    },
    ncco: [{
      "action": "talk",
      "text": "This is a text to speech call from Vonage"
    }]
  }, (error, response) => {
    if (error) console.error(error)
    if (response) console.log(response)
  })
```

**Run your code** and answer your phone!

### Next Steps: Customise your call

What would you like to hear? Check out our [NCCO reference documentation](https://developer.vonage.com/voice/voice-api/ncco-reference) to learn what else you can do, if you'd like your spoken greeting to have a bit more expression you can investigate [SSML (Speech Synthesis Markup Language)](https://developer.vonage.com/voice/voice-api/guides/customizing-tts).

### Next Steps: Track events (insight and debugging tactic)

Go back to the dashboard and configure the application's `event_url` endpoint. You can either point this to:

* your application (probably using [ngrok](https://ngrok.com)), we'll be doing another incoming webhook in the next exercise anyway
* a tool such as the [Voice Event Logger](https://github.com/Nexmo/voice-event-logger)
* a general webhook receiver like Requestbin (still available at <http://bin.on.dockerize.io/> and <http://requestbin.net>) or [Postbin](https://postb.in/)

