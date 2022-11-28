---
description: Memphis notifications to slack channel
cover: ../../../.gitbook/assets/Slack + Memphis.jpeg
coverY: 0
---

# Slack

Receive alerts and notifications directly to your chosen slack channel for faster response and better real-time observability.

### Step 1: Create an app

Please [create](https://api.slack.com/apps/new) an app 'from scratch'

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-23 at 17.09.19.png" alt=""><figcaption></figcaption></figure>

Choose a name. For example - "Memphis"

Choose a workspace.

### Step 2: Configure the slack app

Under "Add features and functionality", choose "Bots"

<figure><img src="../../../.gitbook/assets/1_DwudexxFOihUUHEvAeJe6A.png" alt=""><figcaption></figcaption></figure>

Assign scope by clicking on the "Review Scopes to Add"

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Add the following scopes

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-23 at 20.51.02.png" alt=""><figcaption></figcaption></figure>

Install the app (Sometime you need to switch pages to "Install app" on the left menu)

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-23 at 20.52.34.png" alt=""><figcaption></figcaption></figure>

### Step 3: Implement the token in Memphis

Once the slack app is installed, grab the "Bot User OAuth Token" and paste into Memphis' slack integration model

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-23 at 20.55.27.png" alt=""><figcaption></figcaption></figure>

\*memphis model\*

For the channel ID -> left-click over the designated slack channel in your workspace -> View channel details -> Scroll all the way down in the "About" tab -> Copy and paste the ID

<figure><img src="../../../.gitbook/assets/Screen Shot 2022-11-23 at 21.01.18.png" alt=""><figcaption></figcaption></figure>

