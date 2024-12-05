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
   spectral lint test-cases/failing/api.yaml
   ```

## Expected Behavior

The rule should be disabled for the `/resources/details` endpoint due to the override in `.spectral.yaml`.

## Actual Behavior

The rule is still applied despite the override configuration.

## Output

```shell
bash-3.2$ spectral lint test-cases/failing/api.yaml
(node:22719) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)

/Users/david.lance/Workspace/spectral/spectral-rule-no-store/test-cases/failing/api.yaml
  1:1  warning  oas3-api-servers       OpenAPI "servers" must be present and non-empty array.
  2:6  warning  info-contact           Info object must have "contact" object.                        info
  2:6  warning  info-description       Info "description" must be present and non-empty string.       info
 7:10  warning  operation-description  Operation "description" must be present and non-empty string.  paths./resources/details.post
 7:10  warning  operation-operationId  Operation must have "operationId".                             paths./resources/details.post
 7:10  warning  operation-tags         Operation must have non-empty "tags" array.                    paths./resources/details.post

/Users/david.lance/Workspace/spectral/spectral-rule-no-store/test-cases/shared/common.yaml
 5:20  error  cache-control-header-rule  Success responses should contain a `Cache-Control` header value of `no-store`.  components.headers.CacheControlHeader.description
 ```

## Additional Notes

File-level overrides (without path specification) work as expected:
```yaml
overrides:
  - files:
    - "**/test-cases/failing/api.yaml"  # This works
```

## Directory Structure

```
.
├── .spectral.yaml
├── README.md
└── test-cases
    ├── failing
    │   └── api.yaml
    └── shared
        └── common.yaml
```

## Contribution

Feel free to open issues or pull requests if you have suggestions or improvements.
