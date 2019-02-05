# Cloud Foundry Deploy Sockshop

This directory contains all shared tasks that are needed to deploy [Dynatrace Sockshop](https://github.com/dynatrace-sockshop) on Cloud Foundry using Concourse.
The pipeline of each microservice is in the **ci** directory of its code repository , e.g., https://github.com/dynatrace-sockshop/carts/ci/pipeline.yml

#### Step 1. Complete credentials file

1. Copy the `credentials-sample.yml` to `credentials.yml` and exchange all `< >` elements with the proper value.

#### Step 2. Create pipeline for a microservice

1. To set up a Concourse pipeline for one microservice use the `fly` cli:
```console
$ fly --target pipes login --concourse-url http://127.0.0.1:8080 -u admin -p *******
$ fly -t pipes set-pipeline -c '~\dynatrace-sockshop\carts\ci\pipeline.yml' -p carts -l '~\credentials.yml'
```
