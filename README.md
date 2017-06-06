# dockerfile-digdag-server

Dockerfile to run [digdag-server](https://github.com/treasure-data/digdag) on Amazon ECS.

The dockerfile is based on one in [this repository](https://github.com/IntimateMerger/dockerfile-digdag-server).

# Pushing image to Amazon ECR

## login to ECS

```sh
eval $(aws ecr get-login --region us-east-1 --profile ${YOUR_PROFILE})
```

## build a docker image

```sh
docker build -t ${YOUR_REPOS}/digdag-server -f Dockerfile .
```

`${YOUR_REPOS}/digdag-server` is ECR repository name

## Push a docker image to ECR

```sh
docker tag ${YOUR_REPOS}/digdag-server:latest xxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/${YOUR_REPOS}/digdag-server

docker push xxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/ml-api/digdag-server
```

# Connect from DigDag client

## Configure client endpoint to the docker server

```sh
cat ~/.config/digdag/config
```

> client.http.endpoint = http://ec2-aaa-bbb-ccc-ddd.compute-1.amazonaws.com:65432

## Connect to Digdag server

```sh
digdag sessions

digdag push hackathon-project
```

# Environment

If you don't set the env, digdag use the default env.

| name | default | description |
| --- | --- | --- |
| DB_TYPE | memory | exp.) postgresql  |
| DB_USER | digdag | for postgresql |
| DB_PASSWORD | digdag | for postgresql |
| DB_HOST | 127.0.0.1 | for postgresql |
| DB_PORT | 5432 | for postgresql |
| DB_NAME | digdag | for postgresql |
