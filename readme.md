# Push Notification Demo
Client app demo for push notifications.

## Generate Keys
First to generate the public/private keys.

1. access https://web-push-codelab.glitch.me/ get public key.
2. replace the var `applicationServerPublicKey` in main.js with the public key

## Server This Project in HTTP Server
Using node package `live-server` or `web server for chrome`, then access it in browser.

then click the button, enable the notification.


## Get The Subscription

After allow the subscribtion, you should see the Subscription Object will printed in the page or console log.

## Send Notification

### Way 1(via web app)
via https://web-push-codelab.glitch.me/ below, paste the subscribtion like this

```json{"endpoint":"https://fcm.googleapis.com/fcm/send/cfA_9tbQmTc:APA91bHMKEZzt-lyJRSMUpR5o9MPR9EBIvZgqaIXTFjGWlsTnc1ZaMjcoocrzWCqGRLPjoqGfJriS3FATxzCeeNQvuuugEDCNmDYjVQ7r61puDogSvpOBUDXcBJYfAaS5c8mvsGYlZa6","expirationTime":null,"keys":{"p256dh":"BElYZkSwpVmrqEtFPwDdp2y-PqD0k79w3nQFmngDvKiyiOmwE9B8oc5J6ih3wPxF9nwWu-S60rGr4pbMS00Ix2w","auth":"ny6Ei7QkID-tIxKmcm4Wgg"}}
```

into the `Subscription to Send To` textArea.

then click `Text to Send`, 
the notification will be send to client

### Way 2(via node web-push package)
[web-push](https://github.com/web-push-libs/web-push) lib will help you send meassage in server.

1. Create `vapid.json` file

```bash
$ web-push generate-vapid-keys --json > vapid.json
```
output
```json
{
  "publicKey": "BHebiEracmMhIzGjJdDSiKlbldZfqznZWmCUL8qAZhdYvCwhEFCg1zd52H4IAI9cxbyeMjzr7VQ5EOergUsakJE",
  "privateKey": "QWyQwURmCMQpgmZQQ6xVKMBxpIXKFTZywz_8e73vPI0"
}
```

2. Create `push-server.js` file
then replace the  `endpoint` and `keys` value.
```js
// Web-push Module
const webpush = require('web-push');
const vapid = require('./vapid.json');

// Configure keys
webpush.setVapidDetails(
  'mailto:ray@stackacademy.tv',
  vapid.publicKey,
  vapid.privateKey
);

const pushSubscription = {
  endpoint: "https://fcm.googleapis.com/fcm/send/f8dKsZEPSlA:APA91bFXvGYKwG6Ey1aPuZsISV1cJrRzCPaGkYuf6QZf_jF-uPWJrS7a60hhKc0_O7-pFVUQtW8owl_9_ex9xWHZqZhJxwf7ciSsUas6qcHBooKyB8osXVT_dVmKihm2-K1xpsg-7mlJ",
  keys: {
    auth: "BaoLgoJHMGnrTFHGzaCnvg",
    p256dh: "BDh1-EPnFjeYqnORIls2NZCd2JpVbL1BLJ-ZaRnSf8H5_8_4dkXAKX72qrz2MMV0IYzYSWSnJA_GPCmGi6vZjQQ"
  }
};

webpush.sendNotification(pushSubscription, 'A notification from the push server')
  .then((res) => {
    console.log(res)
  })
  .catch((err) => {
    console.log(err)
  })
console.log('Push sent to client');
```

3. Send notification
```bash
$ node push-server.js
Push sent to client
{ statusCode: 201,
  body: '',
  headers:
   { 'content-type': 'text/plain',
     location: 'https://fcm.googleapis.com/fcm/0:1538364302856233%9743afb3f9fd7ecd',
     date: 'Mon, 01 Oct 2018 03:25:02 GMT',
     expires: 'Mon, 01 Oct 2018 03:25:02 GMT',
     'cache-control': 'private, max-age=0',
     'x-content-type-options': 'nosniff',
     'x-frame-options': 'SAMEORIGIN',
     'x-xss-protection': '1; mode=block',
     'content-length': '0',
     server: 'GSE',
     'alt-svc': 'quic=":443"; ma=2592000; v="44,43,39,35"',
     connection: 'close' } }
```
the notification will be send to client.

## Resources
- https://developers.google.com/web/fundamentals/codelabs/push-notifications
- https://web-push-codelab.glitch.me/
- https://github.com/web-push-libs