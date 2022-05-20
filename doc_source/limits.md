# Guidelines and quotas in Amazon Rekognition Custom Labels<a name="limits"></a>

The following sections provide guidelines and quotas when using Amazon Rekognition Custom Labels\.

## Supported Regions<a name="supported-regions"></a>

For a list of AWS Regions where Amazon Rekognition Custom Labels is available, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rekognition.html) in the *Amazon Web Services General Reference*\.

## Quotas<a name="quotas"></a>

The following is a list of limits in Amazon Rekognition Custom Labels\. For information about limits you can change, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/rekognition.html)\. To change a limit, see [Create Case](https://console.aws.amazon.com/support/v1#/case/create?issueType=service-limit-increase)\.

### Training<a name="limits-training"></a>
+ Supported file formats are PNG and JPEG image formats\. 
+ Maximum number of training datasets in a version of a model is 1\. 
+ Maximum dataset manifest file size is 1 GB\. 
+ Minimum number of unique labels per Objects, Scenes, and Concepts \(classification\) dataset is 2\.
+ Minimum number of unique labels per Object Location \(detection\) dataset is 1\.
+ Maximum number of unique labels per manifest is 250\.
+ Minimum number of images per label is 1\. 
+ Maximum number of images per Object Location \(detection\) dataset is 250,000\.

  The limit for *Asia Pacific \(Mumbai\)* and *Europe \(London\)* AWS Regions is 28,000 images\. 
+ Maximum number of images per Objects, Scenes, and Concepts \(classification\) dataset is 500,000\. The default is 250,000\. To request an increase, see [Create Case](https://console.aws.amazon.com/support/v1#/case/create?issueType=service-limit-increase)\. 

  The limit for *Asia Pacific \(Mumbai\)* and *Europe \(London\)* AWS Regions is 28,000 images\. You can't request a limit increase\. 
+ Maximum number of labels per image is 50\.
+ Minimum number of bounding boxes in an image is 0\.
+ Maximum number of bounding boxes in an image is 50\.
+ Minimum image dimension of image file in an Amazon S3 bucket is 64 pixels x 64 pixels\.
+ Maximum image dimension of image file in an Amazon S3 bucket is 4096 pixels x 4096 pixels\.
+ Maximum file size for an image in an Amazon S3 bucket is 15 MB\.
+ Maximum image aspect ratio is 20:1\.

### Testing<a name="limits-testing"></a>
+ Maximum number of testing datasets in a version of a model is 1\. 
+ Maximum dataset manifest file size is 1 GB\. 
+ Minimum number of unique labels per Objects, Scenes, and Concepts \(classification\) dataset is 2\.
+ Minimum number of unique labels per Object Location \(detection\) dataset is 1\.
+ Maximum number of unique labels per dataset is 250\.
+ Minimum number of images per label is 0\. 
+ Maximum number of images per label is 1000\. 
+ Maximum number of images per Object Location \(detection\) dataset is 250,000\.

  The limit for *Asia Pacific \(Mumbai\)* and *Europe \(London\)* AWS Regions is 7,000 images\. 
+ Maximum number of images per Objects, Scenes, and Concepts \(classification\) dataset is 500,000\. The default is 250,000\. To request an increase, see [Create Case](https://console.aws.amazon.com/support/v1#/case/create?issueType=service-limit-increase)\. 

  The limit for *Asia Pacific \(Mumbai\)* and *Europe \(London\)* AWS Regions is 7,000 images\. You can't request a limit increase\. 
+ Minimum number of labels per image per manifest is 0\.
+ Maximum number of labels per image per manifest is 50\.
+ Minimum number of bounding boxes in an image per manifest is 0\.
+ Maximum number of bounding boxes in an image per manifest is 50\.
+ Minimum image dimension of an image file in an Amazon S3 bucket is 64 pixels x 64 pixels\.
+ Maximum image dimension of an image file in an Amazon S3 bucket is 4096 pixels x 4096 pixels\.
+ Maximum file size for an image in an Amazon S3 bucket is 15 MB\.
+ Supported file formats are PNG and JPEG image formats\. 
+ Maximum image aspect ratio is 20:1\.

### Detection<a name="limits-detection"></a>
+ Maximum size of images passed as raw bytes is 4 MB\.
+ Maximum file size for an image in an Amazon S3 bucket is 15 MB\.
+ Maximum number of running models per account is 2.
+ Minimum image dimension of an input image file \(stored in an Amazon S3 bucket or supplied as image bytes\) is 64 pixels x 64 pixels\.
+ Maximum image dimension of an input image file \(stored in an Amazon S3 or supplied as image bytes\) is 4096 pixels x 4096 pixels\.
+ Supported file formats are PNG and JPEG image formats\. 
+ Maximum image aspect ratio is 20:1\.
