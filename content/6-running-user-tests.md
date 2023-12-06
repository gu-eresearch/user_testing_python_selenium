---
title: Running user tests
nav: Running user tests
---

Ideally user tests are implemented via CICD (continuous integration, continuous deployment), check the following out for more information about [CICD](https://www.redhat.com/en/topics/devops/what-is-ci-cd). The idea is that any new code change that is pushed to the repository is run through user tests via CICD. If a user test fails then the new code will not be deployed.

User test are run from a CLI


# User test deployment via CICD

{% include figure.html img="jenkins-blue-ocean.png" alt="jenkins blue ocean" caption="Jenkins pipeline showing a deployment that failed at the user story tests stage. As the user test stage failed, the code was not deployed to CI and UAT environments" width="100%" %}

The following is the [Jenkins](https://www.jenkins.io/) code used to run the user tests within a CICD pipeline.
```java
steps {
    echo "Run User Acceptance Tests"
    sh 'curl --fail -LI $BASE_URL -o /dev/null -w \'%{http_code}\\n\' -s > /dev/null'

    withCredentials([
        usernamePassword(credentialsId: 'LFTestAdminUser', passwordVariable: 'LFAdminTestPassword', usernameVariable: 'LFAdminTestUsername'),
        usernamePassword(credentialsId: 'LF_LimeTestUser', passwordVariable: 'LF_LimeTestPassword', usernameVariable: 'LF_LimeTestUsername')
        ]) {
        dir('./uat') {
            sh(returnStatus: true, script: 'behave --junit --define baseurl="$BASE_URL" --define username="$LFAdminTestUsername" --define password="$LFAdminTestPassword" --define limeurl="$LIMESURVEY_URL" --define lime_user="$LF_LimeTestPassword" --define lime_password="$LF_LimeTestUsername" --junit --capture --capture-stderr')
            junit testResults: 'reports/*.xml', skipPublishingChecks: true, skipMarkingBuildUnstable: true
            archiveArtifacts artifacts: '**/*.png', followSymlinks: false, allowEmptyArchive: true
        }
    }
}
````


# Running user tests locally




# Trouble-shooting tests
