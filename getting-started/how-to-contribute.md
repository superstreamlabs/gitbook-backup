# How to contribute?

Memphis is and always will be open-source and community-driven. \
Our community is our power. [💙](https://emojipedia.org/blue-heart/)

## Why should you become a contributor?

{% hint style="info" %}
"Working on Memphis OS project helped me earn many of the skills I later used for my studies in university and my actual job. I think working on open-source projects helps me as much as it helps the project!"
{% endhint %}

Contributing to open source can be a rewarding way to learn and increase your experience.

Whether it’s coding, user interface design, graphic design, docs, or writing content, if you’re looking for practice, there’s a task for you on an open-source project.

## Getting started

### 1. Establish a local memphis environment

&#x20; 0\. Join to Memphis [discord](https://discord.gg/WZpysvAeTf) channel

&#x20; 1\. Install [Golang](https://go.dev/doc/install)

&#x20; 2\. Fork Memphis [broker](https://github.com/memphisdev/memphis-broker)

&#x20; 3\. Clone the forked repo to your local station

&#x20; 4\. Run a local "memphis-metadata" db using docker

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose-dev-env.yml -o docker-compose-dev-env.yml && docker compose -f docker-compose-dev-env.yml -p memphis up
```

&#x20; 5\. Install broker dependencies - enter the cloned directory and run

```
go get -d -v .
```

&#x20; 6\. Run the broker in debug mode (If you're using vscode, click F5) or run via terminal via:

```
DEV_ENV="true" DOCKER_ENV="true" ANALYTICS="false" go run main.go
```

### 2. You are

* [Frontend Developer](how-to-contribute.md#frontend-contributions)
* [Backend Developer](how-to-contribute.md#backend-contributions)
* [Data Engineer](how-to-contribute.md#data-engineer)
* [DevOps](how-to-contribute.md#devops)

#### Frontend Contributions

&#x20; 1\. The source files of the UI can be found in a directory called ״[ui\_src](https://github.com/memphisdev/memphis-broker/tree/master/ui\_src)״

&#x20; 2\. Navigate to "ui\_src" dir

&#x20; 3\. Install dependencies by running `npm install`

&#x20; 4\. Run the UI locally by running `npm start`

&#x20; 5\. Start coding! Here are some ["Good first issues"](https://github.com/memphisdev/memphis-broker/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)

&#x20; 6\. Once done - push your code and create a pull request to merge your updates with memphis main repo

#### Backend Contributions

Once your Memphis' local dev environment is ready, you can start coding!

Memphis backend options are -&#x20;

1. Memphis Broker
2. Client libraries
3. Support/add more protocols
4. [Memphis CLI](https://github.com/memphisdev/memphis-cli)

Grab a ["Good first issue"](https://github.com/memphisdev/memphis-broker/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22), and once done - push your changes and open a "pull request"

#### Data Engineer

As a data engineer, it would be great to get your feedback, potential use cases, QA, and push memphis to the limit in terms of data workloads would be an amazing contribution, as at the end of the day, you are our champion!

#### DevOps

As a DevOps engineer, you can find multiple paths of contribution

1. [Helm deployment](https://github.com/memphisdev/memphis-k8s)
2. [Terraform](https://github.com/memphisdev/memphis-terraform)&#x20;
3. [Docker](https://github.com/memphisdev/memphis-docker)
4. DevOps [Roadmap](https://github.com/orgs/memphisdev/projects/2/views/1?filterQuery=label%3A%22epic%3A+deployment%22)
