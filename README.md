# Deploy Cloud Functions with GitHub Actions using Service Account Credentials 🚀

This repository provides a GitHub Actions workflow that allows you to deploy your Firebase Cloud Functions using your service account credentials.

## Usage

### Deploy on each production branch push

To deploy on each push to the production branch, add the following code to your workflow file:

```yaml
name: Deploy Cloud Functions

on:
  push:
    branches:
      - production
      - staging
    # Optionally configure to run only for specific files. For example:
    # paths:
    # - "website/**"
env:
  PROJECT_ID: your-Firebase-project-ID # The ID of your Firebase/GCP project

jobs:
  deploy_live_website:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      # Add any build steps here. For example:
      # - run: npm ci && npm run build
      - uses: aronep6/action-functions-deploy@1.0.0
        with:
          serviceAccountKey: "${{ secrets.DEPLOY_SERVICE_ACCOUNT_KEY }}"
          projectId: "${{ env.PROJECT_ID }}"
```

## Options

You can configure the action-functions-deploy GitHub Action using the following options:

|       Parameter        |   Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| :--------------------: | :------: | :------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  `serviceAccountKey`   | `string` |   Yes    | This is a service account JSON key. The easiest way to set it up is to run `firebase init hosting:github`. However, it can also be [created manually](./docs/service-account.md). It's important to store this token as an [encrypted secret](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets) to prevent unintended access to your Firebase project. Set it in the "Secrets" area of your repository settings and reference it in the workflow file. Example: `serviceAccountKey: "${{ secrets.DEPLOY_SERVICE_ACCOUNT_KEY }}"` |
|      `projectId`       | `string` |    No    | The Firebase project that contains the Hosting site to which you want to deploy. If left blank, you need to check in a `.firebaserc` file so that the Firebase CLI knows which Firebase project to use.                                                                                                                                                                                                                                                                                                                                                                                      |
| `firebaseToolsVersion` | `string` |    No    | The version of `firebase-tools` to use. If not specified, defaults to `latest`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

## Forked from FirebaseExtended/action-hosting-deploy

This GitHub Action was forked from [FirebaseExtended/action-hosting-deploy](https://github.com/FirebaseExtended/action-hosting-deploy). I have made some modifications to the original code to better suit our needs for deploying cloud functions.

The logic of this action is still the same as the original, but instead of deploying hosting content, it now deploys cloud functions. If you are interested in deploying hosting content, we recommend checking out the original GitHub Action.

## Status

This is not an officially supported Google product. Issues and PRs are welcome. 😊
