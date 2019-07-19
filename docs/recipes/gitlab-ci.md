# Using semantic-release with [GitLab CI](https://about.gitlab.com/features/gitlab-ci-cd)

## Environment variables

The [Authentication](../usage/ci-configuration.md#authentication) environment variables can be configured with [Secret variables](https://docs.gitlab.com/ce/ci/variables/README.html#secret-variables).

## Node project configuration

GitLab CI supports [Pipelines](https://docs.gitlab.com/ee/ci/pipelines.html) allowing to test on multiple Node versions and publishing a release only when all test pass.

**Note**: The publish pipeline must run a Node >= 10 version.

### `.gitlab-ci.yml` configuration for Node projects

This example is a minimal configuration for **semantic-release** with a build running Node 10 and 12. See [GitLab CI - Configuration of your jobs with .gitlab-ci.yml](https://docs.gitlab.com/ee/ci/yaml/README.html) for additional configuration options.

**Note**: The`semantic-release` execution command varies depending if you are using a [local](../usage/installation.md#local-installation) or [global](../usage/installation.md#global-installation) **semantic-release** installation.

```yaml
# The release pipeline will run only if all jobs in the test pipeline are successful
stages:
    - test
    - release

before_script:
  - npm install

node:10:
  image: node:10
  stage: test
  script:
    - npm test

node:12:
  image: node:12
  stage: test
  script:
    - npm test

publish:
  image: node:10
  stage: release
  script:
    - npx semantic-release
```

### `package.json` configuration

A `package.json` is required only for [local](../usage/installation.md#local-installation) **semantic-release** installation.

```json
{
  "devDependencies": {
    "semantic-release": "^15.0.0"
  }
}
```
