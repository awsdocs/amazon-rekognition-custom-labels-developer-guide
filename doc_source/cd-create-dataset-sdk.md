# Create training and test datasets \(SDK\)<a name="cd-create-dataset-sdk"></a>

To create your training and test datasets, you use the `CreateDataset` API\. You can create a dataset by using an existing Amazon Sagemaker format manifest file or by copying an existing Amazon Rekognition Custom Labels dataset\. If necessary, create your own manifest file\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.

You can also create an empty dataset and add dataset entries at a later time\. For more information, see [Adding a dataset to a project \(SDK\)](md-add-dataset.md#md-add-dataset-sdk)\.

To add entries to an existing dataset, use the `UpdateDatasetEntries` API\. For more information, see [Adding more images \(SDK\)](md-add-images.md#md-add-images-sdk)\.

You can create a test dataset from the training dataset in your project by calling the `DistributeDatasetEntries` API\. For more information, see [Distributing a training dataset \(SDK\)](md-distributing-datasets.md)

For more information, see [Managing datasets](managing-dataset.md)\.

**Topics**
+ [Creating a dataset with a manifest file \(SDK\)](md-create-dataset-ground-truth-sdk.md)
+ [Creating a dataset using an existing dataset \(SDK\)](md-create-dataset-existing-dataset-sdk.md)