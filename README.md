# simple-solid-messaging-node
Simple solid messaging node. No database, no persistency, no nothing. Simply send and receive message through a node between public key addressed clients.

# Configuration
## Whitelisting Public Keys
```
Put rsa public key file into whitelisted-users folder.
```

## Application config
```
IGNORE_WHITELIST_AND_MAKE_THIS_NODE_PUBLIC=false
MAX_MESSAGE_QUEUE=200
MAX_MESSAGE_TO_RETURN=10
```

# Json REST API

## GET AUTH-CHALLANGE
### Request
```
{
    "public-key": "public-key-content",
}
```
### Response
```
{
    "encrypted-auth-challange": "auth-challange",
}
```

## POST AUTH-CHALLANGE
### Request
```
{
    "auth-challange-secret": "secret-auth-challange-data",
}
```
### Response
```
{
    "session-token": "secret-session-token",
    "session-valid-until": "UTC midnight or something independent from the client"
}
```

## POST MESSAGE
### Request
```
{
    "session-token": "secret-session-token",
    "target-public-key": "target-public-key-data",
    "message-content": "my-message",
    "encryption-type": "plain-text"
}
```
### Response
```
Possible status mesages: queued, user-not-available, unknown-error
{
    "status": "",
}
```

## GET MESSAGE
### Request
```
{
    "session-token": "secret-session-token"
}
```
### Response
```
Possible status mesages: success, unknown-error
{
    "status": "success",
    "messages": [
        {
            "sender-public-key": "public-key-id",
            "message-sent-timestamp": "UTC sometime in the past",
            "encryption-type": "plain-text"
            "message-content": "message",
        }
    ],
    "how-many-messages-left": 20
}
```

## POST AVAILABILITY
### Request
```
{
    "session-token": "secret-session-token",
    "is-available": true
}
```
### Response
```
Possible status mesages: success unknown-error
{
    "status": "success",
    "available-until": "UTC sometime in near future"
}
```

## GET AVAILABILITY
### Request
```
{
    "session-token": "secret-session-token",
    "target-public-key-id": "public-key-id"
}
```
### Response
```
Possible status mesages: success, unknown-error
{
    "status": "success",
    "is-available": true
    "available-until": "UTC sometime in near future"
}
```