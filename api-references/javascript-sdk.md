# API Guide

{% api-method method="post" host="https://nvision.cakes.com" path="/api/object-detetion" %}
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

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find the service endpoint.
{% endapi-method-response-example-description %}

```javascript
{
    "message": "Not found"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

#### Example JSON Request Body

```javascript
{
    "raw_data": <<BASE64_ENCODED_IMAGE>>,
    "configurations": [
        {
            "parameter": "OutputCroppedImage",
            "value": "false"
        },
        {
            "parameter": "ConfidenceThreshold",
            "value": "0.1"
        }
    ]
}
```

The configuration is different on individual service types. It is structured as a key-value mapping. A config name is defined in **`parameter`** field and the corresponding value is defined in **`value`** field in **string format**.  
  
For object detection service, there are two available configurations as follows:

* **`OutputCroppedImage`**: to return cropped images from bounding box detections.
  * Value options: `"true"` or `"false"`
  * Default: `"false"`
* **`ConfidenceThreshold`**: to define the minimum confidence score of the prediction results.
  * Value options: `[0, 1]`
  * Default: `"0.1"`

