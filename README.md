# publish-gae-action

This action deploying your application to Google App Engine.

## Example

[BoardCAM.org](https://github.com/BoardCAM/BoardCAM.org/blob/master/.github/workflows/publishgae.yml)

## Usage
```yaml
steps:
- name: Publish to GAE
  uses: zxyle/publish-gae-action@master
```

# License
The scripts and documentation in this project are released under the [MIT License](LICENSE)
## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
