# COCO Format<a name="cd-coco-overview"></a>

A COCO dataset consists of 5 sections of information that provide information for the entire dataset\. The format for a COCO object detection dataset is documented at [COCO Data Format](http://cocodataset.org/#format-data)\. 
+ info – general information about the dataset\. 
+ licenses – license information for the images in the dataset\.
+ [images](#cd-coco-images) – a list of images in the dataset\.
+ [annotations](#cd-coco-annotations) – a list of annotations \(including bounding boxes\) that are present in all images in the dataset\.
+ [categories](#cd-coco-categories) – a list of label categories\.

To create a Custom Labels manifest, you use the `images`, `annotations`, and `categories` lists from the COCO manifest file\. The other sections \(`info`, `licences`\) aren’t required\. The following is an example COCO manifest file\.

```
{
    "info": {
        "description": "COCO 2017 Dataset","url": "http://cocodataset.org","version": "1.0","year": 2017,"contributor": "COCO Consortium","date_created": "2017/09/01"
    },
    "licenses": [
        {"url": "http://creativecommons.org/licenses/by/2.0/","id": 4,"name": "Attribution License"}
    ],
    "images": [
        {"id": 242287, "license": 4, "coco_url": "http://images.cocodataset.org/val2017/xxxxxxxxxxxx.jpg", "flickr_url": "http://farm3.staticflickr.com/2626/xxxxxxxxxxxx.jpg", "width": 426, "height": 640, "file_name": "xxxxxxxxx.jpg", "date_captured": "2013-11-15 02:41:42"},
        {"id": 245915, "license": 4, "coco_url": "http://images.cocodataset.org/val2017/nnnnnnnnnnnn.jpg", "flickr_url": "http://farm1.staticflickr.com/88/xxxxxxxxxxxx.jpg", "width": 640, "height": 480, "file_name": "nnnnnnnnnn.jpg", "date_captured": "2013-11-18 02:53:27"}
    ],
    "annotations": [
        {"id": 125686, "category_id": 0, "iscrowd": 0, "segmentation": [[164.81, 417.51,......167.55, 410.64]], "image_id": 242287, "area": 42061.80340000001, "bbox": [19.23, 383.18, 314.5, 244.46]},
        {"id": 1409619, "category_id": 0, "iscrowd": 0, "segmentation": [[376.81, 238.8,........382.74, 241.17]], "image_id": 245915, "area": 3556.2197000000015, "bbox": [399, 251, 155, 101]},
        {"id": 1410165, "category_id": 1, "iscrowd": 0, "segmentation": [[486.34, 239.01,..........495.95, 244.39]], "image_id": 245915, "area": 1775.8932499999994, "bbox": [86, 65, 220, 334]}
    ],
    "categories": [
        {"supercategory": "speaker","id": 0,"name": "echo"},
        {"supercategory": "speaker","id": 1,"name": "echo dot"}
    ]
}
```

## images List<a name="cd-coco-images"></a>

The images referenced by a COCO dataset are listed in the images array\. Each image object contains information about the image such as the image file name\. In the following example image object, note the following information and which fields are required to create an Amazon Rekognition Custom Labels manifest file\.
+ `id` – \(Required\) A unique identifier for the image\. The `id` field maps to the `id` field in the annotations array \(where bounding box information is stored\)\.
+ `license` – \(Not Required\) Maps to the license array\. 
+ `coco_url` – \(Optional\) The location of the image\.
+ `flickr_url` – \(Not required\) The location of the image on Flickr\.
+ `width` – \(Required\) The width of the image\.
+ `height` – \(Required\) The height of the image\.
+ `file_name` – \(Required\) The image file name\. In this example, `file_name` and `id` match, but this is not a requirement for COCO datasets\. 
+ `date_captured` –\(Required\) the date and time the image was captured\. 

```
{
    "id": 245915,
    "license": 4,
    "coco_url": "http://images.cocodataset.org/val2017/nnnnnnnnnnnn.jpg",
    "flickr_url": "http://farm1.staticflickr.com/88/nnnnnnnnnnnnnnnnnnn.jpg",
    "width": 640,
    "height": 480,
    "file_name": "000000245915.jpg",
    "date_captured": "2013-11-18 02:53:27"
}
```

## annotations \(Bounding Boxes\) List<a name="cd-coco-annotations"></a>

Bounding box information for all objects on all images is stored the annotations list\. A single annotation object contains bounding box information for a single object and the object's label on an image\. There is an annotation object for each instance of an object on an image\. 

In the following example, note the following information and which fields are required to create an Amazon Rekognition Custom Labels manifest file\. 
+ `id` – \(Not required\) The identifier for the annotation\.
+ `image_id` – \(Required\) Corresponds to the image `id` in the images array\.
+ `category_id` – \(Required\) The identifier for the label that identifies the object within a bounding box\. It maps to the `id` field of the categories array\. 
+ `iscrowd` – \(Not required\) Specifies if the image contains a crowd of objects\. 
+ `segmentation` – \(Not required\) Segmentation information for objects on an image\. Amazon Rekognition Custom Labels doesn't support segmentation\. 
+ `area` – \(Not required\) The area of the annotation\.
+ `bbox` – \(Required\) Contains the coordinates, in pixels, of a bounding box around an object on the image\.

```
{
    "id": 1409619,
    "category_id": 1,
    "iscrowd": 0,
    "segmentation": [
        [86.0, 238.8,..........382.74, 241.17]
    ],
    "image_id": 245915,
    "area": 3556.2197000000015,
    "bbox": [86, 65, 220, 334]
}
```

## categories List<a name="cd-coco-categories"></a>

Label information is stored the categories array\. In the following example category object, note the following information and which fields are required to create an Amazon Rekognition Custom Labels manifest file\. 
+ `supercategory` – \(Not required\) The parent category for a label\. 
+ `id` – \(Required\) The label identifier\. The `id` field maps to the `category_id` field in an `annotation` object\. In the following example, The identifier for an echo dot is 2\. 
+ `name` – \(Required\) the label name\. 

```
        {"supercategory": "speaker","id": 2,"name": "echo dot"}
```