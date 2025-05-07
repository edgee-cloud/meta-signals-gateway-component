<div align="center">
<p align="center">
  <a href="https://www.edgee.cloud">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://cdn.edgee.cloud/img/component-dark.svg">
      <img src="https://cdn.edgee.cloud/img/component.svg" height="100" alt="Edgee">
    </picture>
  </a>
</p>
</div>


<h1 align="center">Meta Signals Gateway Component for Edgee</h1>

[![Coverage Status](https://coveralls.io/repos/github/edgee-cloud/meta-signals-gateway-component/badge.svg)](https://coveralls.io/github/edgee-cloud/meta-signals-gateway-component)
[![GitHub issues](https://img.shields.io/github/issues/edgee-cloud/meta-signals-gateway-component.svg)](https://github.com/edgee-cloud/meta-signals-gateway-component/issues)
[![Edgee Component Registry](https://img.shields.io/badge/Edgee_Component_Registry-Public-green.svg)](https://www.edgee.cloud/edgee/meta-signals-gateway)

This component implements the data collection protocol between [Edgee](https://www.edgee.cloud) and [Meta Signals Gateway](https://developers.facebook.com/docs/marketing-api/conversions-api/).

## Quick Start

1. Download the latest component version from our [releases page](../../releases)
2. Place the `meta_signals_gateway.wasm` file in your server (e.g., `/var/edgee/components`)
3. Add the following configuration to your `edgee.toml`:

```toml
[[components.data_collection]]
id = "meta_signals_gateway"
file = "/var/edgee/components/meta_signals_gateway.wasm"
settings.servers_location = "EU"           # Choose servers location (EU or US)
settings.pixel_id = "YOUR_PIXEL_ID"        # Meta Pixel ID
settings.data_collection_method = "edge"   # Data collection method (edge or js)
settings.path_prefix = ""                  # Custom path prefix for Meta Pixel (not used by the component)
settings.inject_sdk = false                # Automatically inject Meta Pixel (not used by the component)
```

## Event Handling

### Event Mapping
The component maps Edgee events to Meta Signals Gateway events as follows:

| Edgee event | Meta Signals Gateway Event  | Description |
|-------------|-----------|-------------|
| Page   | `PageView`     | Triggered when a user views a page |
| Track  | Name of the event | Uses the provided event name directly |
| User   | `Lead` | Used for lead identification |

### User Event Handling
User events in Meta Signals Gateway serve multiple purposes:
- Triggers an `Lead` call to Meta Signals Gateway
- Stores `user_id`, `anonymous_id`, and `properties` on the user's device
- Enriches subsequent Page and Track events with user data
- Enables proper user attribution across sessions

## Configuration Options

### Basic Configuration
```toml
[[components.data_collection]]
id = "meta_signals_gateway"
file = "/var/edgee/components/meta_signals_gateway.wasm"
settings.servers_location = "EU"           # Choose servers location (EU or US)
settings.pixel_id = "YOUR_PIXEL_ID"        # Meta Pixel ID
settings.data_collection_method = "edge"   # Data collection method (edge or js)
settings.path_prefix = ""                  # Custom path prefix for Meta Pixel (not used by the component)
settings.inject_sdk = false                # Automatically inject Meta Pixel (not used by the component)

# Optional configurations
settings.edgee_default_consent = "pending" # Set default consent status
```

### Event Controls
Control which events are forwarded to Meta Signals Gateway:
```toml
settings.edgee_page_event_enabled = true   # Enable/disable page view tracking
settings.edgee_track_event_enabled = true  # Enable/disable custom event tracking
settings.edgee_user_event_enabled = true   # Enable/disable user identification
```

## Development

### Building from Source
Prerequisites:
- [Rust](https://www.rust-lang.org/tools/install)
- WASM target: `rustup target add wasm32-wasip2`
- wit-deps: `cargo install wit-deps`

Build command:
```bash
edgee component build
```

### Contributing
Interested in contributing? Read our [contribution guidelines](./CONTRIBUTING.md)

### Security
Report security vulnerabilities to [security@edgee.cloud](mailto:security@edgee.cloud)
```