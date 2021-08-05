# Non\-Terminal JSON Line Validation Errors<a name="tm-debugging-json-line-errors"></a>

This topic lists the non\-terminal JSON Line validation errors reported by Amazon Rekognition Custom Labels during training\. The errors are reported in the training and testing validation manifest\. For more information, see [Understanding training and testing validation result manifests](tm-debugging-scope-json-line.md)\. You can fix a non\-terminal JSON Line error by updating the JSON Line in the training or test manifest file\. You can also remove the JSON Line from the manifest, but doing so might reduce the quality of your model\. If there are many non\-terminal validation errors, you might find it easier to recreate the manifest file\. Validation errors typically occur in manually created manifest files\. For more information, see [Creating a manifest file](cd-manifest-files.md)\. For information about fixing validation errors, see [Fixing training errors](tm-debugging-fixing-validation-errors.md)\. Some errors can be fixed by using the Amazon Rekognition Custom Labels console\. 

## ERROR\_MISSING\_SOURCE\_REF<a name="tm-error-ERROR_MISSING_SOURCE_REF"></a>

### Error message<a name="tm-error-message-ERROR_MISSING_SOURCE_REF"></a>

The source\-ref key is missing\.

### More information<a name="tm-error-description-ERROR_MISSING_SOURCE_REF"></a>

The JSON Line `source-ref` field provides the Amazon S3 location of an image\. This error occurs when the `source-ref` key is missing or is misspelt\. This error typically occurs in manually created manifest files\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.

**To fix `ERROR_MISSING_SOURCE_REF`**

1. Check that the `source-ref` key is present and is spelt correctly\. A complete `source-ref` key and value is similar to the following\. is `"source-ref": "s3://bucket/path/image"`\. 

1. Update or the `source-ref` key in the JSON Line\. Alternatively, remove, the JSON Line from the manifest file\. 

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_INVALID\_SOURCE\_REF\_FORMAT<a name="tm-error-ERROR_INVALID_SOURCE_REF_FORMAT"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_SOURCE_REF_FORMAT"></a>

The format of the source\-ref value is invalid\. 

### More information<a name="tm-error-description-ERROR_INVALID_SOURCE_REF_FORMAT"></a>

The `source-ref` key is present in the JSON Line, but the schema of the Amazon S3 path is incorrect\. For example, the path is `https://....` instead of `S3://....`\. An ERROR\_INVALID\_SOURCE\_REF\_FORMAT error typically occurs in manually created manifest files\. For more information, see [Creating a manifest file](cd-manifest-files.md)\. 

**To fix `ERROR_INVALID_SOURCE_REF_FORMAT`**

1. Check that the schema is `"source-ref": "s3://bucket/path/image"`\. For example, `"source-ref": "s3://custom-labels-console-us-east-1-1111111111/images/000000242287.jpg"`\. 

1. Update, or remove, the JSON Line in the manifest file\. 

 You can't use the Amazon Rekognition Custom Labels console to fix this `ERROR_INVALID_SOURCE_REF_FORMAT`\.

## ERROR\_NO\_LABEL\_ATTRIBUTES<a name="tm-error-ERROR_NO_LABEL_ATTRIBUTES"></a>

### Error message<a name="tm-error-message-ERROR_NO_LABEL_ATTRIBUTES"></a>

No label attributes found\.

### More information<a name="tm-error-description-ERROR_NO_LABEL_ATTRIBUTES"></a>

The label attribute or the label attribute `-metadata` key name \(or both\) is invalid or missing\. In the following example, `ERROR_NO_LABEL_ATTRIBUTES` occurs whenever the `bounding-box` or `bounding-box-metadata` key \(or both\) is missing\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.

```
{
	"source-ref": "s3://custom-labels-bucket/images/IMG_1186.png",
	"bounding-box": {
		"image_size": [{
			"width": 640,
			"height": 480,
			"depth": 3
		}],
		"annotations": [{
			"class_id": 1,
			"top": 251,
			"left": 399,
			"width": 155,
			"height": 101
		}, {
			"class_id": 0,
			"top": 65,
			"left": 86,
			"width": 220,
			"height": 334
		}]
	},
	"bounding-box-metadata": {
		"objects": [{
			"confidence": 1
		}, {
			"confidence": 1
		}],
		"class-map": {
			"0": "Echo",
			"1": "Echo Dot"
		},
		"type": "groundtruth/object-detection",
		"human-annotated": "yes",
		"creation-date": "2018-10-18T22:18:13.527256",
		"job-name": "my job"
	}
}
```

 A `ERROR_NO_LABEL_ATTRIBUTES` error typically occurs in a manually created manifest file\. For more information, see [Creating a manifest file](cd-manifest-files.md)\. 

**To fix `ERROR_NO_LABEL_ATTRIBUTES`**

1. Check that label attribute identifier and label attribute identifer `-metadata` keys are present and that the key names are spelt correctly\. 

1. Update, or remove, the JSON Line in the manifest file\.

You can't use the Amazon Rekognition Custom Labels console to fix `ERROR_NO_LABEL_ATTRIBUTES` \.

## ERROR\_INVALID\_LABEL\_ATTRIBUTE\_FORMAT<a name="tm-error-ERROR_INVALID_LABEL_ATTRIBUTE_FORMAT"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_LABEL_ATTRIBUTE_FORMAT"></a>

The format of the label attribute \{\} is invalid\.

### More information<a name="tm-error-description-ERROR_INVALID_LABEL_ATTRIBUTE_FORMAT"></a>

The schema for the label attribute key is missing or invalid\. An ERROR\_INVALID\_LABEL\_ATTRIBUTE\_FORMAT error typically occurs in manually created manifest files\. for more information, see [Creating a manifest file](cd-manifest-files.md)\. 

**To fix `ERROR_INVALID_LABEL_ATTRIBUTE_FORMAT`**

1. Check that the JSON Line section for the label attribute key is correct\. In the following example object location example, the `image_size` and `annotations` objects must be correct\. The label attribute key is named `bounding-box`\.

   ```
   	"bounding-box": {
   		"image_size": [{
   			"width": 640,
   			"height": 480,
   			"depth": 3
   		}],
   		"annotations": [{
   			"class_id": 1,
   			"top": 251,
   			"left": 399,
   			"width": 155,
   			"height": 101
   		}, {
   			"class_id": 0,
   			"top": 65,
   			"left": 86,
   			"width": 220,
   			"height": 334
   		}]
   	},
   ```

   

1. Update, or remove, the JSON Line in the manifest file\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_INVALID\_LABEL\_ATTRIBUTE\_METADATA\_FORMAT<a name="tm-error-ERROR_INVALID_LABEL_ATTRIBUTE_METADATA_FORMAT"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_LABEL_ATTRIBUTE_METADATA_FORMAT"></a>

The format of the label attribute metadata is invalid\.

### More information<a name="tm-error-description-ERROR_INVALID_LABEL_ATTRIBUTE_METADATA_FORMAT"></a>

The schema for the label attribute metadata key is missing or invalid\. An ERROR\_INVALID\_LABEL\_ATTRIBUTE\_METADATA\_FORMAT error typically occurs in manually created manifest files\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.

**To fix `ERROR_INVALID_LABEL_ATTRIBUTE_FORMAT`**

1. Check that the JSON Line schema for the label attribute metadata key is similar to the following example\. The label attribute metadata key is named `bounding-box-metadata`\.

   ```
   	"bounding-box-metadata": {
   		"objects": [{
   			"confidence": 1
   		}, {
   			"confidence": 1
   		}],
   		"class-map": {
   			"0": "Echo",
   			"1": "Echo Dot"
   		},
   		"type": "groundtruth/object-detection",
   		"human-annotated": "yes",
   		"creation-date": "2018-10-18T22:18:13.527256",
   		"job-name": "my job"
   	}
   ```

   

1. Update, or remove, the JSON Line in the manifest file\.



You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_NO\_VALID\_LABEL\_ATTRIBUTES<a name="tm-error-ERROR_NO_VALID_LABEL_ATTRIBUTES"></a>

### Error message<a name="tm-error-message-ERROR_NO_VALID_LABEL_ATTRIBUTES"></a>

No valid label attributes found\.

### More information<a name="tm-error-description-ERROR_NO_VALID_LABEL_ATTRIBUTES"></a>

No valid label attributes were found in the JSON Line\. Amazon Rekognition Custom Labels checks both the label attribute and the label attribute identifier\. An ERROR\_INVALID\_LABEL\_ATTRIBUTE\_FORMAT error typically occurs in manually created manifest files\. for more information, see [Creating a manifest file](cd-manifest-files.md)\. 

If a JSON Line isn't in a supported SageMaker manifest format, Amazon Rekognition Custom Labels marks the JSON Line as invalid and an `ERROR_NO_VALID_LABEL_ATTRIBUTES` error is reported\. Currently, Amazon Rekognition Custom Labels supports classification job and bounding box formats\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.

**To fix `ERROR_NO_VALID_LABEL_ATTRIBUTES`**

1. Check that the JSON for the label attribute key and label attribute metadata is correct\.

1. Update, or remove, the JSON Line in the manifest file\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_MISSING\_BOUNDING\_BOX\_CONFIDENCE<a name="tm-error-ERROR_MISSING_BOUNDING_BOX_CONFIDENCE"></a>

### Error message<a name="tm-error-message-ERROR_MISSING_BOUNDING_BOX_CONFIDENCE"></a>

One or more bounding boxes has a missing confidence value\.

### More information<a name="tm-error-description-ERROR_MISSING_BOUNDING_BOX_CONFIDENCE"></a>

The confidence key is missing for one or more object location bounding boxes\. The confidence key for a bounding box is in the label attribute metadata, as shown in the following example\. A ERROR\_MISSING\_BOUNDING\_BOX\_CONFIDENCE error typically occurs in manually created manifest files\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\.

```
	"bounding-box-metadata": {
		"objects": [{
			"confidence": 1
		}, {
			"confidence": 1
		}],
```

**To fix `ERROR_MISSING_BOUNDING_BOX_CONFIDENCE`**

1. Check that the `objects` array in the label attribute contains the same number of confidence keys as there are objects in the label attribute `annotations` array\.

1. Update, or remove, the JSON Line in the manifest file\.



You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_MISSING\_CLASS\_MAP\_ID<a name="tm-error-ERROR_MISSING_CLASS_MAP_ID"></a>

### Error message<a name="tm-error-message-ERROR_MISSING_CLASS_MAP_ID"></a>

One of more class ids is missing from the class map\.

### More information<a name="tm-error-description-ERROR_MISSING_CLASS_MAP_ID"></a>

The `class_id` in an annotation \(bounding box\) object doesn't have a matching entry in the label attribute metadata class map \(`class-map`\)\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\. A ERROR\_MISSING\_CLASS\_MAP\_ID error typically occurs in manually created manifest files\.

**To fix ERROR\_MISSING\_CLASS\_MAP\_ID**

1. Check that the `class_id` value in each annotation \(bounding box\) object has a corresponding value in the `class-map` array, as shown in the following example\. The `annotations` array and `class_map` array should have the same number of elements\.

   ```
   {
   	"source-ref": "s3://custom-labels-bucket/images/IMG_1186.png",
   	"bounding-box": {
   		"image_size": [{
   			"width": 640,
   			"height": 480,
   			"depth": 3
   		}],
   		"annotations": [{
   			"class_id": 1, 
   			"top": 251,
   			"left": 399,
   			"width": 155,
   			"height": 101
   		}, {
   			"class_id": 0,
   			"top": 65,
   			"left": 86,
   			"width": 220,
   			"height": 334
   		}]
   	},
   	"bounding-box-metadata": {
   		"objects": [{
   			"confidence": 1
   		}, {
   			"confidence": 1
   		}],
   		"class-map": {
   			"0": "Echo",
   			"1": "Echo Dot"
   		}, 
   		"type": "groundtruth/object-detection",
   		"human-annotated": "yes",
   		"creation-date": "2018-10-18T22:18:13.527256",
   		"job-name": "my job"
   	}
   }
   ```

1. Update, or remove, the JSON Line in the manifest file\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_INVALID\_JSON\_LINE<a name="tm-error-ERROR_INVALID_JSON_LINE"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_JSON_LINE"></a>

The JSON Line has an invalid format\.

### More information<a name="tm-error-description-ERROR_INVALID_JSON_LINE"></a>

An unexpected character was found in the JSON Line\. The JSON Line is replaced with a new JSON Line that contains only the error information\. An ERROR\_INVALID\_JSON\_LINE error typically occurs in manually created manifest files\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\. 

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

**To fix `ERROR_INVALID_JSON_LINE`**

1. Open the manifest file and navigate to the JSON Line where the ERROR\_INVALID\_JSON\_LINE error occurs\.

1. Check that the JSON Line doesn't contain invalid characters and that required `;` or `,` characters are not missing\.

1. Update, or remove, the JSON Line in the manifest file\.

## ERROR\_INVALID\_IMAGE<a name="tm-error-ERROR_INVALID_IMAGE"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_IMAGE"></a>

The image is invalid\. Check S3 path and/or image properties\.

### More information<a name="tm-error-description-ERROR_INVALID_IMAGE"></a>

The file referenced by `source-ref` is not a valid image\. Potential causes include the image aspect ratio, the size of the image, and the image format\.

For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\.

**To fix `ERROR_INVALID_IMAGE`**

1. Check the following\.
   + The aspect ratio of the image is less than 20:1\.
   + The size of the image is greater than 15 MB
   + The image is in PNG or JPEG format\. 
   + The path to the image in `source-ref` is correct\.
   + The minimum image dimension of the image is greater 64 pixels x 64 pixels\.
   + The maximum image dimension of the image is less than 4096 pixels x 4096 pixels\.

1. Update, or remove, the JSON Line in the manifest file\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_INVALID\_BOUNDING\_BOX<a name="tm-error-ERROR_INVALID_BOUNDING_BOX"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_BOUNDING_BOX"></a>

The bounding box has off frame values\.

### More information<a name="tm-error-description-ERROR_INVALID_BOUNDING_BOX"></a>

The bounding box information specifies an image that is either off the image frame or contains negative values\.

For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\.

**To fix `ERROR_INVALID_BOUNDING_BOX`**

1. Check the values of the bounding boxes in the `annotations` array\. 

   ```
   	"bounding-box": {
   		"image_size": [{
   			"width": 640,
   			"height": 480,
   			"depth": 3
   		}],
   		"annotations": [{
   			"class_id": 1,
   			"top": 251,
   			"left": 399,
   			"width": 155,
   			"height": 101
   		}]
   	},
   ```

1. Update, or alternatively remove, the JSON Line from the manifest file\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_NO\_VALID\_ANNOTATIONS<a name="tm-error-ERROR_NO_VALID_ANNOTATIONS"></a>

### Error message<a name="tm-error-message-ERROR_NO_VALID_ANNOTATIONS"></a>

No valid annotations found\.

### More information<a name="tm-error-description-ERROR_NO_VALID_ANNOTATIONS"></a>

None of the annotation objects in the JSON Line contain valid bounding box information\. 

**To fix `ERROR_NO_VALID_ANNOTATIONS`**

1. Update the `annotations` array to include valid bounding box objects\. Also, check that corresponding bounding box information \(`confidence` and `class_map`\) in the label attribute metadata is correct\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\.

   ```
   {
   	"source-ref": "s3://custom-labels-bucket/images/IMG_1186.png",
   	"bounding-box": {
   		"image_size": [{
   			"width": 640,
   			"height": 480,
   			"depth": 3
   		}],
   		"annotations": [
   		   {              
   			"class_id": 1,    #annotation object
   			"top": 251,
   			"left": 399,
   			"width": 155,
   			"height": 101
   		}, {
   			"class_id": 0,
   			"top": 65,
   			"left": 86,
   			"width": 220,
   			"height": 334
   		}]
   	},
   	"bounding-box-metadata": {
   		"objects": [
   		>{                
   			"confidence": 1          #confidence  object
   		}, 
           {
   			"confidence": 1
   		}],
   		"class-map": {  
   			"0": "Echo",    #label 
   			"1": "Echo Dot"
   		},
   		"type": "groundtruth/object-detection",
   		"human-annotated": "yes",
   		"creation-date": "2018-10-18T22:18:13.527256",
   		"job-name": "my job"
   	}
   }
   ```

1. Update, or alternatively remove, the JSON Line from the manifest file\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_BOUNDING\_BOX\_TOO\_SMALL<a name="tm-error-ERROR_BOUNDING_BOX_TOO_SMALL"></a>

### Error message<a name="tm-error-message-ERROR_BOUNDING_BOX_TOO_SMALL"></a>

The height and width of the bounding box is too small\.

### More information<a name="tm-error-description-ERROR_BOUNDING_BOX_TOO_SMALL"></a>

The bounding box dimensions \(height and width\) have to be greater than 1 x 1 pixels\.

During training, Amazon Rekognition Custom Labels resizes an image if any of its dimensions are greater than 1280 pixels \(the source images aren't affected\)\. The resulting bounding box heights and widths must be greater than 1 x 1 pixels\. A bounding box location is stored in the `annotations` array of an object location JSON Line\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md) 

```
	"bounding-box": {
		"image_size": [{
			"width": 640,
			"height": 480,
			"depth": 3
		}],
		"annotations": [{
			"class_id": 1,
			"top": 251,
			"left": 399,
			"width": 155,
			"height": 101
		}]
	},
```

The error information is added to the annotation object\.

**To fix ERROR\_BOUNDING\_BOX\_TOO\_SMALL**
+ Choose one of the following options\.
  + Increase the size of bounding boxes that are too small\.
  + Remove bounding boxes that are too small\. For information about removing a bounding box, see [ERROR\_TOO\_MANY\_BOUNDING\_BOXES](#tm-error-ERROR_TOO_MANY_BOUNDING_BOXES)\.
  + Remove the image \(JSON Line\) from the manifest\.





## ERROR\_TOO\_MANY\_BOUNDING\_BOXES<a name="tm-error-ERROR_TOO_MANY_BOUNDING_BOXES"></a>

### Error message<a name="tm-error-message-ERROR_TOO_MANY_BOUNDING_BOXES"></a>

There are more bounding boxes than the allowed maximum\.

### More information<a name="tm-error-description-ERROR_TOO_MANY_BOUNDING_BOXES"></a>

There are more bounding boxes than the allowed limit \(50\)\. You can remove excess bounding boxes in the Amazon Rekognition Custom Labels console, or you can remove them from the JSON Line\.

**To fix `ERROR_TOO_MANY_BOUNDING_BOXES` \(Console\)\.**

1. Decide which bounding boxes to remove\. 

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose the project that contains the dataset that you want to use\.

1. In the **Datasets** section, choose the dataset that you want to use\.

1. In the dataset gallery page, choose **Start labeling** to enter labeling mode\.

1. Choose the image that you want to remove bounding boxes from\.

1. Choose **Draw bounding box**\. 

1. In the drawing tool, choose the bounding box that you want to delete\.

1. Press the delete key on your keyboard to delete the bounding box\.

1. Repeat the previous 2 steps until you have deleted enough bounding boxes\.

1. Choose **Done**

1. Choose **Save changes** to save your changes\. 

1. Choose **Exit** to exit labeling mode\.



**To fix ERROR\_TOO\_MANY\_BOUNDING\_BOXES \(JSON Line\)\.**

1. Open the manifest file and navigate to the JSON Line where the ERROR\_TOO\_MANY\_BOUNDING\_BOXES error occurs\.

1. Remove the following for each bounding box that you want to remove\. 
   + Remove the required `annotation` object from `annotations` array\.
   + Remove the corresponding `confidence` object from the `objects` array in the label attribute metadata\.
   + If no longer used by other bounding boxes, remove the label from the `class-map`\.

   Use the following example to identify which items to remove\.

   ```
   {
   	"source-ref": "s3://custom-labels-bucket/images/IMG_1186.png",
   	"bounding-box": {
   		"image_size": [{
   			"width": 640,
   			"height": 480,
   			"depth": 3
   		}],
   		"annotations": [
   		   {              
   			"class_id": 1,    #annotation object
   			"top": 251,
   			"left": 399,
   			"width": 155,
   			"height": 101
   		}, {
   			"class_id": 0,
   			"top": 65,
   			"left": 86,
   			"width": 220,
   			"height": 334
   		}]
   	},
   	"bounding-box-metadata": {
   		"objects": [
   		>{                
   			"confidence": 1          #confidence  object
   		}, 
           {
   			"confidence": 1
   		}],
   		"class-map": {  
   			"0": "Echo",    #label 
   			"1": "Echo Dot"
   		},
   		"type": "groundtruth/object-detection",
   		"human-annotated": "yes",
   		"creation-date": "2018-10-18T22:18:13.527256",
   		"job-name": "my job"
   	}
   }
   ```



## WARNING\_UNANNOTATED\_RECORD<a name="tm-warning-WARNING_UNANNOTATED_RECORD"></a>

### Warning Message<a name="tm-warning-message-WARNING_UNANNOTATED_RECORD"></a>

Record is unannotated\.

### More information<a name="tm-warning-description-WARNING_UNANNOTATED_RECORD"></a>

An image added to a dataset by using the Amazon Rekognition Custom Labels console wasn't labeled\. The JSON line for the image isn't used for training\. 

```
{
    "source-ref": "s3://bucket/images/IMG_1186.png",
    "warnings": [
        {
            "code": "WARNING_UNANNOTATED_RECORD",
            "message": "Record is unannotated."
        } 
    ]
}
```

**To fix WARNING\_UNANNOTATED\_RECORD**
+ Label the image by using the Amazon Rekognition Custom Labels console\. For instructions, see [Assigning image\-level labels to an image](rv-assign-labels.md)\.





## WARNING\_NO\_ANNOTATIONS<a name="tm-warning-WARNING_NO_ANNOTATIONS"></a>

### Warning Message<a name="tm-warning-message-WARNING_NO_ANNOTATIONS"></a>

No annotations provided\.

### More information<a name="tm-warning-description-WARNING_NO_ANNOTATIONS"></a>

A JSON Line in Object Localization format doesn't contain any bounding box information, despite being annotated by a human \(`human-annotated = yes`\)\. The JSON Line is valid, but isn't used for training\. For more information, see [Understanding training and testing validation result manifests](tm-debugging-scope-json-line.md)\. 

```
{
    "source-ref": "s3://bucket/images/IMG_1186.png",
    "bounding-box": {
        "image_size": [
            {
                "width": 640,
                "height": 480,
                "depth": 3
            }
        ],
        "annotations": [
           
        ],
        "warnings": [
            {
                "code": "WARNING_NO_ATTRIBUTE_ANNOTATIONS",
                "message": "No attribute annotations were found."
            }
        ]
    },
    "bounding-box-metadata": {
        "objects": [
           
        ],
        "class-map": {
           
        },
        "type": "groundtruth/object-detection",
        "human-annotated": "yes",
        "creation-date": "2013-11-18 02:53:27",
        "job-name": "my job"
    },
    "warnings": [
        {
            "code": "WARNING_NO_ANNOTATIONS",
            "message": "No annotations were found."
        } 
    ]
}
```

**To fix WARNING\_NO\_ANNOTATIONS**
+ Choose one of the following options\.
  + Add the bounding box \(`annotations`\) information to the JSON Line\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\. 
  + Remove the image \(JSON Line\) from the manifest\.

## WARNING\_NO\_ATTRIBUTE\_ANNOTATIONS<a name="tm-warning-WARNING_NO_ATTRIBUTE_ANNOTATIONS"></a>

### Warning Message<a name="tm-warning-message-WARNING_NO_ATTRIBUTE_ANNOTATIONS"></a>

No attribute annotations provided\.

#### More information<a name="tm-warning-description-WARNING_NO_ATTRIBUTE_ANNOTATIONS"></a>

A JSON Line in Object Localization format doesn't contain any bounding box annotation information, despite being annotated by a human \(`human-annotated = yes`\)\. The `annotations` array is not present or is not populuated\. The JSON Line is valid, but isn't used for training\. For more information, see [Understanding training and testing validation result manifests](tm-debugging-scope-json-line.md)\. 

```
{
    "source-ref": "s3://bucket/images/IMG_1186.png",
    "bounding-box": {
        "image_size": [
            {
                "width": 640,
                "height": 480,
                "depth": 3
            }
        ],
        "annotations": [
           
        ],
        "warnings": [
            {
                "code": "WARNING_NO_ATTRIBUTE_ANNOTATIONS",
                "message": "No attribute annotations were found."
            }
        ]
    },
    "bounding-box-metadata": {
        "objects": [
           
        ],
        "class-map": {
           
        },
        "type": "groundtruth/object-detection",
        "human-annotated": "yes",
        "creation-date": "2013-11-18 02:53:27",
        "job-name": "my job"
    },
    "warnings": [
        {
            "code": "WARNING_NO_ANNOTATIONS",
            "message": "No annotations were found."
        }
    ]
}
```

**To fix WARNING\_NO\_ATTRIBUTE\_ANNOTATIONS**
+ Choose one of the following options\.
  + Add one or more bounding box `annotation` objects to the JSON Line\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\. 
  + Remove the bounding box attribute\.
  + Remove the image \(JSON Line\) from the manifest\. If other valid bounding box attributes exist in the JSON Line, you can instead remove just the invalid bounding box attribute from the JSON Line\.