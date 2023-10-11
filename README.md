# Simplest-Spring-Boot-Hello-World
This project run a simple spring boot project, on receiving a get request, it's response looks like below:
```
Hello FUCKING World!!!
Image: ghcr.io/owenchenxy/example-hello-world:20
```

# Continous Integration/Deployment(CI/CD)
## Overview
This project uses Github Action for CI/CD, the pipeline files are located under directory `.github/workflows`:
- **pipeline.yml**: This is the CI/CD pipeline, will be triggered on any push on `master` branch. It uses `mvn` to build the `.war` file, and then put it to a `docker` image, and then deploy the image to AWS ECS Fargate.
- **pr.yml**: This is the PR pipeline, will be triggered on any `Pull Request` whose target branch is `master`. It simply execute unit tests with `mvn test` command.
- 
## Configuration
### Secrets
There are secrets used in the workflow, which needs to be add in the projects secrets(`Settings -> Security -> Secrets and Variables -> Actions -> New Repository Secrets`)
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`

### Environment Variables
In `.github/workflows/pipeline.yml`, set the below envs to your own:
- `REGISTRY`
- `IMAGE_NAME`

The other envs do not need to be changed. If you want to change the other envs, the corresponding code in the infra repo should also be updated.

# Image Tag
The container image tag is set with the github action workflow's build-in variable `${{ github.run_number }}`.

# Test
To validate the CI/CD process, you can make a small change in the repo and complete a `PR` to the `master` branch. This will trigger a CI/CD workflow. After the workflow is completed. You can visit the service website again, the image tag in the response should increase.

```
Hello FUCKING World!!!
Image: ghcr.io/owenchenxy/example-hello-world:21
```

If you did not observed the increment of the image tag from the service website. Make sure you've disabled the browser cache or try openning the site in a incognito window. If it still does not increase. Wait one more second, everything will be as we wished.
