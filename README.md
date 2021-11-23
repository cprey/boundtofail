# :skull: Bound to Fail

Here is code that is bound to fail. It's using old docker images and not using best practices in multiple areas.

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
