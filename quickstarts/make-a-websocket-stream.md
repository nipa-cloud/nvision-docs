# Make a WebSocket Stream

WebSocket stream is another way to submit images to the Nvision service. To use the WebSocket stream, the easiest way is to use our provided SDK, see [JavaScript SDK](../api-references/javascript-sdk.md).

{% hint style="info" %}
Consider using the WebSocket stream if you need to continuously submit images \(i.e., frames of the video stream\).
{% endhint %}

**To use Nvision with streaming videos,** your application needs to implement the following:

* [Stream video frames to Nvision service](make-a-websocket-stream.md#stream-video-frames-to-nvision-service)**:** Develop websocket stream using SDK.
* [Get results from the WebSocket via WebHook](make-a-websocket-stream.md#get-results-from-the-websocket-via-webhook): Create a webhook url to receive HTTP callback.
* [Set up callback endpoints](make-a-websocket-stream.md#set-up-callback-endpoints): in the Nvision service page.

### Stream video frames to Nvision service

{% hint style="info" %}
In this quickstart, we present an end-to-end use case of how to stream video frames from edge services and set up your backend service to receive the results.
{% endhint %}

Some application need to be run as a **Headless** agent on edge compute. For example, you develop a headless agent running on a RaspberryPi to read video frames from CCTV and submit them to the Nvision service as the following diagram.

![](../.gitbook/assets/nvision-pic-nn2.png)

#### Installation

Initialize your NPM project, then install `@nipacloud/nvision` and `opencv4nodejs` using `npm` or `yarn` command.

```bash
yarn init
yarn add @nipacloud/nvision opencv4nodejs
```

**Example Headless Agent**

{% code title="How to run: $ node index.js" %}
```javascript
const OpenCV = require("opencv4nodejs");
const nvision = require("@nipacloud/nvision");

const objectdetectionStreamClient = nvision.objectDetection({
    streamingKey: "<YOUR_STREAMING_KEY_GOES_HERE>"
}).stream();

objectdetectionStreamClient.connect().then(() => {
    const cvCam = new OpenCV.VideoCapture(0);
    setInterval(() => {
        const cvCamFrameMat = cvCam.read();
        const jpgEncoded = OpenCV.imencode(".jpg", cvCamFrameMat);
        objectdetectionStreamClient.predict(jpgEncoded);
    }, 200);
});
```
{% endcode %}

Receive results back to the agent if the webhook url is not configured.

```javascript
objectDetectionStreamClient.on("message", (result) => {
    console.log(result);
});
```

### Get results from the WebSocket via **WebHook**

To get results, you need to configure a **Webhook** endpoint to your service**.** Webhook is a user-defined HTTP callback endpoint. 

#### Installation

```bash
yarn init
yarn add koa koa-bodyparser
```

**Example Headless Agent**

{% code title="How to run: $ node webhook.js" %}
```javascript
const Koa = require("koa");
const bodyparser = require("koa-bodyparser");
const koa = new Koa();

koa.use(bodyparser());
koa.use((ctx) => { 
  console.log(ctx.request.body);
  ctx.status = 204;
});

koa.listen(3000);
```
{% endcode %}

When the image is processed, the Nvision service will make a HTTP request to the specified endpoint with the prediction results structured as follow.

* **Method**: POST
* **Body**: The request body will be provided as

{% code title="Example response from object detection service" %}
```javascript
{
    "detected_objects": [
        {
            "confidence": number,
            "parent": string,
            "bounding_box": {
                "left": number,
                "right": number,
                "top": number,
                "bottom": number
            },
            "name": string
        }
    ]
}
```
{% endcode %}

### Set up callback endpoints

As the socket protocol is used, we provide a custom **callback endpoint** configuration that allows you to have independent backends for receiving and analyzing prediction results.

**WebSocket Streaming Callback URL**

```css
e.g. https://YOUR_HOST:PORT/
```

![](../.gitbook/assets/screenshot-from-2020-01-31-10-39-44.png)

