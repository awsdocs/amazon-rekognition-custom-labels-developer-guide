# Understanding training and testing validation result manifests<a name="tm-debugging-scope-json-line"></a>

During training, Amazon Rekognition Custom Labels creates validation result manifests to hold non\-terminal JSON Line errors\. The validation results manifests are copies of the training and testing datasets with error information added\. You can access the validation manifests after training completes\. For more information, see [Getting the validation results](tm-debugging-getting-validation-data.md)\. Amazon Rekognition Custom Labels also creates a manifest summary that includes overview information for JSON Line errors, such as error locations and JSON Line error counts\. For more information, see [Understanding the manifest summary](tm-debugging-summary.md)\.

**Note**  
Validation results \(Training and Testing Validation Result Manifests and Manifest Summary\) are only created if there are no [Terminal manifest file errors](tm-debugging.md#tm-error-category-terminal)\.

A manifest contains JSON Lines for each image in the dataset\. Within the validation results manifests, JSON Line error information is added to the JSON Lines where errors occur\.

A JSON Line error is a non\-terminal error related to a single image\. A non\-terminal validation error can invalidate the entire JSON Line or just a portion\. For example, if the image referenced in a JSON Line is not in PNG or JPG format, an [ERROR\_INVALID\_IMAGE](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_IMAGE) error occurs and the entire JSON Line is excluded from training\. Training continues with other valid JSON Lines\.

Within a JSON Line, an error might mean the JSON Line can stil be used for training\. For example, if the left value for one of four bounding boxes associated with a label is negative, the model is still trained using the other valid bounding boxes\. JSON Line error information is returned for the invalid bounding box \([ERROR\_INVALID\_BOUNDING\_BOX](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_BOUNDING_BOX)\)\. In this example, the error information is added to the `annotation` object where the error occurs\. 

Warning errors, such as [WARNING\_NO\_ANNOTATIONS](tm-debugging-json-line-errors.md#tm-warning-WARNING_NO_ANNOTATIONS), aren't used for training and count as ignored JSON lines \(`ignored_json_lines`\) in the manifest summary\. For more information, see [Understanding the manifest summary](tm-debugging-summary.md)\. Additionally, ignored JSON Lines don't count towards the 20% error threshold for training and testing\.

  For information about specific non\-terminal data validation errors, see [Non\-Terminal JSON Line Validation Errors](tm-debugging-json-line-errors.md)\. 

**Note**  
If there are too many data validation errors, training is stopped and a [ERROR\_TOO\_MANY\_INVALID\_ROWS\_IN\_MANIFEST](tm-debugging-aggregate-errors.md#tm-error-ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST) terminal error is reported in the manifest summary\.

For information about correcting JSON Line errors, see [Fixing training errors](tm-debugging-fixing-validation-errors.md)\. 



## JSON line error format<a name="tm-json-line-error-format"></a>

Amazon Rekognition Custom Labels adds non\-terminal validation error information to image level and object localization format JSON Lines\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.

### Image Level Errors<a name="tm-debugging-image-level"></a>

The following example shows the `Error` arrays in an image level JSON Line\. There are two sets of errors\. Errors related to label attribute metadata \(in this example, sport\-metadata\) and errors related to the image\. An error includes an error code \(code\), error message \(message\)\. For more information, see [Image\-Level labels in manifest files](md-create-manifest-file-classification.md)\. 

```
{
    "source-ref": String,
    "sport": Number,
    "sport-metadata": {
        "class-name": String,
        "confidence": Float,
        "type": String,
        "job-name": String,
        "human-annotated": String,
        "creation-date": String,
        "errors": [
            {
                "code": String, # error codes for label
                "message": String # Description and additional contextual details of the error
            }
        ] 
    },
    "errors": [
        {
            "code": String, # error codes for image
            "message": String # Description and additional contextual details of the error
        }
    ]
}
```

### Object localization errors<a name="tm-debugging-object-localization"></a>

The following example show the error arrays in an object localization JSON Line\. The JSON Line contains an `Errors` array information for fields in the following JSON Line sections\. Each `Error` object includes the error code and the error message\.
+ *label attribute* – Errors for the label attribute fields\. See `bounding-box` in the example\. 
+ *annotations* – Annotation errors \(bounding boxes\) are stored in the `annotations` array inside the label attribute\.
+ *label attribute\-metadata* – Errors for the label attribute metadata\. See `bounding-box-metadata` in the example\.
+ *image* – Errors not related to the label attribute, annotation, and label attribute metadata fields\. 

For more information, see [Object localization in manifest files](md-create-manifest-file-object-detection.md)\. 

```
{
    "source-ref": String,
    "bounding-box": {
        "image_size": [
            {
                "width": Int,
                "height": Int,
                "depth":Int,
            }
        ],
        "annotations": [
            {
                "class_id": Int,
                "left": Int,
                "top": Int,
                "width": Int,
                "height": Int,
                "errors": [   # annotation field errors
                    {
                        "code": String, # annotation field error code
                        "message": String # Description and additional contextual details of the error
                    }
                ]
            }
        ],
        "errors": [ #label attribute field errors
            {
                "code": String, # error code
                "message": String # Description and additional contextual details of the error
            }
        ] 
    },
    "bounding-box-metadata": {
        "objects": [
            {
                "confidence": Float
            }
        ],
        "class-map": {
            String: String
        }, 
        "type": String,
        "human-annotated": String,
        "creation-date": String,
        "job-name": String,
        "errors": [  #metadata field errors
            {
                "code": String, # error code
                "message": String # Description and additional contextual details of the error
            }
        ] 
    },
   "errors": [  # image errors
        {
            "code": String, # error code
            "message": String # Description and additional contextual details of the error
        }
    ] 
 }
```

## Example JSON line error<a name="tm-debugging-scope-json-line-example"></a>

The following object localization JSON Line \(formatted for readability\) shows an [ERROR\_BOUNDING\_BOX\_TOO\_SMALL](tm-debugging-json-line-errors.md#tm-error-ERROR_BOUNDING_BOX_TOO_SMALL) error\. In this example, the bounding box dimensions \(height and width\) aren't greater than 1 x 1\.

```
{
    "source-ref": "s3://bucket/Manifests/images/199940-1791.jpg",
    "bounding-box": {
        "image_size": [
            {
                "width": 3000,
                "height": 3000,
                "depth": 3
            }
        ],
        "annotations": [
            {
                "class_id": 1,
                "top": 0,
                "left": 0,
                "width": 1,
                "height": 1, 
                "errors": [
                    {
                        "code": "ERROR_BOUNDING_BOX_TOO_SMALL",
                        "message": "The height and width of the bounding box is too small."
                    }
                ]
            },
            {
                "class_id": 0,
                "top": 65,
                "left": 86,
                "width": 220,
                "height": 334
            }
        ]
    },
    "bounding-box-metadata": {
        "objects": [
            {
                "confidence": 1
            },
            {
                "confidence": 1
            }
        ],
        "class-map": {
            "0": "Echo",
            "1": "Echo Dot"
        },
        "type": "groundtruth/object-detection",
        "human-annotated": "yes",
        "creation-date": "2019-11-20T02:57:28.288286",
        "job-name": "my job"
    }
}
```