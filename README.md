# Cloud Foundry Deploy Sockshop

This directory contains all shared tasks that are needed to deploy [Dynatrace Sockshop](https://github.com/dynatrace-sockshop) using Concourse.
The pipeline of each microservice is in its code repository, e.g., https://github.com/dynatrace-sockshop/carts/ci/pipeline.yml

To set up an Concourse pipeline for one microservice use the fly cli:
    
```console
$ fly -t pipes set-pipeline -c '~\dynatrace-sockshop\carts\ci\pipeline.yml' -p carts -l '~\credentials.yml'
```
