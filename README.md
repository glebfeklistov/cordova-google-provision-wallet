# Google Pay (GPAY)

This plugin implements Google Pay Push Provisioning API features for card enrollment integration. Lets customers add a bank card to Google Pay from the issuer app by providing the required card details.

## Supported Platforms

- Android

## Using
It's only documentation for preview of capabilities. If you want you this plugin, contact with author for getting full version plugin.

## Installation

Type this string in cordova application root:

```sh
cordova plugin add cordova-google-provision
```

## Get stable hardware id
Each physical Android device has a stable hardware ID which is consistent between wallets for a given device. This ID will change as a result of a factory reset.

```js
getStableHardwareId(successCallback, errorCallback);
```

Success callback structure:

```json
{
    "stableHardwareId": "Qw9ntt8G_iYYq--yNPZLOGD7"
}
```

## Get active wallet id
Returns the Wallet ID of the active wallet. If there is no active wallet, the status TAP_AND_PAY_NO_ACTIVE_WALLET is returned.

```js
getActiveWalletId(successCallback, errorCallback);
```

Success callback structure:

```json
{
    "activeWalletId": "10A0HAmLKziwj69ySpiDu69G"
}
```

## Activate Google Pay
Some issuers may need the active wallet ID before creating an opaque payment card for push provisioning. In the event a wallet ID is needed but getActiveWalletId returns TAP_AND_PAY_NO_ACTIVE_WALLET, the issuer can call the createWallet method to launch the wallet creation workflow. If the wallet ID does not need to be included in the opaque payment card, simply let the tokenize and pushTokenize manage wallet creation automatically.

Calling createWallet will start an Activity that prompts the user:

- To add a Google account: if the user is not logged in to Google on this device
- To select a Google account: if more than one account is available.

```js
createWallet('OPERATION', successCallback, errorCallback);
```

## Check already enrolled cards by provision token
Return the token status for a token in the active wallet. The Push Provisioning API caches status values retrieved from the networks and makes a best effort to keep these cached values up-to-date. However, in some cases the values returned here may be different from the network values. For example, if a user has cleared data so that the token is no longer on the device, the status TAP_AND_PAY_TOKEN_NOT_FOUND is returned.

```js
getTokenStatus("CARD_NETWORK", "Token", successCallback, errorCallback);
```

Success callback structure:

```json
{
    "status": "TOKEN_STATE", 
    "token": "DNITHE462022432238102030"
}
```

## Add a card to Google Pay Wallet (push provisioning)
Starts the push tokenization flow in which the issuer provides most or all card details needed for Google Pay to get a valid token. Tokens added using this method are added to the active wallet.

```js
pushTokenize("OPERATION", "NETWORK", "PROVIDER", "CARD_NAME", "FPAN", "OPC", successCallback, errorCallback);
```

Success callback structure:

```json
{
    "token": "DNITHE302022433796989780"
}
```

## Data Change Callbacks
The Push Provisioning API will immediately call an issuer app callback whenever the following events occur:

- The active wallet changes (by changing the active account)
- The selected card of the active wallet changes
- Tokenized cards are added or removed from the active wallet
- The status of a token in the active wallet changes

Registering for these broadcasts allows an issuer app to re-query information about their digitized cards whenever the user makes a change.

> Only foreground applications will be notified of the data change events. Therefore, each application should update the token statuses not only by receiving DataChangedListener but also when the application launches or returns to the foreground.

```js
registerDataChangedListener(changeCallback);
```

## General error callback structure

```json
{
    "error": 1500
}
```

## List of operations

```json
{
  "REQUEST_CODE_TOKENIZE": 1,
  "REQUEST_CODE_PUSH_TOKENIZE": 3,
  "REQUEST_CREATE_WALLET": 4
}
```

## List of card networks

```json
{
    "CARD_NETWORK_AMEX": 1,
    "CARD_NETWORK_DISCOVER": 2,
    "CARD_NETWORK_MASTERCARD": 3,
    "CARD_NETWORK_VISA": 4,
    "CARD_NETWORK_INTERAC": 5,
    "CARD_NETWORK_PRIVATE_LABEL": 6,
    "CARD_NETWORK_EFTPOS": 7,
    "CARD_NETWORK_MAESTRO": 8,
    "CARD_NETWORK_ID": 9,
    "CARD_NETWORK_QUICPAY": 10,
    "CARD_NETWORK_JCB": 11,
    "CARD_NETWORK_ELO": 12
}
```

## List of token statuses

```json
{
    "TOKEN_STATE_UNTOKENIZED": 1,
    "TOKEN_STATE_PENDING": 2,
    "TOKEN_STATE_NEEDS_IDENTITY_VERIFICATION": 3,
    "TOKEN_STATE_SUSPENDED": 4,
    "TOKEN_STATE_ACTIVE": 5,
    "TOKEN_STATE_FELICA_PENDING_PROVISIONING": 6
}
```

## List of error codes and meanings

```json
{
    "TAP_AND_PAY_NO_ACTIVE_WALLET": 15002,
    "TAP_AND_PAY_TOKEN_NOT_FOUND": 15003,
    "TAP_AND_PAY_INVALID_TOKEN_STATE": 15004,
    "TAP_AND_PAY_ATTESTATION_ERROR": 15005,
    "TAP_AND_PAY_UNAVAILABLE": 15009
}
```
