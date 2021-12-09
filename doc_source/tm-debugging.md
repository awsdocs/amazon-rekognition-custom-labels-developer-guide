# Debugging a failed model training<a name="tm-debugging"></a>

You might encounter errors during model training\. Amazon Rekognition Custom Labels reports training errors in the console and in the response from [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions)\.

Errors are either terminal \(training can't continue\), or they are non\-terminal \(training can continue\)\. For errors that relate to the contents of the training and testing datasets, you can download the validation results \( a [manifest summary](tm-debugging-summary.md) and [training and testing validation manifests](tm-debugging-scope-json-line.md)\)\. Use the error codes in the validation results to find further information in this section\. This section also provides information for manifest file errors \(terminal errors that happen before the manifest file contents are validated\)\. 

**Note**  
A manifest is the file used to store the contents of a dataset\.

You can fix some errors by using the Amazon Rekognition Custom Labels console\. Other errors might require you to make updates to the training or testing manifest files\. You might need to make other changes, such as IAM permissions\. For more information, see the documentation for individual errors\.

## Terminal errors<a name="tm-error-categories-terminal"></a>

Terminal errors stop the training of a model\. There are 3 categories of terminal training errors – service errors, manifest file errors, and manifest content errors\. 

In the console, Amazon Rekognition Custom Labels shows terminal errors for a model in the **Status message** column of the projects page\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/terminal-errors.png)

If you using the AWS SDK, you can find out if a terminal manifest file error or a terminal manifest content error has occured by checking the response from [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions)\. In this case, the `Status` value is `TRAINING_FAILED` and `StatusMessage` field contains the error\. 

### Service errors<a name="tm-error-category-service"></a>

Terminal service errors occur when Amazon Rekognition experiences a service issue and can't continue training\. For example, the failure of another service that Amazon Rekognition Custom Labels depends upon\. Amazon Rekognition Custom Labels reports service errors in the console as *Amazon Rekognition experienced a service issue*\. If you use the AWS SDK, service errors that occur during training are raised as an `InternalServerError` exception by [CreateProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProjectVersion) and [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions)\.

If a service error occurs, retry training of the model\. If training continues to fail, contact *[AWS Support](https://aws.amazon.com/premiumsupport/)* and include any error information reported with the service error\. 

### Terminal manifest file errors<a name="tm-error-category-terminal"></a>

Manifest file errors are terminal errors, in the training and testing datasets, that happen at the file level, or across multiple files\. Manifest file errors are detected before the contents of the training and testing datasets are validated\. Manifest file errors prevent the reporting of [non\-terminal validation errors](#tm-error-category-non-terminal-errors)\. For example, an empty training manifest file generates an *The manifest file is empty* error\. Since the file is empty, no non\-terminal JSON Line validation errors can be reported\. The manifest summary is also not created\. 

You must fix manifest file errors before you can train your model\. 

The following lists the manifest file errors\.
+ [The manifest file extension or contents are invalid\.](tm-terminal-errors-reference.md#tm-error-message-ERROR_MANIFEST_INACCESSIBLE_OR_UNSUPPORTED_FORMAT)
+ [The manifest file is empty\.](tm-terminal-errors-reference.md#tm-error-message-ERROR_EMPTY_MANIFEST)
+ [The manifest file size exceeds the maximum supported size\.](tm-terminal-errors-reference.md#tm-error-message-ERROR_MANIFEST_SIZE_TOO_LARGE)
+ [Unable to write to output S3 bucket\.](tm-terminal-errors-reference.md#tm-error-message-ERROR_CANNOT_WRITE_OUTPUT_S3_BUCKET)
+ [The S3 bucket permissions are incorrect\.](tm-terminal-errors-reference.md#tm-error-message-ERROR_INVALID_PERMISSIONS_MANIFEST_S3_BUCKET)

### Terminal manifest content errors<a name="tm-error-category-combined-terminal"></a>

Manifest content errors are terminal errors that relate to the content within a manifest\. For example, if you get the error [The manifest file contains insufficient labeled images per label to perform auto\-split](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_INSUFFICIENT_IMAGES_PER_LABEL_FOR_AUTOSPLIT), training can't finish as there aren't enough labeled images in the training dataset to create a testing dataset\. 

As well as being reported in the console and in the response from `DescribeProjectVersions`, the error is reported in the manifest summary along with any other terminal manifest content errors\. For more information, see [Understanding the manifest summary](tm-debugging-summary.md)\.

Non terminal JSON Line errors are also reported in seperate training and testing validation results manifests\. The non\-terminal JSON Line errors found by Amazon Rekognition Custom Labels are not necessarily related to the manifest content error\(s\) that stop training\. For more information, see [Understanding training and testing validation result manifests](tm-debugging-scope-json-line.md)\. 

You must fix manifest content errors before you can train your model\. 

The following are the error messages for manifest content errors\. 
+ [The manifest file contains too many invalid rows\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST)
+ [The manifest file contains images from multiple S3 buckets\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_IMAGES_IN_MULTIPLE_S3_BUCKETS)
+ [Invalid owner id for images S3 bucket\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_INVALID_IMAGES_S3_BUCKET_OWNER)
+ [The manifest file contains insufficient labeled images per label to perform auto\-split\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_INSUFFICIENT_IMAGES_PER_LABEL_FOR_AUTOSPLIT)
+ [The manifest file has too few labels\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_MANIFEST_TOO_FEW_LABELS)
+ [The manifest file has too many labels\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_MANIFEST_TOO_MANY_LABELS)
+ [Less than \{\}% label overlap between the training and testing manifest files\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_INSUFFICIENT_LABEL_OVERLAP)
+ [The manifest file has too few usable labels\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_MANIFEST_TOO_FEW_USABLE_LABELS)
+ [Less than \{\}% usable label overlap between the training and testing manifest files\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_INSUFFICIENT_USABLE_LABEL_OVERLAP)
+ [Failed to copy images from S3 bucket\.](tm-debugging-aggregate-errors.md#tm-error-message-ERROR_FAILED_IMAGES_S3_COPY)

## Non terminal JSON line validation errors<a name="tm-error-category-non-terminal-errors"></a>

JSON Line validation errors are non\-terminal errors that don't require Amazon Rekognition Custom Labels to stop training a model\.

JSON Line validation errors are not shown in the console\. 

In the training and testing datasets, a JSON Line represents the training or testing information for a single image\. Validation errors in a JSON Line, such as an invalid image, are reported in the training and testing validation manifests\. Amazon Rekognition Custom Labels completes training using the other, valid, JSON Lines that are in the manifest\. For more information, see [Understanding training and testing validation result manifests](tm-debugging-scope-json-line.md)\. For information about validation rules, see [Validation rules for manifest files](md-create-manifest-file-validation-rules.md)\.

**Note**  
Training fails if there are too many JSON Line errors\.

We recommend that you also fix non\-terminal JSON Line errors errors as they can potentially cause future errors or impact your model training\.

Amazon Rekognition Custom Labels can generate the following non\-terminal JSON Line validation errors\.
+ [The source\-ref key is missing\.](tm-debugging-json-line-errors.md#tm-error-ERROR_MISSING_SOURCE_REF)
+ [The format of the source\-ref value is invalid\. ](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_SOURCE_REF_FORMAT)
+ [No label attributes found\.](tm-debugging-json-line-errors.md#tm-error-ERROR_NO_LABEL_ATTRIBUTES)
+ [The format of the label attribute \{\} is invalid\.](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_LABEL_ATTRIBUTE_FORMAT)
+ [The format of the label attributemetadata is invalid\.](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_LABEL_ATTRIBUTE_METADATA_FORMAT)
+ [No valid label attributes found\.](tm-debugging-json-line-errors.md#tm-error-ERROR_NO_VALID_LABEL_ATTRIBUTES)
+ [One or more bounding boxes has a missing confidence value\.](tm-debugging-json-line-errors.md#tm-error-ERROR_MISSING_BOUNDING_BOX_CONFIDENCE)
+ [One of more class ids is missing from the class map\.](tm-debugging-json-line-errors.md#tm-error-ERROR_MISSING_CLASS_MAP_ID)
+ [The JSON Line has an invalid format\.](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_JSON_LINE)
+ [The image is invalid\. Check S3 path and/or image properties\.](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_IMAGE)
+ [The bounding box has off frame values\.](tm-debugging-json-line-errors.md#tm-error-ERROR_INVALID_BOUNDING_BOX)
+ [The height and width of the bounding box is too small\.](tm-debugging-json-line-errors.md#tm-error-ERROR_BOUNDING_BOX_TOO_SMALL)
+ [There are more bounding boxes than the allowed maximum\.](tm-debugging-json-line-errors.md#tm-error-ERROR_TOO_MANY_BOUNDING_BOXES)
+ [No valid annotations found\.](tm-debugging-json-line-errors.md#tm-error-ERROR_NO_VALID_ANNOTATIONS)