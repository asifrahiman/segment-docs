---
title: Matomo Destination
rewrite: true
redirect_from: '/connections/destinations/catalog/piwik/'
---

[Matomo](https://matomo.org/), formerly Piwik, is the leading open source web analytics platform that gives you valuable insights into your website's visitors, your marketing campaigns and much more, so you can optimize your strategy and online experience of your visitors.

This document was last updated on July 18th, 2018. If you notice any gaps, outdated information or simply want to leave some feedback to help us improve our documentation, please [let us know](https://segment.com/help/contact)!

## Getting Started

{% include content/connection-modes.md %}

1. From your Segment UI's Destinations page click on "Add Destination".
2. Search for "Matomo" within the Destinations Catalog and confirm the Source you'd like to connect to. Please note the source must be sending events via our Javascript library Analytics.js.
3. Drop in your Site Id. You can find your Site ID in your Matomo snippet.
4. Drop in your Server URL. You can find your Server URL in your snippet, we will append /matomo.php to the URL automatically.
  5. When you enable Matomo in your Segment settings, Segment's CDN is updated within 45 minutes. Once that happens, Segment asynchronously loads `matomo.js` on your page whenever it is loaded. This means you should remove Matomo's snippet from your page.

## Page
If you haven't had a chance to review our spec, please take a look to understand what the [Page method](https://segment.com/docs/connections/spec/page/) does. An example call would look like:

```js
analytics.page();
```

Our Page method triggers a call to Matomo's `trackPageView` method.

## Identify
If you haven't had a chance to review our spec, please take a look to understand what the [Identify method](https://segment.com/docs/connections/spec/identify/) does. An example call would look like:

```js
analytics.identify('97980cfea0068');
```
Our Identify method triggers a call to Matomo's `setUserId` method and will send the `userId` to Matomo.

## Track
If you haven't had a chance to review our spec, please take a look to understand what the [Track method](https://segment.com/docs/connections/spec/track/) does. An example call would look like:

```js
analytics.track('Logged In');
```

We'll record a Matomo event whenever you make a `track` call.

For the example above, these event attributes are sent to Matomo:

<table>
  <tr>
    <td>**Event Category**</td>
    <td>All</td>
  </tr>
  <tr>
    <td>**Event Action**</td>
    <td>Logged In</td>
  </tr>
</table>

Find below another Track example, this time with all the available Matomo event parameters:

```js
analytics.track('Created Account', {
  category: 'Account',
  label: 'Premium',
  value: 30
})
```

That call will create a Matomo Event with these attributes:

<table>
  <tr>
    <td>**Event Category**</td>
    <td>Account</td>
  </tr>
  <tr>
    <td>**Event Action**</td>
    <td>Logged In</td>
  </tr>
  <tr>
    <td>**Event Name**</td>
    <td>Premium</td>
  </tr>
  <tr>
    <td>**Event Value**</td>
    <td>30</td>
  </tr>
</table>

For **Event Value** you can name the event property `value` or `revenue`. We'll look for `value` first, then fall back to `revenue`.

## Best Pratices

Matomo allows you to set [custom variables](http://matomo.org/docs/custom-variables/) with your pageviews and events. With Segment, you can set page-scoped custom variables with any `track` call you make with analytics.js.

Since these custom variables must be mapped to an index you define, which can change from call to call, the only way we can support these custom variables with full flexibility is to allow you to pass your map in the `context.Matomo.customVars` dictionary of each call.

To take advantage of this feature, your `track` calls should look like this:

> **Note** The destination's name is still "piwik" in the JSON for these calls.

```js
analytics.track('event', {
  property: 'property'
}, { integrations: {
  Piwik: {
    customVars: {
      1: ["<variableName>", "<variableValue>"],
      2: ['SubscriptionId', '1234'],
      3: ['PlanName', 'ENTERPRISE']
      }
    }
  }
})
```

### Goals
If you want to flag specific events as Matomo Goals you can do so by mapping those events in your Segment Source Destinations page under Matomo Settings.

Fill in the event on the left and the Goal ID from Matomo on the right. Then every time the event happens we'll fire Matomo's `trackGoal` method.