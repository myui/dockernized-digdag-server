# dockernized-digdag-server

Dockerfile to run [digdag-server](https://github.com/treasure-data/digdag) on Amazon ECS.

The dockerfile is based on one in [this repository](https://github.com/IntimateMerger/dockerfile-digdag-server).

# Pull and Run Docker image in Dockerhub

You can used [our prepared docker image](https://hub.docker.com/r/myui/digdag-server/tags/) in Dockerhub.

```sh
docker pull myui/digdag-server:latest
docker run -p 65432:65432 myui/digdag-server
```

http://localhost:65432/

# Pushing image to Amazon ECR

## 1. login to ECS

```sh
eval $(aws ecr get-login --region us-east-1 --profile ${YOUR_PROFILE} --no-include-email)
```

## 2. build a docker image

```sh
docker build -t ${YOUR_REPOS}/digdag-server -f Dockerfile .
```

`${YOUR_REPOS}/digdag-server` is ECR repository name

## 3. Push a docker image to ECR

```sh
docker tag ${YOUR_REPOS}/digdag-server:latest xxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/${YOUR_REPOS}/digdag-server

docker push xxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/ml-api/digdag-server
```

# Run docker image on ECS

You can run digdag-server instance on ECR GUI. 

CLI command to appear.

# Connect from DigDag client

## 1. Configure client endpoint to the docker server

```sh
cat ~/.config/digdag/config
```

> client.http.endpoint = http://ec2-aaa-bbb-ccc-ddd.compute-1.amazonaws.com:65432

## 2. Connect to Digdag server

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
