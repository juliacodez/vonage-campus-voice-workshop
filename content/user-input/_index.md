---
title: "Interact with your user"
weight : 35
---


Making a call, or receiving one, is great but what next? In this final exercise we'll interact with our user. To keep things simple, we'll prompt them to input digits on their keypad, but the same approaches would work with spoken interactions on both sides.

The outline of the process looks something like this:

1. Create an NCCO to prompt the user to input some digits by using a [`talk`](https://developer.vonage.com/voice/voice-api/ncco-reference#talk) action and then an [`input`](https://developer.vonage.com/voice/voice-api/ncco-reference#input) action. The `input` action accepts a URL so use your ngrok URL with a suffix - our examples use `/webhooks/dtmf`.
2. When the user sends some input, we'll receive a webhook containing some data to identify the call (see the [detail of the webhook](https://developer.vonage.com/voice/voice-api/webhook-reference#input) on the Developer Portal)
3. Using the data that arrived in the webhook, we can return a new NCCO with custom content.

Start a new application, we have some new code for you!

Prepare your dependencies: `npm install @vonage/server-sdk express`

```js
import express from 'express'
const PORT = 3000
//destructuring
const {
    json
} = express;
//instantiate the Express app
const app = express()

app.use(json())

const onInboundCall = (request, response) => {
    const ncco = [{
            action: 'talk',
            text: 'Please enter a digit'
        },
        {
            action: 'input',
            eventUrl: [`${request.protocol}://${request.get('host')}/webhooks/dtmf`]
        }
    ]

    response.json(ncco)
}

const onInput = (request, response) => {
    const dtmf = request.body.dtmf

    const ncco = [{
        action: 'talk',
        text: `You pressed ${dtmf}`
    }]

    response.json(ncco)
}

app
    .get('/webhooks/answer', onInboundCall)
    .post('/webhooks/dtmf', onInput)

app.listen(3000, () => {
    console.log('Server listening at http://localhost:3000')
})
```

Put this code into `user-input.js` and start the server with `node user-input.js`.

**Run your code** - if your ngrok server is still running from the previous example then you should be able to simply call your Vonage number from your cellphone again. Don't forget to bring up the dial pad ready to enter a digit!

### Next Steps: Expand to make a simple IVR

How about expanding your example to make a small IVR (Interactive Voice Response) system? "Press one for sales, two for support ..." you get the idea. Or go sillier and base something on my colleague Tony's [Dial-a-Carol system](https://learn.vonage.com/blog/2018/12/03/dial-a-christmas-carol-with-nexmo-and-python-dr/)!

### Next Steps: Record the Call

Vonage enables call recording, which is useful in many different applications. Take a look at the [NCCO documentation for the `record` action](https://developer.vonage.com/voice/voice-api/ncco-reference#record) and record the call.

You need to be authenticated to be able to download the call recording - check the first example we did for how to create the client with the application and key you need. There are also code examples [on the developer portal](https://developer.nexmo.com/voice/voice-api/code-snippets/download-a-recording) to help you along.

