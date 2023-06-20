---
description: Deliver Memphis notifications to a slack channel
cover: ../../.gitbook/assets/Slack + Memphis.jpeg
coverY: 0
---

# Slack

## Introduction

Receive alerts and notifications directly to your chosen slack channel for faster response and better real-time observability.

## Getting started

### Step 1: Create an app

Please [create](https://api.slack.com/apps/new) an app 'from scratch'

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-23 at 17.09.19.png" alt=""><figcaption></figcaption></figure>

Choose a name. For example - "Memphis"

Choose a workspace.

### Step 2: Configure the slack app

Under "Add features and functionality", choose "Bots"

<figure><img src="../../.gitbook/assets/1_DwudexxFOihUUHEvAeJe6A.png" alt=""><figcaption></figcaption></figure>

Assign scope by clicking on the "Review Scopes to Add"

<figure><img src="../../.gitbook/assets/image (5) (2).png" alt=""><figcaption></figcaption></figure>

Add the following scopes

<figure><img src="../../.gitbook/assets/Screenshot 2022-12-04 at 10.36.39.png" alt=""><figcaption></figcaption></figure>

Install the app (Sometimes you need to switch pages to "Install app" on the left menu)

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-23 at 20.52.34.png" alt=""><figcaption></figcaption></figure>

### Step 3: Invite the new bot to the required channel

Left-click over the designated slack channel in your workspace -> View channel details -> Go to "Integrations" tab -> Under "Apps" click on "Add an App".

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-22 at 15.17.59.png" alt=""><figcaption></figcaption></figure>

Search for the newly created slack app and click "Add".

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-22 at 14.56.38.png" alt=""><figcaption></figcaption></figure>

### Step 4: Implement the token in Memphis

Once the slack app is installed, grab the "Bot User OAuth Token" and paste it into Memphis' slack integration model

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-04 at 13.25.20.png" alt=""><figcaption></figcaption></figure>

For the bot token

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-23 at 20.55.27.png" alt=""><figcaption></figcaption></figure>

For the channel ID -> left-click over the designated slack channel in your workspace -> View channel details -> Scroll down in the "About" tab -> Copy and paste the ID

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-23 at 21.01.18.png" alt=""><figcaption></figcaption></figure>

###
