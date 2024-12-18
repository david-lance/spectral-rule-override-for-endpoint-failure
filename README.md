# Spectral Override Test Case

This repository demonstrates a potential issue with Spectral's override functionality when targeting specific paths within OpenAPI files.

## Issue Description

When attempting to override Spectral rules for specific paths within an OpenAPI file using JSON Pointer notation, the override does not take effect.

## Test Case

The test case includes:
- A simple rule checking for Cache-Control headers
- An attempt to override this rule for a specific endpoint using path notation
- A minimal OpenAPI specification demonstrating the issue

## Setup

1. Install Spectral CLI:
   ```shell
   npm install -g @stoplight/spectral-cli
   ```

2. Run the test:
   ```shell
   spectral lint reference/api.yaml
   ```

## Expected Behavior

The rule should be disabled for the `/resources/details` endpoint due to the override in `.spectral.yaml`.

## Actual Behavior

The rule is still applied despite the override configuration.

## Expected Output

```shell
(base) david.lance@David-Lance spectral-rule-override-for-endpoint-failure % spectral lint test-cases/failing/api.yaml --verbose
Found 57 rules (46 enabled)
Linting /Users/david.lance/Workspace/spectral-rule-override-for-endpoint-failure/reference/api.yaml
No results with a severity of 'error' found!
```

## Actual Output

```shell
bash-3.2$ spectral lint reference/api.yaml
/Users/david.lance/Workspace/spectral/spectral-rule-no-store/test-cases/shared/common.yaml
 5:20  error  cache-control-header-rule  Success responses should contain a `Cache-Control` header value of `no-store`.  components.headers.CacheControlHeader.description
 ```

## Additional Notes

File-level overrides (without path specification) work as expected. Also found that the `**` prefix was necessary to target the file.:
```yaml
overrides:
  - files:
    - "**/reference/api.yaml"  # This works
```

## Directory Structure

```
.
├── .spectral.yaml
├── README.md
└── reference
    ├── api.yaml
    └── shared
        └── common.yaml
```

## Contribution

Feel free to open issues or pull requests if you have suggestions or improvements.
