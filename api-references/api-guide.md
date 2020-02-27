# API Guide

{% api-method method="post" host="https://nvision.nipa.cloud" path="/api/v1/object-detection" %}
{% api-method-summary %}
POST object-detection
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get free cakes.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter required=true name="Authentication" type="string" %}
ApiKey: Authentication token to track down who is requesting predictions.
{% endapi-method-parameter %}

{% api-method-parameter name="Content-Type" type="string" required=true %}
Application/json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="raw\_data" type="string" required=true %}
Base64 encoded string of an image
{% endapi-method-parameter %}

{% api-method-parameter name="configurations" type="array" required=false %}
List of configuration params corresponding to the service.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Predictions successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{    
   "source_id" : "",
   "frame_id" : "",
   "service_id" : "",
   "detected_objects" : [
      {
         "bounding_box" : {
            "bottom" : 251,
            "top" : 167,
            "right" : 479,
            "left" : 361
         },
         "parent" : "electronic",
         "cropped_image" : "",
         "confidence" : 0.995199978351593,
         "name" : "tvmonitor"
      }
   ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
The request body is not conformed.
{% endapi-method-response-example-description %}

```text
{
    "message": "Bad request"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
The API key is not valid, not defined, or revoked.
{% endapi-method-response-example-description %}

```text
{
    "message": "This API key is invalid."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=429 %}
{% api-method-response-example-description %}
Request rate limit exceeded.
{% endapi-method-response-example-description %}

```text
{
    "message": "Too many request."
}ConfidenceThreshold: to define the minimum confidence score of the prediction results.
Value options: [0, 1]
Default: "0.1"
OutputCroppedImage: to return cropped images from bounding box detections.
Value options: "true" or "false"
Default: "false"
OutputVisualizedImage: to return drawn bounding box detections on raw image.
Value options: "true" or "false"
Default: "false"
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Internal server error.
{% endapi-method-response-example-description %}

```
{
    "message": "Something went wrong."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Example JSON Request Body

```javascript
{
    "raw_data": <<BASE64_ENCODED_IMAGE>>,
    "configurations": [
        {
            "parameter": "ConfidenceThreshold",
            "value": "0.1"
        },
        {
            "parameter": "OutputCroppedImage",
            "value": "false"
        },
        {
            "parameter": "OutputVisualizedImage",
            "value": "false"
        }
    ]
}
```

The configuration is different on individual service types. It is structured as a key-value mapping. A config name is defined in **`parameter`** field and the corresponding value is defined in **`value`** field in **string format**.

For object detection service, there are two available configurations as follows:

* **`ConfidenceThreshold`**: to define the minimum confidence score of the prediction results.
  * Value options: `[0, 1]`
  * Default: `"0.1"`
* **`OutputCroppedImage`**: to return cropped images from bounding box detections.
  * Value options: `"true"` or `"false"`
  * Default: `"false"`
* **`OutputVisualizedImage`**: to return drawn bounding box detections on raw image.
  * Value options: `"true"` or `"false"`
  * Default: `"false"`

