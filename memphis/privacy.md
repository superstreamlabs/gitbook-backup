# Privacy

For our engineering team to track and resolve bugs in real-time and our product team to understand which feature is more required or did not build right, Memphis collects **anonymous** metadata **only.**

### Why?

We are constantly exploring and researching how to build the best dev-product we can with value, features, and capabilities built by our users' requests and because we are open-sourced first. It means that we are almost entirely "blind" to understand what our users like or not. **That is the only reason we are collecting anonymous metadata.**

### Which metrics does Memphis collect?

* Deployment event
* Login event (If login event happened, not creds!)

### Can I disable it?

ALWAYS. Please keep in mind that you are helping Memphis directly to become better, but we understand if you prefer not to.

{% tabs %}
{% tab title="Docker" %}
When downloading Memphis docker-compose file

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose.yml -o docker-compose.yml
```

Please change the env "ANALYTICS" to "false"

```
services:
  mongo:
    image: "memphisos/mongo:4.2"
    restart: on-failure
    pull_policy: always
    networks:
      - memphis
    ports:
      - "27017:27017"
  broker:
    image: "memphisos/memphis-broker:2.7.2-alpine3.15-docker"
    restart: on-failure
    pull_policy: always
    networks:
      - memphis
    ports:
      - "7766:4222"
    command: >
      -js --auth=memphis
  control-plane:
    image: "memphisos/memphis-control-plane:latest"
    restart: on-failure
    pull_policy: always
    networks:
      - memphis
    ports:
      - "5555:5555"
      - "6666:6666"
    environment:
      - ROOT_PASSWORD=memphis
      - CONNECTION_TOKEN=memphis
      - DOCKER_ENV="true"
      - ANALYTICS="false"
  ui:
    image: "memphisos/memphis-ui:0.1.0-docker"
    restart: on-failure
    pull_policy: always
    networks:
      - memphis
    ports:
      - "9000:80"
    environment:
      - DOCKER_ENV="true"
networks:
  memphis:
    ipam:
      driver: default
```
{% endtab %}

{% tab title="Kubernetes" %}
When installing Memphis using "Helm", please add the option `analytics="false"`

```
helm install my-memphis --set analytics="false" memphis/memphis --create-namespace --namespace memphis
```

****
{% endtab %}
{% endtabs %}

