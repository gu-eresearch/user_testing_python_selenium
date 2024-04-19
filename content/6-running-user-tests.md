---
title: Running user tests
nav: Running user tests
---

Ideally user tests are implemented via CICD (continuous integration, continuous deployment), check the following out for more information about [CICD](https://www.redhat.com/en/topics/devops/what-is-ci-cd). The idea is that any new code change that is pushed to the repository is run through user tests via CICD. If a user test fails then the new code will not be deployed.

User test are run from a CLI

# Running user tests locally




# User test deployment via CICD

{% include figure.html img="jenkins-blue-ocean.png" alt="jenkins blue ocean" caption="Jenkins pipeline showing a deployment that failed at the user story tests stage. As the user test stage failed, the code was not deployed to CI and UAT environments" width="100%" %}

Using your pipeline of choice you can run the Behave tests as a CLI command
```bash 
behave  --define baseurl="www.website.com" --define username="Jacky_182" --define password="apples_andPears"
```
The following is the [Jenkins](https://www.jenkins.io/) code used to run the user tests within a CICD pipeline.
```java
steps {
    echo "Run User Acceptance Tests"
    sh 'curl --fail -LI $BASE_URL -o /dev/null -w \'%{http_code}\\n\' -s > /dev/null'

    withCredentials([
        usernamePassword(credentialsId: 'AdminUser', passwordVariable: 'AdminTestPassword', usernameVariable: 'AdminTestUsername')
        ]) {
        dir('./uat') {
            sh(returnStatus: true, script: 'behave --junit --define baseurl="$BASE_URL" --define username="$TestUsername" --define password="$TestUserPassword" --junit --capture --capture-stderr')
            junit testResults: 'reports/*.xml', skipPublishingChecks: true, skipMarkingBuildUnstable: true
            archiveArtifacts artifacts: '**/*.png', followSymlinks: false, allowEmptyArchive: true
        }
    }
}
````

# Trouble-shooting tests
