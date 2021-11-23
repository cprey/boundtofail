# :skull: Bound to Fail

Here is code that is bound to fail. It's using old docker images and not using best practices in multiple areas. This code demonstrates how GitHub actions can be used to safely create, scan, and publish container images using some nice best practices like _fail-left_ and _don't do unneeded work_

## Steps -

Look at the `.github/workflows/main.yaml` file.

CI Pipeline flow....

1. developer pushes a `feature` branch
    * triggers a SAST code scan of code (python, npm, java etc) - scan issues won't fail the build
    * triggers a scan of the `Dockerfile` scan issues won't fail the build
1. store container artifact locally
    * deploys container and runs mock tests
    * reports go to output folder
    * if these pass, then artifact _promotes_ to integration container repo
    * a GIT tag is cut
1. container image is tagged with _latest_ and _git-tag_ and pushed to CI repo
1. image is deployed into CI
1. tests are fired off
1. report generated
1. if these pass, container is retagged with _git-tag_ and pushed to the final ECR image repo
1. continuous scanning happens on PROD ECR repo

## Improvements Needed

* only build if [files have changed or have been added](https://github.com/tj-actions/changed-files)
* add tagging
* add push to registry _if_ the scan passed
* integrate with tooling selected by InfoSec
* get feedback
  * InfoSec
  * Dev Teams management
  * Architects
  * SRE

## TODO

* Create drawing to visually display the flow
* Create SBOM (software bill of materials)
* Scan for license 
* Auto create release notes
* Helm chart scanning
* Helm chart promotion
* Jira GitHub integration [GitHub link](https://github.com/atlassian/github-for-jira#install-from-github-marketplace)
  * create issues based on failing tests
  * link issues in Jira
* GitHub repo best practices
  * global git
    * set default branch to _main_ `git config --global init.defaultBranch main`
    * create user and email
  * don't allow _anyone_ to push to `main`
