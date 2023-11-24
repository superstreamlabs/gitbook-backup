---
description: A "Getting started" guide for Memphis Functions
cover: ../.gitbook/assets/Github.png
coverY: 0
---

# Getting started

### How to develop a new private function

Memphis functions offers two types of function libraries: Public, and private.\
The public library is available by default for each account and is powered by Memphis.dev Functions [repository](https://github.com/memphisdev/memphis-dev-functions).

The private library is available for your account only and therefore requires the user to develop functions and integrate its account with the designated repositories.

A function comprises code files (based on [Memphis template](https://github.com/memphisdev/memphis-dev-academy/tree/master/memphis-functions)) and a `memphis.yaml` file contained within a unified directory.\
The directory ought to be included in a Git repository that's linked with Memphis.\
Here is a brief hierarchy diagram of how a compatible function file tree should be constructed:&#x20;

<figure><img src="https://private-user-images.githubusercontent.com/70286779/284149937-57591907-ce74-454c-a9e3-f7348db88c48.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDA4NjY1MTIsIm5iZiI6MTcwMDg2NjIxMiwicGF0aCI6Ii83MDI4Njc3OS8yODQxNDk5MzctNTc1OTE5MDctY2U3NC00NTRjLWE5ZTMtZjczNDhkYjg4YzQ4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjQlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI0VDIyNTAxMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTFhZjU0ZjNiY2VlNTM5Y2RjNDFmYmVjOGZkMTQ1NGYwMTU4NmU1ZWRiY2JjOTRiMzc2MzdlYjYxMjYxNDE0NmEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.Z6gIZd5mzaC2OFKT84-yf2BEC1J3_1Y0pB6TPnf9pAQ" alt=""><figcaption></figcaption></figure>

#### 🚀  Step-by-step Guide:

1. Clone or create a new repository (At the moment, support is exclusively available for GitHub.)
2. Within this repository, establish a fresh directory and initialize it to your chosen programming language

```bash
mkdir my-function && cd my-function && npm init -y
```

3. [Copy](https://github.com/memphisdev/function-templates) one of the Memphis Functions templates. For this guide, we chose Node.js
4. _**Required**_**.** Write your logic inside the `eventHandler` block.\
   Incoming events will be accumulated and dispatched to a function collectively in a batch; therefore, the wrapper

{% code lineNumbers="true" %}
```javascript
exports.handler = async (event) => {
    return await createFunction(event, eventHandler);
};

/**
 * https://github.com/memphisdev/memphis.js/tree/functions_wrapper#creating-a-memphis-function
 * @param {Uint8Array} payload 
 * @param {Object} headers 
 * @param {Object} inputs 
 * @returns {Object} 
 */
function eventHandler(payload, headers, inputs) {
    // Handle event here

    // A short example of converting the payload to a json object and returning it as an Uint8Array
    const decodedPayload = payload.toString('utf-8');
    const asJson = JSON.parse(decodedPayload);

    return {
        processedMessage: Buffer.from(JSON.stringify(asJson), 'utf-8'),
        processedHeaders: headers
    };
}
```
{% endcode %}

Messages will return to the Memphis Station in a batch as well.\
5\. _**Required**_**.** Add or modify the `memphis.yaml` file based on the following template:

```
function_name:        #Required. Must be equal to the directory name
runtime:              #Required. [go | nodejs | nodejs16.x | nodejs18.x | python3.8 | python3.9 | python3.10 | python3.11]
dependencies:         #The file name contains the list of dependencies the function making use of - default to [requirements.txt(python) / go.mod(go) / package.json (nodes)]
handler:              #Required for node.js/Python only. The name of the function's entry point - <file name>.<function name> - for example, if your function is called 'handler' and written inside 'main.py', the handler should be main.handler
tags:                 #List of tags
  - tag: json
  - tag: dev
inputs:               #list of input fields that will be injected into your function per attachment
  - name: timestamp
description:          #Description
```

6. _**Optional**_**.** Add a `README` file to describe your function so others will know what to do :)
7. _**Optional.**_ Add `test.json` so when a user tests the function, the `test event` will already be populated
8. Connect the designated repository with your Memphis account
9. `my-function` should be available through the Functions main page or a station

### How to develop a new public function

1. Fork [https://github.com/memphisdev/memphis-dev-functions](https://github.com/memphisdev/memphis-dev-functions)
2. Add your function's directory, including `memphis.yaml` file
3. Create a PR
4. The addition of the new function will take place following a thorough review and subsequent approval
5. Get your swag pack! 😍
