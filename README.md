# loopback-connector-disco-rest


![Disco Agent](./stu_loopback.png "Disco Agent")

## Overview

Based on [loopback-connector-rest](https://github.com/strongloop/loopback-connector-rest) and uses [disco-agent](https://github.com/moonthug/discovery-agent)

Go from 'hardcoded' to 'service-discovery loaded' in a few lines of config!

The configuration of `loopback-connector-disco-rest` is identical to that of the standard rest connector
with a few extra parameters.

An example config for a datasource that uses service discovery to find api details and make a call.

```javascript 1.7
disco: {
  name: 'disco',
  debug: true,
  baseURL: 'http://127.0.0.1:3000',
  connector: 'loopback-connector-disco-rest',
  serviceDiscovery: {
    enabled: true,
    adapter: 'eureka',
    providers: [
      {
        host: process.env.SERVICE_DISCOVERY_HOST,
        port: process.env.SERVICE_DISCOVERY_PORT,
      },
    ],
    registration: {
      type: 'cs-twitter',
      advertiseHost: process.env.SERVICE_DISCOVERY_ADVERTISE_HOST || os.hostname(),
      advertisePort: process.env.SERVICE_DISCOVERY_ADVERTISE_PORT || process.env.PORT || 3000,
    },
    pool: {
      type: 'es-twitter',
    },
  },
  operations: [
    {
      template: {
        method: 'POST',
        url: 'http://notused.com/',
        headers: {
          accept: 'application/json',
          'content-type': 'multipart/form-data',
        },
        form: {
          screenName: '{screenName}',
        },
      },
      functions: {
        getUserByScreenName: [
          'screenName',
        ],
      },
    },
  ],
},
```

An example config for a datasource that registers with a service discovery provider

```javascript 1.7

disco: {
  name: 'disco',
  debug: true,
  baseURL: 'http://127.0.0.1:3000',
  connector: 'loopback-connector-disco-rest',
  serviceDiscovery: {
    enabled: true,
    adapter: 'eureka',
    providers: [
      {
        host: process.env.SERVICE_DISCOVERY_HOST,
        port: process.env.SERVICE_DISCOVERY_PORT,
      },
    ],
    registration: {
      type: 'es-twitter',
      advertiseHost: process.env.SERVICE_DISCOVERY_ADVERTISE_HOST || os.hostname(),
      advertisePort: process.env.SERVICE_DISCOVERY_ADVERTISE_PORT || process.env.PORT || 3000,
      checks: [
        {
          name: 'alive',
          route: '/',
        },
      ],
    },
  },
},
```

