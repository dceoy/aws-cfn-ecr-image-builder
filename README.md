aws-cfn-container-builder
=========================

AWS CloudFormation stacks of container builder

[![Lint](https://github.com/dceoy/aws-cfn-container-builder/actions/workflows/lint.yml/badge.svg)](https://github.com/dceoy/aws-cfn-container-builder/actions/workflows/lint.yml)

Installation
------------

1.  Check out the repository.

    ```sh
    $ git clone git@github.com:dceoy/aws-cfn-container-builder.git
    $ cd aws-cfn-container-builder
    ```

2.  Install [Rain](https://github.com/aws-cloudformation/rain) and set `~/.aws/config` and `~/.aws/credentials`.

3.  Deploy stacks for a CodeBuild project.

    ```sh
    $ rain deploy \
        --params ProjectName=cb-dev,DefaultImageTag=latest \
        codebuild-project.cfn.yml cb-dev-codebuild-project
    ```
