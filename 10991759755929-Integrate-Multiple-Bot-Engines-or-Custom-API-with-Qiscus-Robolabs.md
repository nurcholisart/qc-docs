# Integrate Multiple Bot Engines or Custom API with Qiscus Robolabs

By default, Robolabs use the Google Dialogflow engine. The solutions in this article are suitable for you who are using Robolabs but still need a custom flow with your own bot/API. So we need to integrate them.

Before we jump to Robolabs, the Qiscus Omnichannel also provides webhook services that you can utilize, see the [documentation](https://documentation.qiscus.com/multichannel-customer-service/webhook).

In Robolabs, there are these 3 options:

1. [Add Project from Custom Bot Engine](https://documentation.qiscus.com/robolabs/custom-bot-engine)
2. [Dialogflow Webhook](https://documentation.qiscus.com/robolabs/external-system#dialogflow-webhook)
3. [Custom API](https://documentation.qiscus.com/robolabs/external-system#custom-api)

## 1. Add Project from Custom Bot Engine

Similar to the feature on Integration menu > Bot Integration, the difference is you can assign to specific channels while the default Bot Integration is for all channels. You only need to set up the bot URL.

Note: *other features from Robolabs (Bot template, handover agent, etc.) will be disabled. Qiscus only send the webhook.*

## 2. Dialogflow Webhook

With this webhook, Qiscus will forward the data from Dialogflow to your URL. You only need to set up the URL and choose the intent.

## 3. Custom API

More customizable than others, in this feature, you are able to define the Headers, Query, Body, and Response for each intent.

**Headers, Query,** and **Body** are the data that Qiscus send to the URL. All the values can be anything or the default data from Qiscus.  
You can use the JSON path **`{{$.path.to.the.value}}`** to get data from the **Qiscus** **default payload**. See the [payload structure](https://documentation.qiscus.com/robolabs/external-system#custom-api-configuration).
