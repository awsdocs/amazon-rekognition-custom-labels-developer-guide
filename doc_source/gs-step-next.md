# Step 6: Next steps<a name="gs-step-next"></a>

After you finished trying the examples projects, you can use your own images to create your own model\. The [Tutorial: Training a model with the Amazon Rekognition Custom Labels console](training-model-console.md) model shows you how to train a model by creating a dataset with your own images\.

Use the labeling information in the following table to train models similar to the example projects\.


| Example | Training images | Test images | 
| --- | --- | --- | 
|  Image classification \(Rooms\)  |  1 Image\-level label per image  |  1 Image\-level label per image   | 
|  Multi\-label classification \(Flowers\)  |  Multiple image\-level labels per image  |  Multiple image\-level labels per image  | 
|  Brand detection \(Logos\)  |  image level\-labels \(you can also use Labeled bounding boxes\)  |  Labeled bounding boxes  | 
|  Image localization \(Circuit boards\)  |  Labeled bounding boxes  |  Labeled bounding boxes  | 

The example models can return multiple labels in the response from `DetectCustomLabels`\. For information, see [Example projects](gs-introduction.md#gs-example-projects)\.

For detailed information about creating datasets and training models, see [Managing an Amazon Rekognition Custom Labels dataset](cd-managing-datasets.md) and [Managing an Amazon Rekognition Custom Labels model](um-use-model.md)\.

You can also use the AWS SDK to train a model\. For more information, see [Tutorial: Training a model with the Amazon Rekognition Custom Labels SDK](tutorial-cli.md)\.

For other examples, see [Examples](examples.md)\.