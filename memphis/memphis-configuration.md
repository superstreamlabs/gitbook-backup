---
description: How and where to configure Memphis internals
---

# Memphis configuration

## Through the GUI

<figure><img src="../.gitbook/assets/Screen Shot 2023-02-21 at 11.24.03.png" alt=""><figcaption></figcaption></figure>

### Parameters

| Parameter                               | Description                                                                                                                                                                                                                       | Possible values                             |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| MAX MESSAGE SIZE                        | Max message size in megabytes                                                                                                                                                                                                     | 1mb-12mb                                    |
| DEAD LETTER MESSAGES RETENTION IN HOURS | Amount of hours to retain dead letter messages in a DLS. Cross system                                                                                                                                                             | 1h - 30h                                    |
| LOGS RETENTION IN DAYS                  | <p>Amount of days to retain system logs. To reduce storage overhead. </p><p><strong>Best practice</strong> would be to keep the local logs retention short and export the logs to a 3rd party tool like Datadog or promethous</p> |  1 day - 100 days                           |
| TIERED STORAGE UPLOAD INTERVAL          | Based on this time interval, the broker will pack and migrate a batch of messages to the second storage tier (if configured)                                                                                                      | 5 seconds - 1 hour                          |
| BROKER HOSTNAME                         | \*For display purpose only\* Which URL should be seen as the "broker hostname" across the GUI components                                                                                                                          | based on the type of deployment: Docker/K8S |
| UI HOSTNAME                             | \*For display purpose only\* Which URL should be seen as the "UI hostname" across the GUI components                                                                                                                              | based on the type of deployment: Docker/K8S |
| REST GATEWAY HOSTNAME                   | \*For display purpose only\* Which URL should be seen as the "REST Gateway hostname" across the GUI components and "generate token" utility                                                                                       | based on the type of deployment: Docker/K8S |

## via configuration file

Soon.

## via API

Soon.



Search terms: environment variables
