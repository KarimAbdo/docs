GitLab can automatically deploy your Android and iOS Apps to [TestFairy](https://www.testfairy.com/).

* On the TestFairy dashboard, navigate to the Preferences page.
  ![](https://docs.testfairy.com/img/continuous-integration/testfairy-open-preferences.png)

* On the Preferences page, go to the API Key section.
  ![](https://docs.testfairy.com/img/continuous-integration/testfairy_upload_key.png)

* Copy your API key and go to your application's project settings in GitLab.
  ![](https://docs.testfairy.com/img/continuous-integration/gitlab_secret_keys.png)

* To deploy, add a job to your `.gitlab-ci.yml` configuration using [fastlane](https://docs.fastlane.tools/getting-started/ios/beta-deployment/) or `curl` (example below).
``` yaml
stages:
  - deploy

deploy:
  stage: deploy
  only:
  - master
  script:
  - |
    curl \
      -A "GitLab CI" \
      -F api_key="${TESTFAIRY_API_KEY}" \
      -F comment="GitLab Pipeline build ${CI_COMMIT_SHA}" \
      -F file=@android.apk \
      https://upload.testfairy.com/api/upload/
```

**Note** Be sure to replace the `-F file=@android.apk` argument with a path to your own APK or IPA.

For a complete list of available options, please visit the [TestFairy Upload API documentation](https://docs.testfairy.com/API/Upload_API.html)
