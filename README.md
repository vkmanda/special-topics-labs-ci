# Special Topics Continuous Integration Lab

## Objective

For this lab, you will gain hands-on experience using Jenkins for continuous integration and managing your Jenkins configuration
as code.  You will write pipeline jobs using the Groovy DSL and create those jobs in a "dockerized" Jenkins managed by a DSL seed job.

## Getting Started:

1. Copy the starter code from here into a new, private repository in your personal GitHub account using [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#copy-the-starter-code-into-a-new-private-repository-in-your-personal-github-account) substituting this repository URL ``https://github.com/jschmersal-cscc/special-topics-labs-ci`` for the one referenced in that document.  When adding a collaborator, be sure to add me ("jschmersal-cscc").
2. Create a new branch for your code changes as described in [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#before-you-start-coding)

## Understanding the Starter Code
This repository contains all you need to have a fully dockerized Jenkins that supports a DSL seed job and 
configurable plugins.  Note that you shouldn't have to change either the seed job provided or the plugins that are 
provided.  However, it's helpful to walk through what is provided, to better understand what you need to do.

#### Building a Custom Jenkins Image
You are provided with a [build-jenkins-image.sh](build-jenkins-image.sh) script that will build the `edu.cscc.special-topics/jenkins:latest` Jenkins image.  You can look under [docker/](docker/) to see how the image is built:
1. There is, of course, a [Dockerfile](docker/Dockerfile).  The `Dockerfile` is fairly self-explanatory.  Note that it uses `jenkins/jenkins:lts` as its base image.
1. The `Dockerfile` references [init.groovy.d](docker/init.groovy.d).  Scripts in this directory in Jenkins home will be automatically executed during Jenkins startup.  There are 3 scripts in here; two of them install Tools that Jenkins can use (Java and Maven).  The third is more important.  [startup.groovy](docker/init.groovy.d/startup.groovy) creates the [seed](docker/jobs/seed.groovy) job and runs it at startup.  This is great because you can go from a "naked" Jenkins to one with all of your jobs defined and ready to go without any manual intervention.  For more on seed jobs, see [the Jenkins Job DSL Tutorial](https://github.com/jenkinsci/job-dsl-plugin/wiki/Tutorial---Using-the-Jenkins-Job-DSL).
1. The `Dockerfile` references [plugins.txt](docker/plugins.txt).  This text file simply contains a list of Jenkins plugins we wish to be installed as part of the image build.  Note that there are more plugins listed than necessary, but it gives you a flavor of the rich plugin environment that Jenkins supports.
1. The [seed job](docker/jobs/seed.groovy) is set up to create jobs based on the files under the [dsl](docker/dsl) directory.  For your setup there is only one job, [build_pipeline.groovy](docker/dsl/build_pipeline.groovy) so this whole setup seems like overkill.  However, it is common for a given Jenkins instance to host 10s or 100s of jobs, and managing that many jobs through the Jenkins GUI is cumbersome and difficult.  Study the structure of `build_pipeline.groovy`.  Note that it creates a Pipeline job based on the instructions provided in the [Jenkinsfile](Jenkinsfile) from this repo.

#### The Maven Project
The source code for this project is quite simple.  It's pretty much the unit test example from the quality lab.  You won't need to change any of its code.  Note that if you want you can build it from the command line with a simple `mvn clean install`

## Completing the Assignment

1. For this lab you will modify the existing Jenkins configuration to have the pipeline job:
    1. Checkout _your_ repository
    1. Build the source code using a maven stage
    1. Report the unit test results
1. To do so, you will need to:
    1. Change the [build_pipeline.groovy](docker/dsl/build_pipeline.groovy) to use your repository
    1. Modify your [Jenkinsfile](Jenkinsfile) appropriately. 

## Hints
1. I created a [run-jenkins.sh](run-jenkins.sh) script that will start up jenkins for you if you'd like.  You can view your Jenkins by opening Firefox within your workspace to [http://localhost:8080](http://localhost:8080).
1. If your repository is marked private, you might get an error when running your `build-pipeline` job.  To fix this, you can [add your credentials](https://jenkins.io/doc/book/using/using-credentials/#adding-new-global-credentials) and select them in the SCM section of the  [build-pipeline job configuration](http://localhost:8080/job/build-pipeline-job/configure).
1. The [DSL API](https://jenkinsci.github.io/job-dsl-plugin/) is available online.
1. There is a [Pipeline Syntax](http://localhost:8080/job/build-pipeline-job/pipeline-syntax/) page to help you out too!.

## Submitting Your Work

1. Create a pull request for your branch using [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#once-you-are-ready-to-submit-your-work-for-grading)
1. Submit the assignment in Blackboard as described in [these instructions](https://github.com/jeff-anderson-cscc/submitting-assignments-lab#once-your-pull-request-is-created-and-i-am-added-as-a-reviewer)

__NOTE: I will provide feedback via. comments in your pull request.__
If you need to amend your work after you issue your initial pull request:

1. Commit your updates
1. Push your changes to gitHub
1. Verify the new commits were automatically added to your open pull request
