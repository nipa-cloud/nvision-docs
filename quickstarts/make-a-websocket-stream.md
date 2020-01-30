# Make a WebSocket Stream

WebSocket stream is another way to submit images to the Nvision service. To use the WebSocket stream, the easiest way is to use our provided SDK, see [JavaScript SDK](../api-references/javascript-sdk.md).

{% hint style="info" %}
Consider using the WebSocket stream if you need to continuously submit images \(i.e., frames of the video stream\).
{% endhint %}

## Getting results from the WebSocket stream using Webhook

Some application need to be run as a headless agent, for example: your application is a headless agent silently run in a RaspberryPi to receive the CCTV video stream and submit video frames to Nvision recognition service.

To get results, you need to configure a **Webhook** endpoint to your service**.** Webhook is the user-defined HTTP callback endpoint. When the image is processed, the recognition service will make a HTTP request to the specified endpoint with the recognition result.

You need to provide a conformed HTTP endpoint to receive the data as follow.

* **Method**: POST
* **Body**: The request body will be provided as

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

