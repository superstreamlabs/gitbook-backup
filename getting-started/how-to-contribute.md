---
description: A manual for the new community members!
---

# How to contribute?

That's amazing that you reached this page!

Memphis is and always will be open-source and community-driven. Our community is our power.

### Why you should become a contributor?

"Working on \[Memphis] helped me earn many of the skills I later used for my studies in university and my actual job. I think working on open source projects helps me as much as it helps the project!"

Contributing to open source can be a rewarding way to learn, teach, and build experience in just about any skill you can imagine.

Whether it’s coding, user interface design, graphic design, writing, or organizing, if you’re looking for practice, there’s a task for you on an open source project.

### You are

{% tabs %}
{% tab title="Frontend developer" %}
#### 0. Join to Memphis [discord](https://discord.gg/WZpysvAeTf) channel

#### 1. Establish a dev environment

The UI is written in React, therefore you would need [node.js](https://nodejs.org/) and [npm](https://npmjs.com) installed on your local machine.



Let's start by <mark style="color:blue;">**forking**</mark> the UI repo.

Now clone it

```
git clone https://github.com/<username>/memphis-ui.git
```

Run the broker using this docker in the background

```
curl -s https://memphisdev.github.io/memphis-docker/docker-compose-dev-ui.yaml -o docker-compose.yaml && \
docker compose -f docker-compose.yaml -p memphis up
```

Install dependencies

```
npm install
```

Run the UI

```
npm start
```

#### 2. Start with a ["Good first issue"](https://github.com/memphisdev/memphis-ui/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)

#### 3. Grab more tasks from the [Open Tasks Board](https://github.com/orgs/memphisdev/projects/1)
{% endtab %}

{% tab title="Backend developer" %}
#### Soon...
{% endtab %}

{% tab title="Data Engineer" %}
#### Soon...
{% endtab %}

{% tab title="DevOps" %}
#### Soon...
{% endtab %}

{% tab title="QA" %}
#### Soon...
{% endtab %}
{% endtabs %}

### Merge your changes

Create a new branch (from the <mark style="color:purple;">**"staging"**</mark> branch) for your fix.

```
git checkout -b branch-name-here staging
```

Commit and Push your changes

```
git add .
```

```
git commit -m "[issue-fix/feature] <Description of what you did>"
```

```
git push
```

The "push" will create a pull request in the source repo you forked.

Describe exactly what you did and mention (using a hashtag) the issue/feature related to your PR&#x20;

and that's it!

### Write about it

Like this for example\
![](../.gitbook/assets/image.png)

### Get a sweet swag pack!

\
![Image](https://media.discordapp.net/attachments/963333392844328964/999672120344850452/WhatsApp\_Image\_2022-07-21\_at\_15.00.16.jpeg?width=332\&height=300)

We will reach out for delivery address :)
