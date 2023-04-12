aws-cfn-image-builder
=====================

AWS CloudFormation stacks of CodeCommit-to-ECR integration to build and push a container image

[![Lint](https://github.com/dceoy/aws-cfn-image-builder/actions/workflows/lint.yml/badge.svg)](https://github.com/dceoy/aws-cfn-image-builder/actions/workflows/lint.yml)

```mermaid
sequenceDiagram
  participant U as User
  participant C as AWS CodeCommit
  participant B as AWS CodeBuild
  participant E as Amazon ECR
  U->>C: Push a Git repository into the branch (default: main)
  C->>B: Start running a build if the events rule detects the update
  activate B
  Note over B: built-in buildspec
  C->>B: Pull the source codes
  B->>B: Build the Docker image
  B->>E: Push the Docker image
  deactivate B
```

Installation
------------

1.  Check out the repository.

    ```sh
    $ git clone git@github.com:dceoy/aws-cfn-image-builder.git
    $ cd aws-cfn-image-builder
    ```

2.  Install [AWS CLI](https://aws.amazon.com/cli/) and [Rain](https://github.com/aws-cloudformation/rain), and set `~/.aws/config` and `~/.aws/credentials`.

3.  Create a CodeCommit repository for Dockerfile if it doesn't exist.

    ```sh
    $ aws codecommit create-repository --repository-name your-codecommit-repo
    ```

4.  Create an ECR repository for container images if it doesn't exist.

    ```sh
    $ aws ecr create-repository --repository-name your-ecr-repo
    ```

5.  Deploy stacks for IAM roles.

    ```sh
    $ rain deploy \
        --params ProjectName=ib-dev \
        iam-roles-for-codebuild.cfn.yml \
        ib-dev-iam-roles-for-codebuild
    ```

6.  Deploy stacks for a CodeBuild project.

    ```sh
    # Replace your-codecommit-repo and your-ecr-repo with your repository names
    $ rain deploy \
        --params ProjectName=ib-dev,CodeCommitRepositoryName=your-codecommit-repo,EcrRepositoryName=your-ecr-repo,IamStackName=ib-dev-iam-roles-for-codebuild \
        codebuild-project.cfn.yml ib-dev-codebuild-project
    ```

Usage
-----

1.  Push a Git repository including Dockerfile to the CodeCommit repository.
    (The example below requires [git-remote-codecommit](https://github.com/aws/git-remote-codecommit).)

    ```sh
    $ cd /path/to/your/git/project
    $ git push codecommit://your-codecommit-repo main:main
    ```
