# Python SDK

Python SDK is based on the Nvision image processing service for **synchronous calls**. The input requests and output responses are structured in JSON format. You can make a **RESTful API call** by sending an image as a **base64 encoded** **string** in the body of your request, see [make API calls](https://nvision-docs.nipa.cloud/quickstarts/make-a-restful-call) quickstart.

### Quickstarts

Before you begin to use this SDK, these quickstarts will guide you to get started with the Nvision API.

1. [API Concepts: RESTful and WebSocket](https://nvision-docs.nipa.cloud/api-concepts/restful-calls)
2. [Set up the Nvision Service](https://nvision-docs.nipa.cloud/quickstarts/set-up-the-nvision-service)
3. [Make a RESTful Call](https://nvision-docs.nipa.cloud/quickstarts/make-a-restful-call)
4. [Detect Objects](https://nvision-docs.nipa.cloud/how-to-guides/detect-objects)

### Installation

Using PyPi package here: [https://pypi.org/project/nvision/](https://pypi.org/project/nvision/)

```bash
pip install nvision
```

Supported Python versions: Python &gt;= 3.5

### Using the SDK

#### Detect Objects

To use object detection service, you can initialize the **ObjectDetection** class directly with your api\_key. Then, call the **predict\(\)** method to make a RESTful request for model inference.

```python
import os
import json
import base64
from nvision import ObjectDetection

model = ObjectDetection(api_key=os.getenv('API_KEY'))

# base64 encoed string
with open(os.path.join('docs', 'street.jpg'), 'rb') as file:
    image = file.read()
    image = base64.b64encode(image).decode('utf-8')

# make a RESTful call to the Nvision API
response = model.predict(image)

print(response)
print(json.dumps(response.json(), indent=4, sort_keys=True))
```

#### **nvision.ObjectDetection.predict\(\) params:**

* **`image`**: [base64](https://en.wikipedia.org/wiki/Base64) encoded string that represents binary data in an ASCII **string** format.
  *  e.g. `/9j/4AAQSkZJRgABAQEBLAEsAAD...`
* **`ConfidenceThreshold`**: to define the minimum confidence score of the prediction results.
  * Value options: `[0, 1]`
  * Default: `"0.1"`
* **`OutputCroppedImage`**: to return cropped images from bounding box detections.
  * Value options: `"true"` or `"false"`
  * Default: `"false"`
* **`OutputVisualizedImage`**: to return drawn bounding box detections on raw image.
  * Value options: `"true"` or `"false"`
  * Default: `"false"`

```python
def predict(self,
            image,
            confidence_threshold=0.1,
            output_cropped_image=False,
            output_visualized_image=False):

docstring: Make a RESTful request for model inference 
    :param image: base64 encoded string
    :param confidence_threshold: float
        - value options: [0,1]
        - default: 0.1
    :param output_cropped_image: Boolean
        - value options: True or False,
        - default: False
    :param output_visualized_image: Boolean
        - value options: True or False,
        - default: False
```

#### **Prediction response:**

* **detected\_objects**: a list of prediction outputs for corresponding objects/labels/boxes
  * `bounding_box`: integer
  * `confidence`: how likely it is, the object is contained within the image or video.
  * `cropped_image`: returned if `output_cropped_image` is set to `True`.
  * `name` : output object name ot label category.
  * `parent`: parent class or the output label.

```javascript
{
    "detected_objects": [
        {
            "bounding_box": {
                "left": 245,
                "right": 307,
                "top": 515,
                "bottom": 602
            },
            "confidence": 0.9126193721036421,
            "cropped_image": <<base64_encoded_string>>,
            "name": "pserson"
            "parent": "human"
        },
        ...
    ]
}
```

