# JavaScript SDK

JavaScript SDK is based on the Nvision image processing service as `Promise` based function call and WebSocket client with TypeScript definition provided.

## Quickstarts

1. [API Concepts: RESTful and WebSocket](https://nvision-docs.nipa.cloud/api-concepts/restful-calls)
2. [Set up the Nvision Service](https://nvision-docs.nipa.cloud/quickstarts/set-up-the-nvision-service)
3. [Make a Websocket Stream](../quickstarts/make-a-websocket-stream.md)
4. [Detect Objects](https://nvision-docs.nipa.cloud/how-to-guides/detect-objects)

Before you begin to use this SDK, these quickstarts will guide you to get started with the Nvision API.

## Installation

Install `@nipacloud/nvision` via NPM or Yarn to your project.

```bash
yarn add @nipacloud/nvision
```

### Importing the SDK

Import the SDK module to your source file

```javascript
const nvision = require("@nipacloud/nvision")
// or use the ES6 import
import nvision from "@nipacloud/nvision"
```

### Additional setup for front-end app

If you use the SDK in the front-end application, you need to import the browser variant provided at `@nipacloud/nvision/dist/browser/nvision.js`

```javascript
const nvision = require("@nipacloud/nvision/dist/browser/nvision")
// or use the ES6 import
import nvision from "@nipacloud/nvision/dist/browser/nvision"
```

If you use the SDK in the webpack-based project, you can provide the module resolution alias in your webpack configuration.

```javascript
module.exports = {
  ...,
  "resolve": {
    "alias": {
      "@nipacloud/nvision": "@nipacloud/nvision/dist/browser/nvision.js"
    }
  }
}
```

If you correctly setup the module resolution, you can import module using a typical module name `@nipacloud/nvision` without path.

## Using the object detection service

To use the object detection service, you need to create the service object using `objectDetection()` generator function

```javascript
import nvision from "@nipacloud/nvision";

const objectDetectionService = nvision.objectDetection({
    apiKey: "<YOUR_API_KEY_GOES_HERE>",
    streamingKey: "<YOUR_STREAMING_KEY_GOES_HERE>"
});
```

You do not have to provide both `apiKey` or `streamingKey` . If you use only API call, you can provide only `apiKey` , this applied to WebSocket streaming too.

### Making an API call

You can make an API call using `predict()` function of the service object

{% code title="Function signature" %}
```typescript
predict (
    base64EncodedImage: string,
    cropOutputImage?: boolean
): Promise<{
    service_id: string,
    detected_object: {
        bounding_box: {
            bottom: number,
            left: number,
            right: number,
            top: number
        },
        name: string,
        confidence: number,
        cropped_image: string
    }
}>
```
{% endcode %}

#### Parameters

| Parameter | Description |
| :--- | :--- |
| base64EncodedImage | Base64 string encoded JPEG/PNG image |
| cropOutputImage \(Optional\) | Set "CropOutputImage" parameter, if true, API response will include the cropped images for each bounding box in Base64 encoded JPEG/PNG  format. Default is false. |

#### Promised return object

| Field |  |
| :--- | :--- |
| service\_id | Service ID associated to the API key used |
| detected\_object | List of the detected object |
| name | Classified name of the object |
| confidence | Confidence value of the prediction |
| cropped\_image | Bounding box cropped image |

#### Example

```javascript
import nvision from "@nipacloud/nvision";

const objectDetectionService = nvision.objectDetection({
    apiKey: "<YOUR_API_KEY_GOES_HERE>"
});

objectDetectionService.predict(
    "BASE64_ENCODED_IMAGE"
).then((result) => {
    // Outout the result object to console
    console.log(result);
});
```

### Streaming video frames through WebSocket

You can make a WebSocket connection using `stream()` generator function to get the streaming client object.

```typescript
stream (): {
    on: (event: string, listener: (eventArgs: any) => unknown) => void;
    once: (event: string, listener: (eventArgs: any) => unknown) => void;
    connect: () => Promise<void>;
    predict: (
        buffer: ArrayBuffer, 
        cropOutputImage?: boolean, 
        sourceId?: string, 
        frameId?: string
    ) => Promise<void>;
};
```

#### Returned object

| Field | Description |
| :--- | :--- |
| on\(\) | Add an event subscriber, Possible event are "connect", "message", "error" |
| once\(\) | Add a one-time event subscriber, Possible event are "connect", "message", "error" |
| connect\(\) | Establish the WebSocket connection |
| predict\(\) | Send the data through the websocket |

#### Example

This example uses `opencv4nodejs` to capture the webcam image, then submit it through the WebSocket channel.

```javascript
import * as OpenCV from "opencv4nodejs";
import nvision from "@nipacloud/nvision";

const objectdetectionStreamClient = nvision.objectDetection({
    streamingKey: "<YOUR_STREAMING_KEY_GOES_HERE>"
}).stream();

objectDetectionStreamClient.on("message", (result) => {
    console.log(result);
});

objectdetectionStreamClient.connect().then(() => {
    const cvCam = new OpenCV.VideoCapture(0);
    setInterval(() => {
        const cvCamFrameMat = cvCam.read();
        const jpgEncoded = OpenCV.imencode(".jpg", cvCamFrameMat);
        objectdetectionStreamClient.predict(jpgEncoded);
    }, 200);
});
```

