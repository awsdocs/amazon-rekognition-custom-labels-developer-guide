# Creating a project with AWS CloudFormation<a name="cloudformation"></a>

Amazon Rekognition Custom Labels is integrated with AWS CloudFormation, a service that helps you model and set up your AWS resources so that you can spend less time creating and managing your resources and infrastructure\. You create a template that describes all the AWS resources that you want, and AWS CloudFormation takes care of provisioning and configuring those resources for you\.

You can use AWS CloudFormation to provision and configure Amazon Rekognition Custom Labels projects\. 

When you use AWS CloudFormation, you can reuse your template to set up your Amazon Rekognition Custom Labels projects consistently and repeatedly\. Just describe your projects once, and then provision the same projects over and over in multiple AWS accounts and Regions\. 

## Amazon Rekognition Custom Labels and AWS CloudFormation templates<a name="working-with-templates"></a>

To provision and configure projects for Amazon Rekognition Custom Labels and related services, you must understand [AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\. Templates are formatted text files in JSON or YAML\. These templates describe the resources that you want to provision in your AWS CloudFormation stacks\. If you're unfamiliar with JSON or YAML, you can use AWS CloudFormation Designer to help you get started with AWS CloudFormation templates\. For more information, see [What is AWS CloudFormation Designer?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html) in the *AWS CloudFormation User Guide*\.

  For reference information about Amazon Rekognition Custom Labels projects, including examples of JSON and YAML templates, see [Rekognition resource type reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_Rekognition.html)\.

## Learn more about AWS CloudFormation<a name="learn-more-cloudformation"></a>

To learn more about AWS CloudFormation, see the following resources:
+ [AWS CloudFormation](http://aws.amazon.com/cloudformation/)
+ [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [AWS CloudFormation API Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/Welcome.html)
+ [AWS CloudFormation Command Line Interface User Guide](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)