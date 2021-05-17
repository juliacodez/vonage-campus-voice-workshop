---
title: "Call Your Application"
weight : 25
---

**Make a phone call, have your application answer it**

To receive incoming calls, you need a publicly-available URL that will respond with a NCCO telling Vonage what to do with the call. One thing that's different from the previous example is that it does not need an API keys - the Vonage server will call your application this time rather than the other way around!

1. Start by adding a simple route to your application, the code samples here are to help you get started, or you can of course improvise.

    Prepare your dependencies: `npm install @vonage/server-sdk express`

    ```js
    import express from 'express'

    const app = express()

    app.get('/webhooks/answer', (request, response) => {
    const from = request.query.from
    const fromSplitIntoCharacters = from.split('').join(' ')

    const ncco = [{
        action: 'talk',
        text: `Thank you for calling from ${fromSplitIntoCharacters}`
    }]

    response.json(ncco)
    })

    app.listen(3000, () => {
        console.log('Server listening at http://localhost:3000')
    })
    ```

    Save the code sample to `answer-call.js` and then run `node answer-call.js`.

    Again, there are [code examples in other languages](https://developer.vonage.com/voice/voice-api/code-snippets/receive-an-inbound-call) if you prefer.

2. **Test your endpoint** by requesting <http://localhost:3000/webhook/answer> in your browser. You should see some JSON returned.

3. Next, your local URL needs to be publicly available. One way to do this is to use [ngrok](https://ngrok.com):

    `ngrok http 3000`

    When ngrok starts the tunnel, it will show you your URL, something like `https://abc123.ngrok.io` - copy the URL from your ngrok console as we will need it shortly.

4. Back in the dashboard, you can edit the Answer URL of your application, by pasting the ngrok URL and adding `/webhooks/answer` to the end of it, to make something like `https://abc123.ngrok.com/webhooks/answer`.

5. **Call your application** by making a call from your cellphone to the Vonage number linked to your application. You should hear the spoken greeting giving the number you are calling from.

### Next Steps: A more interesting greeting

We've shared our standard "answer a call" example but it isn't particularly interesting! What do you wish your application would do? How about:

* A talking clock
* A randomly chosen uplifting message
* Some music

Use the [NCCO reference documentation](https://developer.vonage.com/voice/voice-api/ncco-reference) to try some alternative content for your Answer URL.

For more ideas, there's a set of [NCCO Examples on GitHub](https://github.com/nexmo-community/ncco-examples) that might help you along the way.

