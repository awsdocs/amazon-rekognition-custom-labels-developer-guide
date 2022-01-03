# Non\-terminal errors<a name="debugging-datasets-non-terminal-errors"></a>

The following are non\-terminal errors that can occur during dataset creation or update\. These errors can invalidate an entire JSON Line or invalidate annotations within a JSON Line\. If a JSON Line has an error, it is not used for training\. If an annotation within a JSON Line has an error, the JSON Line is still used for training, but without the broken annotation\. For more information about JSON Lines, see [Creating a manifest file](md-create-manifest-file.md)\.

You can access non\-terminal errors from the console and by calling the `ListDatasetEntries` API\. For more information, see [Listing dataset entries \(SDK\)](md-listing-dataset-entries-sdk.md)\.

The following errors are are also returned during training\. We recommend that you fix these errors before training your model\.For more information, see [Non\-Terminal JSON Line Validation Errors](tm-debugging-json-line-errors.md)\.
+ [ERROR\_NO\_LABEL\_ATTRIBUTES](tm-debugging-json-line-errors.md#tm-error-ERROR_NO_LABEL_ATTRIBUTES)
+ [ERROR\_INVALID\_LABEL\_ATTRIBUTE\_FORMAT](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_LABEL_ATTRIBUTE_FORMAT)
+ [ERROR\_INVALID\_LABEL\_ATTRIBUTE\_METADATA\_FORMAT](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_LABEL_ATTRIBUTE_METADATA_FORMAT)
+ [ERROR\_NO\_VALID\_LABEL\_ATTRIBUTES](tm-debugging-json-line-errors.md#tm-error-ERROR_NO_VALID_LABEL_ATTRIBUTES)
+ [ERROR\_INVALID\_BOUNDING\_BOX](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_BOUNDING_BOX)
+ [ERROR\_INVALID\_IMAGE\_DIMENSION](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_IMAGE_DIMENSION)
+ [ERROR\_BOUNDING\_BOX\_TOO\_SMALL](tm-debugging-json-line-errors.md#tm-error-ERROR_BOUNDING_BOX_TOO_SMALL)
+ [ERROR\_NO\_VALID\_ANNOTATIONS](tm-debugging-json-line-errors.md#tm-error-ERROR_NO_VALID_ANNOTATIONS)
+ [ERROR\_MISSING\_BOUNDING\_BOX\_CONFIDENCE](tm-debugging-json-line-errors.md#tm-error-ERROR_MISSING_BOUNDING_BOX_CONFIDENCE)
+ [ERROR\_MISSING\_CLASS\_MAP\_ID](tm-debugging-json-line-errors.md#tm-error-ERROR_MISSING_CLASS_MAP_ID)
+ [ERROR\_TOO\_MANY\_BOUNDING\_BOXES](tm-debugging-json-line-errors.md#tm-error-ERROR_TOO_MANY_BOUNDING_BOXES)
+ [ERROR\_UNSUPPORTED\_USE\_CASE\_TYPE](tm-debugging-json-line-errors.md#tm-error-ERROR_UNSUPPORTED_USE_CASE_TYPE)
+ [ERROR\_INVALID\_LABEL\_NAME\_LENGTH](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_LABEL_NAME_LENGTH)

## Accessing non\-terminal errors<a name="debugging-dataset-access-non-terminal-errors"></a>

You can use the console to find out which images in a dataset have non\-terminal errors\. You can also call, call `ListDatasetEntries` API to get the error messages\. For more information, see [Listing dataset entries \(SDK\)](md-listing-dataset-entries-sdk.md)\. 

**To access non\-terminal errors\(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project that you want to use\. The details page for your project is displayed\.

1. If you want to view non\-terminal errors in your training dataset, choose the **Training** tab\. Otherwise choose the **Test** tab to view non\-terminal errors in your test dataset\. 

1. In the **Labels** section of the dataset gallery, choose **Errors**\. The dataset gallery is filtered to only show images with errors\.

1. Choose **Error** underneath an image to see the error code\. Use the information at [Non\-Terminal JSON Line Validation Errors](tm-debugging-json-line-errors.md) to fix the error\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/dataset-non-terminal-error.jpg)