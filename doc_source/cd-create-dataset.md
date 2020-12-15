# Creating an Amazon Rekognition Custom Labels Dataset<a name="cd-create-dataset"></a>

Datasets contain the images, labels, and bounding box information that is used to train and test an Amazon Rekognition Custom Labels model\. Datasets are managed by Amazon Rekognition Custom Labels projects\. You create the initial training dataset for a project during project creation\. You can also add new and existing datasets to a project after the project is created\. For example, you might create a dataset that is used to test a model\. 

You can create a dataset using images from one of the following locations\.
+ [Amazon S3 Bucket](cd-s3.md)
+ [Local Computer](cd-computer.md)
+ [SageMaker Ground Truth](cd-ground-truth.md)
+ [Existing Dataset](cd-existing-dataset.md)

## Purposing Datasets<a name="cd-dataset-purpose"></a>

We recommend that you create separate datasets for different image categories\. For example, if you have images of scenic views and images of dogs, you should create a dogs dataset and a scenic views dataset\.

### Finding Objects, Scenes, and Concepts<a name="cd-classification"></a>

If you want to create a model that predicts the presence of objects, scenes, and concepts in your images, you assign image\-level labels to the images in your dataset\. In this case, your dataset requires at least two labels\. For example, the following image shows a scene\. Your dataset could include image\-level labels such *sunrise* or *countryside*\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/sunrise.png)

When you create your dataset, you can include image\-level labels for your images\. For example, if your images are stored in an Amazon S3 bucket, you can use [folder names](cd-s3.md) to add image\-level labels\. You can also add image\-level labels to images after you create a dataset\. For more information, see [Assigning Image\-Level Labels to an Image](rv-assign-labels.md)\. You can add new labels as you need them\. For more information, see [Labeling the Dataset](rv-editing-labels.md)\.

The default number of images per dataset is 250,000\. You can request an increase for up to 500,000 images per dataset\. For more information, [Service Quotas](https://docs.aws.amazon.com/general/latest/gr/rekognition_region.html#limits_rekognition)\.

### Detecting Object Locations<a name="cd-localization"></a>

To create a model that predicts the location of objects in your images, you define object location bounding boxes and labels for the images in your dataset\. A bounding box is a box that tightly surrounds an object\. For example, the following image shows bounding boxes around an Amazon Echo and an Amazon Echo Dot\. Each bounding box has an assigned label \(*Amazon Echo* or *Amazon Echo Dot*\)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/echos.png)

To detect object locations, your dataset needs at least one label\. During model training, a further label is automatically created that represents the area outside of the bounding boxes on an image\. When you create your dataset, you can include bounding box information for your images\. For example, you can import an SageMaker Ground Truth format [manifest file](cd-manifest-files.md) that contains bounding boxes\. You can also add bounding boxes after you create a dataset\. For more information, see [Drawing Bounding Boxes](rv-bounding-box.md)\. You can add new labels as you need them\. For more information, see [Labeling the Dataset](rv-editing-labels.md)\.

You can have up to 250,000 images per dataset\. This value can't be increased\. For more information, see [Limits in Amazon Rekognition Custom Labels](limits.md)\.

### Combining Image\-Level and Bounding Box Labeled Images<a name="cd-combine"></a>

You can combine image\-level labels and bounding box labeled images in a single dataset\. In this case, Amazon Rekognition Custom Labels chooses whether to create an image\-level model or an object location model\. 

For information about adding labels to a dataset, see [Labeling the Dataset](rv-editing-labels.md)\.