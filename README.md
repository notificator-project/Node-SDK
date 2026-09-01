# Notificator Node.js SDK

Send alerts from Node.js, serverless functions, workers, queues, and backend applications to the Notificator mobile app and connected devices.

The SDK connects to the hosted Notificator API. Notificator operates the delivery service and securely manages mobile push, account email, and connected-device delivery.

The current package is available from [npm](https://www.npmjs.com/package/@notificator-project/api), with release notes published on [GitHub](https://github.com/notificator-project/Node-SDK/releases/latest).

## Install

```bash
npm install @notificator-project/api
```

## Send an alert

```js
import { NotificatorClient } from "@notificator-project/api";

const notificator = new NotificatorClient({
  apiKey: process.env.NOTIFICATOR_API_KEY,
});

await notificator.notify({
  title: "Deployment complete",
  body: "Version 2.4.1 is live.",
  source: "deploy-worker",
  category: "info",
  data: { version: "2.4.1", environment: "production" },
});
```

Use a `public_client` API key created in the Notificator mobile app. Keep it on the server and never include it in browser or mobile bundles.

The SDK never contains or requests Notificator infrastructure credentials such as Expo, Supabase, email-provider, or MQTT service secrets.

## Delivery controls

```js
await notificator.notify({
  title: "Queue needs attention",
  body: "The order queue exceeded its threshold.",
  severity: "warning",
  sendPush: true,
  sendEmail: true,
  sendMqtt: true,
  deviceId: "optional-target-device-id",
});
```

Email delivery follows the account preference unless `sendEmail` is explicitly supplied. MQTT can target all active devices or a single owned device.

See the [Public Notify API documentation](https://docs.notificator-project.com/reference/public-notify/) for the complete payload and response contract.

## Requirements

- Node.js 20 or newer
- A server-side `public_client` API key
- Network access to `https://api.notificator-project.com`

## Development

```bash
npm install
npm test
npm run format:check
npm pack --dry-run
```

## Security

Only use the SDK in trusted server-side code. If an API key is exposed, revoke it in the Notificator mobile app and create a replacement.
