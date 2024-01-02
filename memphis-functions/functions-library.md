---
cover: ../.gitbook/assets/Github.png
coverY: 0
---

# Functions library

The entire library can be found here: [https://github.com/memphisdev/memphis-dev-functions](https://github.com/memphisdev/memphis-dev-functions)

<figure><img src="../.gitbook/assets/Screenshot 2023-11-25 at 0.34.14.png" alt=""><figcaption></figcaption></figure>

## Available functions

The available functions in the functions library are:
### add-timestamp
- Add timestamp adds the current time to an event in the timestamp field. The format of the timestamp will be in YYYY-MM-DD HH:MM:SS OFFSET TIMEZONE format.
### flatten-json
- Flattening JSON takes a JSON event and flattens it so all nested structures are removed from it.
### add-geolocation
- Takes an IP from the field the geolocation input points to and using https://ip-api.com, gets geolocation information and places that information into the event with the key as the given out input.
### add-severity
- Add severity checks the event field specified by the input field against a given cutoff and sets the event field severity to values given by the inputs high if the event field was greater than or equal to the cutoff and low if it is lower than the cutoff.
### extract-email
- Extract Email searches the event field given by the input email for emails and sets the event field given by the input out as a list of the emails found.
### remove-fields
- Sparcify messages removes the given keys in the events based on the keys given in the keys input.
### split-with-delimiter
- Split a string key in a JSON object using custom delimiter
### unix-to-datetime
- Unix time to date time takes a given POSIX time and converts it to a more human readable date time format.
### xml-to-json
- XML to JSON converts an XML message to a JSON message. The JSON representation of the message will discard the root element.