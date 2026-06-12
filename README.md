# setup-k6-action

This action sets up a Grafana k6 environment for use in a GitHub Actions workflow by:

- Installing a specific version of k6.
- Installing Chrome for Browser Testing (optional).

> ⚠️ This action only supports Linux runners ⚠️

## Usage

See [action.yml](action.yaml).

### Basic

```yaml
on:
  push:

jobs:
  protocol:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: grafana/setup-k6-action@v1
        with:
          k6-version: '0.49.0'
      - run: k6 run script.js --quiet
```

### Browser Testing

```yaml
on:
  push:

jobs:
  protocol:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: grafana/setup-k6-action@v1
        with:
          k6-version: '0.49.0'
          browser: true
      - run: k6 run script.js --quiet
```

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
