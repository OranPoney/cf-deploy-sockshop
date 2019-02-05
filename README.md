# Cloud Foundry Deploy Sockshop

This directory contains all shared tasks that are needed to deploy [Dynatrace Sockshop](https://github.com/dynatrace-sockshop) on Cloud Foundry using Concourse.
The pipeline of each microservice is in the **ci** directory of its code repository, e.g., https://github.com/dynatrace-sockshop/carts/ci/pipeline.yml

The scripts provided in this directory run in a BASH and require following tools locally installed: 
* [`git`](https://git-scm.com/) and [`hub`](https://hub.github.com/) that helps you do everyday GitHub tasks without ever leaving the terminal
* [`fly`](https://concourse-ci.org/fly.html) that is logged in to your Concourse. 

Additionally, the scripts need:
* `GitHub organization` to store the repositories of the sockshop application

#### Step 1. Complete credentials file

For the credentials file you need: 
* Login credentials (`username/password`) for a PCF foundation
* An `organization`, and three spaces named: `dev`, `staging`, and `production`
* The `CF API` and `App Domain`
* Dynatrace Tenant with the Dynatrace `Tenant ID`, and a Dynatrace `API Token`
* An aws S3 bucket with the `access key id`, and `secrect key`

1. Copy the `~ci/credentials-sample.yml` to `~/credentials.yml` and exchange all `< >` elements with the proper value.

#### Step 2. Fork Dynatrace sockshop to your GitHub organization

1. Execute the `forkGitHubRepositories.sh` script in the `scripts` directory. This script takes the name of an (empty) GitHub organization, clones all needed repositories, and uses `hub` to fork those repositories to the passed GitHub organization. Afterwards, the script deletes all repositories and clones them again from the GitHub organization.

    ```console
    $ cd ~/scripts/
    $ ./forkGitHubRepositories.sh <GitHubOrg>
    ```

#### Step 3. Create pipelines for all microservices

1. To set up the Concourse pipelines for all microservices use `fly` and run following commands:
    ```console
    $ fly --target pipes login --concourse-url http://127.0.0.1:8080 -u admin -p *******

    $ fly -t pipes set-pipeline -c '~/repositories/carts/ci/pipeline.yml' -p carts -l '~/credentials.yml'
    $ fly -t pipes set-pipeline -c '~/repositories/orders/ci/pipeline.yml' -p orders -l '~/credentials.yml'
    $ fly -t pipes set-pipeline -c '~/repositories/shipping/ci/pipeline.yml' -p shipping -l '~/credentials.yml'
    $ fly -t pipes set-pipeline -c '~/repositories/queue-master/ci/pipeline.yml' -p queue-master -l '~/credentials.yml'
    $ fly -t pipes set-pipeline -c '~/repositories/user/ci/pipeline.yml' -p user -l '~/credentials.yml'
    $ fly -t pipes set-pipeline -c '~/repositories/catalogue/ci/pipeline.yml' -p catalogue -l '~/credentials.yml'
    $ fly -t pipes set-pipeline -c '~/repositories/payment/ci/pipeline.yml' -p payment -l '~/credentials.yml'
    $ fly -t pipes set-pipeline -c '~/repositories/front-end/ci/pipeline.yml' -p front-end -l '~/credentials.yml'

    ```
