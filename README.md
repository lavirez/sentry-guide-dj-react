# Integrate Sentry with 
# FRONT-END With Javascript
## Install

Sentry captures data by using an SDK within your application’s runtime.
`yarn add @sentry/react`

## Configure

Configuration should happen as early as possible in your application's lifecycle.

Sentry supports multiple versions of React Router. See the [React Router Integration](https://docs.sentry.io/platforms/javascript/guides/react/configuration/integrations/react-router/?original_referrer=https%3A%2F%2Fwww.google.com%2F) docs for information on how to configure Sentry with React Router.

The following code sample will let you choose your personal config from the dropdown, once you're [logged in](https://sentry.io/auth/login/?next=https://docs.sentry.io/platforms/javascript/guides/react/)

.


```javascript
import { createRoot } from "react-dom/client";
import { createBrowserRouter } from "react-router-dom";
import React from "react";
import * as Sentry from "@sentry/react";
import App from "./App";

Sentry.init({
  dsn: "
```

```javascript
https://examplePublicKey@o0.ingest.sentry.io/0",
  integrations: [
    new Sentry.BrowserTracing({
      // See docs for support of different versions of variation of react router
      // https://docs.sentry.io/platforms/javascript/guides/react/configuration/integrations/react-router/
      routingInstrumentation: Sentry.reactRouterV6Instrumentation(
        React.useEffect,
        useLocation,
        useNavigationType,
        createRoutesFromChildren,
        matchRoutes
      ),
    }),
    new Sentry.Replay()
  ],

  // Set tracesSampleRate to 1.0 to capture 100%
  // of transactions for performance monitoring.
  tracesSampleRate: 1.0,

  // Set `tracePropagationTargets` to control for which URLs distributed tracing should be enabled
  tracePropagationTargets: ["localhost", /^https:\/\/yourserver\.io\/api/],

  // Capture Replay for 10% of all sessions,
  // plus for 100% of sessions with an error
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
});

const container = document.getElementById(“app”);
const root = createRoot(container);
root.render(<App />);
```


# BACK-END with Django

## Install

Install `sentry-sdk` from PyPI with the `django` extra:


```bash
pip install --upgrade 'sentry-sdk[django]'
```## Configure

To configure the Sentry SDK, initialize it in your `settings.py` file:

The following code sample will let you choose your personal config from the dropdown, once you're [logged in](https://sentry.io/auth/login/?next=https://docs.sentry.io/platforms/python/guides/django/)

.

`settings.py`

Copied

```python
import sentry_sdk

sentry_sdk.init(
    dsn="
```

```python
https://examplePublicKey@o0.ingest.sentry.io/0",

    # Set traces_sample_rate to 1.0 to capture 100%
    # of transactions for performance monitoring.
    # We recommend adjusting this value in production,
    traces_sample_rate=1.0,
)
```

## Verify

The snippet below includes an intentional error that will be captured by Sentry when triggered. This will allow you to make sure that everything is working as soon as you set it up:

```python
from django.urls import path

def trigger_error(request):
    division_by_zero = 1 / 0

urlpatterns = [
    path('sentry-debug/', trigger_error),
    # ...
]
```


