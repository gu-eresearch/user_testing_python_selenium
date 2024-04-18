---
title: Running user tests
nav: Running user tests
---

Ideally user tests are implemented via CICD (continuous integration, continuous deployment), check the following out for more information about [CICD](https://www.redhat.com/en/topics/devops/what-is-ci-cd). The idea is that any new code change that is pushed to the repository is run through user tests via CICD. If a user test fails then the new code will not be deployed.

User test are run from a CLI

# User test deployment via CICD

{% include figure.html img="jenkins-blue-ocean.png" alt="jenkins blue ocean" caption="Jenkins pipeline showing a deployment that failed at the user story tests stage. As the user test stage failed, the code was not deployed to CI and UAT environments" width="100%" %}



# Running user tests locally




# Trouble-shooting tests
