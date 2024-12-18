# Features Component

A well-known component that integrates with [Unleash](https://www.getunleash.io/) to provide feature flag management. It enables runtime feature toggling and configuration through a simple interface.

## Quick Start

1. Install the package:
```bash
npm install @well-known-components/features-component
```

2. Set up environment variables:
```env
FF_URL=https://your-unleash-instance-url
SERVER_URL=https://your-api-url
```

3. Create and use the component:
```typescript
import { createFeaturesComponent } from '@well-known-components/features-component'

// Initialize the component
const features = await createFeaturesComponent(
  {
    config,    // WKC configuration component
    logs,      // WKC logger component
    fetch,     // WKC fetch component
  },
  await config.requireString('SERVER_URL')
)

// Check if a feature is enabled
const isEnabled = await features.getIsFeatureEnabled(
  'APPLICATION_NAME',
  'FEATURE_NAME'
)
```

## Configuration

### Required Environment Variables

| Variable | Description |
|----------|-------------|
| `FF_URL` | URL of your Unleash instance |
| `SERVER_URL` | Your API URL (used as referrer for Unleash's applicationHostname strategy) |

### Component Dependencies

The features component requires three well-known components:
- `config`: For accessing environment variables
- `logs`: For logging and debugging
- `fetch`: For making HTTP requests to Unleash

## Usage Examples

### Basic Feature Check

```typescript
const isEnabled = await features.getIsFeatureEnabled(
  'MY_APP',
  'DARK_MODE'
)

if (isEnabled) {
  // Enable dark mode
}
```

### Integration with Other Components

```typescript
export const validateFeature = async (components: AppComponents, data: any) => {
  const { features, logs } = components

  const isFeatureEnabled = await features.getIsFeatureEnabled(
    'MY_APP',
    'NEW_VALIDATION'
  )

  if (isFeatureEnabled) {
    logs.log('Using new validation')
    return newValidation(data)
  }

  return legacyValidation(data)
}
```

## API Reference

### createFeaturesComponent(options, serverUrl)

Creates a new instance of the features component.

**Parameters:**
- `options`: Configuration object containing:
  - `config`: Configuration component instance
  - `logs`: Logger component instance
  - `fetch`: Fetch implementation for making HTTP requests
- `serverUrl`: URL used as referrer for Unleash's applicationHostname strategy

**Returns:** Promise<FeaturesComponent>

### FeaturesComponent Methods

#### getIsFeatureEnabled(applicationName, featureName)

Checks if a feature is enabled in Unleash.

**Parameters:**
- `applicationName`: Your application name
- `featureName`: Feature flag name

**Returns:** Promise<boolean>

**Note:** The actual feature flag in Unleash will be named: `FF_${applicationName}_${featureName}`

## Feature Flag Naming Convention

Feature flags in Unleash follow this naming pattern:
```
FF_<APPLICATION_NAME>_<FEATURE_NAME>
```

For example:
- `FF_MY_APP_DARK_MODE`
- `FF_MY_APP_NEW_VALIDATION`

## Contributing

Please read our contributing guidelines and code of conduct before submitting pull requests or issues.

## License

This project is licensed under the Apache 2.0 License - see the LICENSE file for details.
