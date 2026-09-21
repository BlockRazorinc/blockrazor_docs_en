---
description: Use this CLI example to subscribe to Solana Geyser Stream of BlockRazor
metaLinks:
  canonical: cli.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/solana/geyser-stream/cli
---

# Solana Geyser Stream CLI Example

### Quick Start

{% stepper %}
{% step %}
#### Create Directory

```
mkdir geyser-stream
cd geyser-stream
```
{% endstep %}

{% step %}
#### Download Client

```
# Mac
curl -L -o client https://github.com/BlockRazorinc/geyserstream-client-go/releases/download/v1.0.0/client-darwin-arm64
```

```
# Ubuntu-22.04
curl -L -o client https://github.com/BlockRazorinc/geyserstream-client-go/releases/download/v1.0.0/client-ubuntu-22.04
```

```
# Ubuntu-24.04
curl -L -o client https://github.com/BlockRazorinc/geyserstream-client-go/releases/download/v1.0.0/client-ubuntu-24.04
```
{% endstep %}

{% step %}
#### Grant permission

```
chmod +x client
```
{% endstep %}

{% step %}
#### Run client

```
# Subscribe transaction
./client -e "https://geyserstream-tokyo.blockrazor.xyz" --x-token "$AUTH_TOKEN" subscribe --transactions --transactions-vote false --transactions-failed false
```

```
# Subscribe account
./client -e "https://geyserstream-tokyo.blockrazor.xyz" --x-token "$AUTH_TOKEN" subscribe --accounts --accounts-owner 11111111111111111111111111111111
```

```
# Subscribe block
./client -e "https://geyserstream-tokyo.blockrazor.xyz" --x-token "$AUTH_TOKEN" subsc
```
{% endstep %}
{% endstepper %}
