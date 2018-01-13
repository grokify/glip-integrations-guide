# Inbound Webhook Integrations

Webhooks are an ability for services to send event-based messages. Glip has the ability to both receive inbound webhooks and send outbound webhooks. This section covers inbound webhooks which are converted to posts in Glip teams, groups and direct coversations. Outbound webhooks are covered in another section with the Subscription API.

Glip has the ability to receive inbound webhooks with a specific format documented here.

There are two types webhooks services can send:

* Simple Webhooks
* Custom Webhooks

## Simple Webhooks

These are explicitly formatted messages that outbound services send, typically in JSON format but sometimes use less popular formats like XML. Some services 

These webhooks must be converted to Glip's format before sending them to Glip. You can do the conversion using a program yourself, using a service like [Zapier](https://zapier.com) or an open source program like [Chathooks](https://github.com/grokify/chathooks). See more on Chathoosk below.

## Custom Webhooks

Some services have the ability to customize webhook messages on their configuration page. These services can be used to format a message for Glip directly in their UI. Some services that have this capability include Datadog, Desk.com, Marketo and New Relic.

To connect these services with Glip, use the Glip documentation for webhooks to create a JSON body to connect to Glip.

## Chathooks

Chathooks is a chat-focused webhook forwarder that can modify a message to Glip's format. Using this approach, you can take many webhook-based services, point them to a Chathooks server, noramlize the message in the server and then forward the message to Glip. Chathooks is open source and available here:

[https://github.com/grokify/chathooks](https://github.com/grokify/chathooks)