# Terminal manifest content errors<a name="tm-debugging-aggregate-errors"></a>

This topic describes the [Terminal manifest content errors](tm-debugging.md#tm-error-category-combined-terminal) reported in the manifest summary\. The manifest summary includes an error code and message for each detected error\. For more information, see [Understanding the manifest summary](tm-debugging-summary.md)\. Terminal manifest content errors don't stop the reporting of [Non terminal JSON line validation errors](tm-debugging.md#tm-error-category-non-terminal-errors)\. 

## ERROR\_MANIFEST\_ROWS\_EXCEEDS\_MAXIMUM<a name="tm-error-ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM"></a>

### Error message<a name="tm-error-message-ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM"></a>

The manifest file has too many rows\.

### More information<a name="tm-error-description-ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM"></a>

The number of JSON Lines \(number of images\) in the manifest file is greater than the allowed limit\. The limit is different for image\-level models and object location models\. For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\. 

JSON Line error are validated until the number of JSON Lines reaches the `ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM` limit\. 

You can't use the Amazon Rekognition Custom Labels console to fix error `ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM`\.

**To fix `ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM`**
+ Reduce the number of JSON Lines in the manifest\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.



## ERROR\_TOO\_MANY\_INVALID\_ROWS\_IN\_MANIFEST<a name="tm-error-ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST"></a>

### Error message<a name="tm-error-message-ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST"></a>

The manifest file contains too many invalid rows\. 

### More information<a name="tm-error-description-ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST"></a>

An `ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST` error occurs if there are too many JSON Lines that contain invalid content\.

You can't use the Amazon Rekognition Custom Labels console to fix an `ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST` error\.

**To fix ERROR\_TOO\_MANY\_INVALID\_ROWS\_IN\_MANIFEST**

1. Check the manifest for JSON Line errors\. For more information, see [Understanding training and testing validation result manifests](tm-debugging-scope-json-line.md)\.

1.  Fix JSON Lines that have errors For more information, see [Non\-Terminal JSON Line Validation Errors](tm-debugging-json-line-errors.md)\. 



## ERROR\_IMAGES\_IN\_MULTIPLE\_S3\_BUCKETS<a name="tm-error-ERROR_IMAGES_IN_MULTIPLE_S3_BUCKETS"></a>

### Error message<a name="tm-error-message-ERROR_IMAGES_IN_MULTIPLE_S3_BUCKETS"></a>

The manifest file contains images from multiple S3 buckets\.

### More information<a name="tm-error-description-ERROR_IMAGES_IN_MULTIPLE_S3_BUCKETS"></a>

A manifest can only reference images stored in a single bucket\. Each JSON Line stores the Amazon S3 location of an image location in the value of `source-ref`\. In the following example, the bucket name is *my\-bucket*\. 

```
"source-ref": "s3://my-bucket/images/sunrise.png"
```

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

**To fix `ERROR_IMAGES_IN_MULTIPLE_S3_BUCKETS`**
+ Ensure that all your images are in the same Amazon S3 bucket and that the value of `source-ref` in every JSON Line references the bucket where your images are stored\. Alternatively, choose a preferred Amazon S3 bucket and remove the JSON Lines where `source-ref` doesn't reference your preferred bucket\. 



## ERROR\_INVALID\_PERMISSIONS\_IMAGES\_S3\_BUCKET<a name="tm-error-ERROR_INVALID_PERMISSIONS_IMAGES_S3_BUCKET"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_PERMISSIONS_IMAGES_S3_BUCKET"></a>

The permissions for the images S3 bucket are invalid\.

### More information<a name="tm-error-description-ERROR_INVALID_PERMISSIONS_IMAGES_S3_BUCKET"></a>

The permissions on the Amazon S3 bucket that contains the images are incorrect\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

**To fix `ERROR_INVALID_PERMISSIONS_IMAGES_S3_BUCKET`**
+ Check the permissions of the bucket containing the images\. The value of the `source-ref` for an image contains the bucket location\. 



## ERROR\_INVALID\_IMAGES\_S3\_BUCKET\_OWNER<a name="tm-error-ERROR_INVALID_IMAGES_S3_BUCKET_OWNER"></a>

### Error message<a name="tm-error-message-ERROR_INVALID_IMAGES_S3_BUCKET_OWNER"></a>

Invalid owner id for images S3 bucket\.

### More information<a name="tm-error-description-ERROR_INVALID_IMAGES_S3_BUCKET_OWNER"></a>

The owner of the bucket that contains the training or test images is different from the owner of the bucket that contains the training or test manifest\. You can use the following command to find the owner of a bucket\.

```
aws s3api get-bucket-acl --bucket bucket name
```

The `OWNER` `ID` must match for the buckets that store the images and manifest files\.

**To fix ERROR\_INVALID\_IMAGES\_S3\_BUCKET\_OWNER**

1. Choose the desired owner of the training, testing, output, and image buckets\. The owner must have permissions to use Amazon Rekognition Custom Labels\.

1. For each bucket not currently owned by the desired owner, create a new Amazon S3 bucket owned by the preferred owner\. 

1. Copy the old bucket contents to the new bucket\. For more information, see [How can I copy objects between Amazon S3 buckets?](https://aws.amazon.com/premiumsupport/knowledge-center/move-objects-s3-bucket/)\.



You can't use the Amazon Rekognition Custom Labels console to fix this error\.

## ERROR\_INSUFFICIENT\_IMAGES\_PER\_LABEL\_FOR\_AUTOSPLIT<a name="tm-error-ERROR_INSUFFICIENT_IMAGES_PER_LABEL_FOR_AUTOSPLIT"></a>

### Error message<a name="tm-error-message-ERROR_INSUFFICIENT_IMAGES_PER_LABEL_FOR_AUTOSPLIT"></a>

The manifest file contains insufficient labeled images per label to perform auto\-split\.

### More information<a name="tm-error-description-ERROR_INSUFFICIENT_IMAGES_PER_LABEL_FOR_AUTOSPLIT"></a>

During model training, you can create a testing dataset by using 20% of the images from the training dataset\. ERROR\_INSUFFICIENT\_IMAGES\_PER\_LABEL\_FOR\_AUTOSPLIT occurs when there aren't enough images to create an acceptable testing dataset\.

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

**To fix ERROR\_INSUFFICIENT\_IMAGES\_PER\_LABEL\_FOR\_AUTOSPLIT**
+ Add more labeled image to your training dataset\. You can add images in the Amazon Rekognition Custom Labels console by adding images to the training dataset, or by adding JSON Lines to your training manifest\. For more information, see [Managing an Amazon Rekognition Custom Labels dataset](cd-managing-datasets.md)\.



## ERROR\_MANIFEST\_TOO\_FEW\_LABELS<a name="tm-error-ERROR_MANIFEST_TOO_FEW_LABELS"></a>

### Error message<a name="tm-error-message-ERROR_MANIFEST_TOO_FEW_LABELS"></a>

The manifest file has too few labels\.

### More information<a name="tm-error-description-ERROR_MANIFEST_TOO_FEW_LABELS"></a>

Training and testing datasets have a required minumum number of labels\. The minimum depends on if the dataset trains/tests a model to detect image\-level labels \(classification\) or if the model detects object locations\. If the training dataset is split to create a testing dataset, the number of labels in the dataset is determined after the training dataset is split\. For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\.

**To fix ERROR\_MANIFEST\_TOO\_FEW\_LABELS \(console\)**

1. Add more new labels to the dataset\. For more information, see [Labeling the dataset](rv-editing-labels.md)\. 

1. Add the new labels to images in the dataset\. If your model detects image\-level labels, see [Assigning image\-level labels to an image](rv-assign-labels.md)\. If your model detects object locations, see [Drawing bounding boxes](rv-bounding-box.md)\.



**To fix ERROR\_MANIFEST\_TOO\_FEW\_LABELS \(JSON Line\)**
+ Add JSON Lines for new images that have new labels\. For more information, see [Creating a manifest file](cd-manifest-files.md)\. If your model detects image\-level labels, you add new labels names to the `class-name` field\. For example, the label for the following image is *Sunrise*\.

  ```
  {
      "source-ref": "s3://bucket/images/sunrise.png",
      "testdataset-classification_Sunrise": 1,
      "testdataset-classification_Sunrise-metadata": {
          "confidence": 1,
          "job-name": "labeling-job/testdataset-classification_Sunrise",
          "class-name": "Sunrise",
          "human-annotated": "yes",
          "creation-date": "2018-10-18T22:18:13.527256",
          "type": "groundtruth/image-classification"
      }
  }
  ```

   If your model detects object locations, add new labels to the `class-map`, as shown in the following example\.

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

  You need to map the class map table to the bounding box annotations\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\.

## ERROR\_MANIFEST\_TOO\_MANY\_LABELS<a name="tm-error-ERROR_MANIFEST_TOO_MANY_LABELS"></a>

### Error message<a name="tm-error-message-ERROR_MANIFEST_TOO_MANY_LABELS"></a>

The manifest file has too many labels\.

### More information<a name="tm-error-description-ERROR_MANIFEST_TOO_MANY_LABELS"></a>

The number of unique labels in the manifest \(dataset\) is more than the allowed limit\. If the training dataset is split to create a testing dataset, the mumber of labels is determined after the split\. 

**To fix ERROR\_MANIFEST\_TOO\_MANY\_LABELS \(Console\)**
+ Remove labels from the dataset\. For more information, see [Labeling the dataset](rv-editing-labels.md)\. The labels are automatically removed from the images and bounding boxes in your dataset\.



**To fix ERROR\_MANIFEST\_TOO\_MANY\_LABELS \(JSON Line\)**
+ Manifests with image level JSON Lines – If the image has a single label, remove the JSON Lines for images that use the desired label\. If the JSON Line contains multiple labels, remove only the JSON object for the desired label\. For more information, see [Adding multiple image\-level labels to an image](cd-manifest-files-classification.md#cd-classification-multiple-labels)\. 

  Manifests with object location JSON Lines – Remove the bounding box and associated label information for the label that you want to remove\. Do this for each JSON Line that contains the desired label\. You need to remove the label from the `class-map` array and corresponding objects in the `objects` and `annotations` array\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\.

## ERROR\_INSUFFICIENT\_LABEL\_OVERLAP<a name="tm-error-ERROR_INSUFFICIENT_LABEL_OVERLAP"></a>

### Error message<a name="tm-error-message-ERROR_INSUFFICIENT_LABEL_OVERLAP"></a>

Less than \{\}% label overlap between the training and testing manifest files\.

### More information<a name="tm-error-description-ERROR_INSUFFICIENT_LABEL_OVERLAP"></a>

There is less than 50% overlap between the testing dataset label names and the training dataset label names\.

**To fix ERROR\_INSUFFICIENT\_LABEL\_OVERLAP \(Console\)**
+ Remove labels from the training dataset\. Alternatively, add more common labels to your testing dataset\. For more information, see [Labeling the dataset](rv-editing-labels.md)\. The labels are automatically removed from the images and bounding boxes in your dataset\.



**To fix ERROR\_INSUFFICIENT\_LABEL\_OVERLAP by removing labels from the training dataset \(JSON Line\)**
+ Manifests with image level JSON Lines – If the image has a single label, remove the JSON Line for the image that use the desired label\. If the JSON Line contains multiple labels, remove only the JSON object for the desired label\. For more information, see [Adding multiple image\-level labels to an image](cd-manifest-files-classification.md#cd-classification-multiple-labels)\. Do this for each JSON Line in the manifest that contains the label that you want to remove\.

  Manifests with object location JSON Lines – Remove the bounding box and associated label information for the label that you want to remove\. Do this for each JSON Line that contains the desired label\. You need to remove the label from the `class-map` array and corresponding objects in the `objects` and `annotations` array\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\.

**To fix ERROR\_INSUFFICIENT\_LABEL\_OVERLAP by adding common labels to the testing dataset \(JSON Line\)**
+ Add JSON Lines to the testing dataset that include images labeled with labels already in the training dataset\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.

## ERROR\_MANIFEST\_TOO\_FEW\_USABLE\_LABELS<a name="tm-error-ERROR_MANIFEST_TOO_FEW_USABLE_LABELS"></a>

### Error message<a name="tm-error-message-ERROR_MANIFEST_TOO_FEW_USABLE_LABELS"></a>

The manifest file has too few usable labels\.

### More information<a name="tm-error-description-ERROR_MANIFEST_TOO_FEW_USABLE_LABELS"></a>

A training manifest can contain JSON Lines in image\-level label format and in object location format\. Depending on type of JSON Lines found in the training manifest, Amazon Rekognition Custom Labels chooses to create a model that detects image\-level labels, or a model that detects object locations\. Amazon Rekognition Custom Labels filters out valid JSON records for JSON Lines that are not in the chosen format\. ERROR\_MANIFEST\_TOO\_FEW\_USABLE\_LABELS occurs when the number of labels in the chosen model type manifest is insufficient to train the model\.

A minimum of 1 label is required to train a model that detects image\-level labels\. A minimum of 2 labels is required to train a model that object locations\. 

**To fix ERROR\_MANIFEST\_TOO\_FEW\_USABLE\_LABELS \(Console\)**

1. Check the `use_case` field in the manifest summary\.

1. Add more labels to the training dataset for the use case \(image level or object localization\) that matches the value of `use_case`\. For more information, see [Labeling the dataset](rv-editing-labels.md)\. The labels are automatically removed from the images and bounding boxes in your dataset\.

**To fix ERROR\_MANIFEST\_TOO\_FEW\_USABLE\_LABELS \(JSON Line\)**

1. Check the `use_case` field in the manifest summary\.

1. Add more labels to the training dataset for the use case \(image level or object localization\) that matches the value of `use_case`\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.



## ERROR\_INSUFFICIENT\_USABLE\_LABEL\_OVERLAP<a name="tm-error-ERROR_INSUFFICIENT_USABLE_LABEL_OVERLAP"></a>

### Error message<a name="tm-error-message-ERROR_INSUFFICIENT_USABLE_LABEL_OVERLAP"></a>

Less than \{\}% usable label overlap between the training and testing manifest files\.

### More information<a name="tm-error-description-ERROR_INSUFFICIENT_USABLE_LABEL_OVERLAP"></a>

 

A training manifest can contain JSON Lines in image\-level label format and in object location format\. Depending on the formats found in the training manifest, Amazon Rekognition Custom Labels chooses to create a model that detects image\-level labels, or a model that detects object locations\. Amazon Rekognition Custom Labels doesn't use valid JSON records for JSON Lines that are not in the chosen model format\. ERROR\_INSUFFICIENT\_USABLE\_LABEL\_OVERLAP occurs when there is less than 50% overlap between the testing and training labels that are used\.

**To fix ERROR\_INSUFFICIENT\_USABLE\_LABEL\_OVERLAP \(Console\)**
+ Remove labels from the training dataset\. Alternatively, add more common labels to your testing dataset\. For more information, see [Labeling the dataset](rv-editing-labels.md)\. The labels are automatically removed from the images and bounding boxes in your dataset\.



**To fix ERROR\_INSUFFICIENT\_USABLE\_LABEL\_OVERLAP by removing labels from the training dataset \(JSON Line\)**
+ Datasets used to detect image\-level labels – If the image has a single label, remove the JSON Line for the image that use the desired label\. If the JSON Line contains multiple labels, remove only the JSON object for the desired label\. For more information, see [Adding multiple image\-level labels to an image](cd-manifest-files-classification.md#cd-classification-multiple-labels)\. Do this for each JSON Line in the manifest that contains the label that you want to remove\.

  Datasets used to detects object locations – Remove the bounding box and associated label information for the label that you want to remove\. Do this for each JSON Line that contains the desired label\. You need to remove the label from the `class-map` array and corresponding objects in the `objects` and `annotations` array\. For more information, see [Object localization in manifest files](cd-manifest-files-object-detection.md)\.

**To fix ERROR\_INSUFFICIENT\_USABLE\_LABEL\_OVERLAP by adding common labels to the testing dataset \(JSON Line\)**
+ Add JSON Lines to the testing dataset that include images labeled with labels already in the training dataset\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.



## ERROR\_FAILED\_IMAGES\_S3\_COPY<a name="tm-error-ERROR_FAILED_IMAGES_S3_COPY"></a>

### Error message<a name="tm-error-message-ERROR_FAILED_IMAGES_S3_COPY"></a>

Failed to copy images from S3 bucket\.

### More information<a name="tm-error-description-ERROR_FAILED_IMAGES_S3_COPY"></a>

The service wasn't able to copy any of the images in your your dataset\. 

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

**To fix ERROR\_FAILED\_IMAGES\_S3\_COPY**

1. Check the permissions of your images\.

1. If you are using AWS KMS, check the bucket policy\. For more information, see [Decrypting files encrypted with AWS Key Management Service](su-encrypt-bucket.md#su-kms-encryption)\.