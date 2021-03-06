![npm](https://img.shields.io/npm/v/@jupita/sdk)

# Jupita Javascript SDK
This library will allow you to make the required `dump` API calls with Jupita. All API calls are made asynchronously, thus there are event listeners available to handle the API results.


## Overview
Jupita is an API product that provides omnichannel communications analytics using OAuth 2.0 authorisation. Within the SDK documentation, `MessageType` refers to which user the utterance is from. `MessageType` 0 = `Touchpoint` and `MessageType` 1 = `Input`, although these labels are handled by the SDK.

Within the dashboard UI touchpoints are referred to as 'channels', and inputs are referred to as 'customers'.

The required parameters for the APIs include setting `channelType`, `MessageType` along with assigning a `touchpointId` + `inputId` to be passed. Please note when assigning the `touchpointId` that no data will be available for that particular touchpoint until the touchpoint has sent at least 1 utterance via the `dump` API. 

You may set any touch`Touchpoint`point or `Input` ID format within the confines of JSON. How this is structured or deployed is completely customisable, for example, you may wish to use full names for users from your database, or you may wish to apply sequencing numbers for `Input` users where the user is not known. 

`Touchpoint` & `Input` IDs must be unique to that user. When dumping an initial `Touchpoint` utterance where there is no `Input` user, such as creating a new Twitter post, simply pass a nominal `inputId` each time, such as '0' for example. It is also recommended to utilise recognisable labels when creating `touchpointId`'s as this will make it easier to identify channels within the dashboard UI, e.g. 'John Smith', 'Jane Doe', 'Twitter Feed', etc.

## APIs
There is one API within the Jupita product – `dump`:

- `dump` allows you to dump each utterance.


##  Quickstart
### Step 1
Install Jupita;

```
npm install @jupita/sdk
```

### Step 2
Build Jupita. Insert your token & touchpoint ID;

```
const { Jupita } = require("@jupita/sdk")

const jupita = new Jupita(token, touchpointId)
```

### Step 3
Dump an utterance from a touchpoint by calling the dump API as a message by specifying the message text and the ID of the input, represented in the example below as '3'. Message dumps are by default from a touchpoint unless otherwise specified. 

The parameter `channelType` is required. This allows you to specify which channel you are deploying the SDK to. You may enter any name you wish in order to identify the channel type in the dashboard UI, e.g "Email", "Web chat", "Social media", "Phone", etc.

The parameter `isCall` is also required and set to false by default. This tells Jupita if the utterance is from an audio call. When dumping an utterance from an audio call, set the `isCall` parameter to `true` otherwise set to `false`;

```
const { Jupita, MessageType } = require("@jupita/sdk")

jupita.dump("Hi, how are you?", 3, "Web chat", MessageType.Touchpoint, false)
```

Similarly, call the dump API whenever dumping an utterance from an input by specifying the message text and ID of the input;
```
const { Jupita, MessageType } = require("@jupita/sdk")

jupita.dump("Hi, good thanks!", 3, "Web chat", MessageType.Input, false)
```

Additionally, you may define a listener as per below;

```
jupita.dump("Hi, good thanks!", 3, "Web chat", MessageType.Input, false, {
    onError: (statusCode, response) => {
        console.log(statusCode)
        console.log(response)
    }, 
        onSuccess: (week) => {
        console.log(week)
    }
})
```

## Error handling
- The SDK will throw an `InvalidParameterException` error which occurs if the `MessageType` set in the dump method is not 1 or 0.
- `JSONException` which occurs if the user input is not JSON compatible. This can be incorrect usage of strings when passed on to the Jupita methods.
- A 401 error code is thrown when the token is incorrect, otherwise Jupita returns error 400 with details.


## Libraries
Use Step 1 and 2 so that the Jupita Javascript SDK is available within the scope of the project.


## Classes
The available product under the Javascript SDK is Jupita. Jupita can be constructed directly using the public constructor but it is highly recommended to use the built class. This will ensure that mistakes are not made while building Jupita.

```
const { Jupita } = require("@jupita/sdk")

const jupita = new Jupita(token, touchpointId)
```

## API Reference
### `dump` method definition

* text (required)
* inputId (required)
* channelType (required)
* MessageType (required, default = Touchpoint)
* isCall (required, default=false)
* listener (optional)

To avoid illegal argument error for `MessageType`, use `MessageType.Touchpoint` or `MessageType.Input`.

## Support
If you require additional support please contact support@jupita.io
