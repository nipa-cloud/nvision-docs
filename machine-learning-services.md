# Machine Learning Services

The followings are services provided by Nvision service that enables machine learning services to extract insightful information from images with simple and easy to use APIs.

### **Object Detection**

Localize and identify multiple objects in the image such as people, animals, vehicles, furniture, etc.

![](.gitbook/assets/street.png)

```text
{
    "detected_objects": [
        {
            "confidence": 0.8327999711036682,
            "parent": "accessory",
            "bounding_box": {
                "left": 246,
                "right": 303,
                "top": 520,
                "bottom": 605
            },
            "name": "backpack"
        },
        ...
    ]
}
```

Object detection is a pre-trained machine learning service that deals with detecting instances of semantic objects of 80 common classes from the [COCO dataset](http://cocodataset.org/). The service has applications in many areas of computer vision, including image search, and video surveillance analytics.

### **Image Tagging & Categorization**

Suggest top relevant tags to the image content from over a thousand tags in the database and classify the image into specified categories.

### **Object Tracking**

Track the movement of objects in video sequences. For example, it can be used to analyze walking directions of customers in the store.

### **Face Detection**

Detect and count the number of faces in the image

### **Facial Analysis**

Identify facial landmarks such as eyes, nose and mouth, estimate angles of the face, and analyze emotions or facial expressions such as smiling, sad, or surprised.

### **Demographic Recognition**

Identify demographic information such as nationality, gender, and age range from the face.

### **Facial Recognition**

Recognize faces and identify multiple people at the same time. Or opt to only extract the embedding features of the face for later comparison without having to collect customer information for privacy protection.

### **Face Comparison**

Find people in videos or a set of pictures and Compare the similarities between faces.

