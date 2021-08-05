# Understanding the manifest summary<a name="tm-debugging-summary"></a>

The manifest summary contains the following information\.
+ Error information about [Terminal manifest content errors](tm-debugging.md#tm-error-category-combined-terminal) encountered during validation\. 
+ Error location information for [Non terminal JSON line validation errors](tm-debugging.md#tm-error-category-non-terminal-errors) in the training and testing datasets\.
+ Error statistics such as the total number of invalid JSON Lines found in the training and testing datasets\. 

The manifest summary is created during training if there are no [Terminal manifest file errors](tm-debugging.md#tm-error-category-terminal)\. To get the location of the manifest summary file \(*manifest\_summary\.json*\), see [Getting the validation results](tm-debugging-getting-validation-data.md)\.

**Note**  
[Service errors](tm-debugging.md#tm-error-category-service) and [manifest file errors](tm-debugging.md#tm-error-category-terminal) are not reported in the manifest summary\. For more information, see [Terminal errors](tm-debugging.md#tm-error-categories-terminal)\. 

For information about specific manifest content errors, see [Terminal manifest content errors](tm-debugging-aggregate-errors.md)\.

## Manifest summary file format<a name="tm-manifest-summary-file"></a>

A manifest file has 2 sections, `statistics` and `errors`\.

### statistics<a name="tm-manifest-summary-statistics"></a>

`statistics` contains information about the errors in the training and testing datasets\.
+ `training` – statistics and errors found in the training dataset\. 
+ `testing` – statistics and errors found in the testing dataset\.



Objects in the `errors` array contain the error code and message for manifest content errors\. ``

The `error_line_indices` array contains the line numbers for each JSON Line in the training or test manifest that has an error\. For more information, see [Fixing training errors](tm-debugging-fixing-validation-errors.md)\. 

### errors<a name="tm-manifest-summary-errors"></a>

Errors spanning both the training and testing dataset\. For example, an [ERROR\_INSUFFICIENT\_USABLE\_LABEL\_OVERLAP](tm-debugging-aggregate-errors.md#tm-error-ERROR_INSUFFICIENT_USABLE_LABEL_OVERLAP) occurs when there is isn't enough usable labels that overlap the training and testing datasets\.

```
{
    "statistics": {
        "training": 
            {
                "use_case": String, # Possible values are IMAGE_LEVEL_LABELS, OBJECT_LOCALIZATION and NOT_DETERMINED
                "total_json_lines": Number,   # Total number json lines (images) in the  training manifest.
                "valid_json_lines": Number,   # Total number of JSON Lines (images) that can be used for training.
                "invalid_json_lines": Number, # Total number of invalid JSON Lines. They are not used for training.
                "ignored_json_lines": Number, # JSON Lines that have a valid schema but have no annotations. The aren't used for training and aren't counted as invalid.
                "error_json_line_indices": List[int], # Contains a list of line numbers for JSON line errors in the training dataset.
                "errors": [
                    {
                        "code": String, # Error code for a training manifest content error.
                        "message": String # Description for a training manifest content error.
                    }
                ]
            },
        "testing": 
            {
                "use_case": String, # Possible values are IMAGE_LEVEL_LABELS, OBJECT_LOCALIZATION and NOT_DETERMINED
                "total_json_lines": Number, # Total number json lines (images) in the manifest.
                "valid_json_lines": Number,  # Total number of JSON Lines (images) that can be used for testing.
                "invalid_json_lines": Number, # Total number of invalid JSON Lines. They are not used for testing.
                "ignored_json_lines": Number, # JSON Lines that have a valid schema but have no annotations. They aren't used for testing and aren't counted as invalid.
                "error_json_line_indices": List[int], # contains a list of error record line numbers in testing dataset.
                "errors": [
                    {
                        "code": String,   # # Error code for a testing manifest content error.
                        "message": String # Description for a testing manifest content error.
                    }
                ]  
            }
    },
    "errors": [
        {
            "code": String, # # Error code for errors that span the training and testing datasets.
            "message": String # Description of the error.
        }
    ]
}
```

## Example manifest summary<a name="tm-debugging-manifest-summary-example"></a>

The following example is a partial manifest summary that shows a terminal manifest content error \([ERROR\_TOO\_MANY\_INVALID\_ROWS\_IN\_MANIFEST](tm-debugging-aggregate-errors.md#tm-error-ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST)\)\. The `error_json_line_indices` array contains the line numbers of non\-terminal JSON Line errors in the corresponding training or testing validation manifest\.

```
{
    "errors": [],
    "statistics": {
        "training": {
            "use_case": "NOT_DETERMINED",
            "total_json_lines": 301,
            "valid_json_lines": 146,
            "invalid_json_lines": 155,
            "ignored_json_lines": 0,
            "errors": [
                {
                    "code": "ERROR_TOO_MANY_INVALID_ROWS_IN_MANIFEST",
                    "message": "The manifest file contains too many invalid rows."
                }
            ],
            "error_json_line_indices": [ 
                15,
                16,
                17,
                22,
                23,
                24,
                 .
                 .
                 .
                 .                 
                300
            ]
        },
        "testing": {
            "use_case": "NOT_DETERMINED",
            "total_json_lines": 15,
            "valid_json_lines": 13,
            "invalid_json_lines": 2,
            "ignored_json_lines": 0,
            "errors": [],
            "error_json_line_indices": [ 
                13,
                15
            ]
        }
    }
}
```