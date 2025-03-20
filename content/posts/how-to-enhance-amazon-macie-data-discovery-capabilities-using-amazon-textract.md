---
title: "How to enhance Amazon Macie data discovery capabilities using Amazon Textract"
date: 2025-01-07
---

Amazon Macie is a managed service that uses machine learning (ML) and deterministic pattern matching to help discover sensitive data that’s stored in Amazon Simple Storage Service (Amazon S3) buckets. Macie can detect sensitive data in many different formats, including commonly used compression and archive formats. However, Macie doesn’t support the discovery of sensitive data within images, audio, video, or other types of multimedia content. Customers often ask how to effectively detect whether there’s sensitive data in images. This can be a significant challenge for organizations, especially those operating in highly regulated industries with strict data protection requirements.

In this post, we show you how to gain visibility of sensitive data embedded in images that are stored within your S3 buckets by adding an additional conversion layer to extract image-based data into a format supported by Macie. The solution also uses the recommended set of managed identifiers and custom data identifiers supported by Macie to cover most use cases.

## Solution overview

In this section, we walk through the components of the solution. The solution is deployed using AWS Serverless Application Model (AWS SAM), which is an open source framework for building serverless applications. AWS SAM helps to organize related components and operate on a single stack. When used together with the AWS SAM CLI, it’s a useful tool for developing, testing, and building serverless applications on AWS. We provided an AWS SAM template that you can use to set up the required services and AWS Lambda functions. Figure 1 illustrates the architecture of the solution.

![Figure 1: Solution architecture and workflow](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/image1-2.jpeg)

Figure 1: Solution architecture and workflow

The solution workflow is as follows:

1. A user uploads images that might contain sensitive data into the S3 bucket.
2. After you have verified that potentially sensitive data has been uploaded into the S3 bucket, you can manually invoke the Lambda function `textract-trigger` to start the process. This function calls Amazon Textract asynchronously to process files in the S3 bucket with filename extensions such as .png, .jpg, and .jpeg. Amazon Textract creates a job for each image and extracts the text found in each image.
3. Because the operation is asynchronous, the job ID and status of each call is stored in an Amazon DynamoDB table to track the status of jobs and make sure that all of the jobs are completed before Macie is triggered to scan the S3 bucket.
4. The resulting JSON file from the Amazon Textract job is stored within the same S3 bucket as the original image.
5. For each analysis job, Amazon Textract sends a job completion notification to the registered Amazon Simple Notification Service (Amazon SNS) topic `AmazonTextractJobSNSTopic`. The Lambda function `macie-trigger` is subscribed to the SNS topic and triggered every time an SNS message is received for a completed Amazon Textract analysis job.
6. Further post-processing is done by the Lambda function `macie-trigger` to extract the values from the JSON file into a text file. This text file is then uploaded into the same S3 bucket.
7. The function then checks for other in-progress Amazon Textract jobs in the DynamoDB table. If there are pending jobs, the function exits and waits to be triggered again.
8. After all of the Amazon Textract jobs are marked as complete in the DynamoDB table, the Lambda function `macie-trigger` creates a Macie classification job.
9. Macie then scans the bucket for sensitive data based on managed identifiers and your custom data identifiers.
10. Macie will continuously publish the classification job status to Amazon CloudWatch Logs.
11. It might take some time to scan all the files in the S3 bucket, and you will be notified through SNS email when the Macie job is completed. The Lambda function `MacieCompletedSNSLambda` will filter for completed job status and send an email notification using the SNS topic `MacieSnsTopic`.

When deploying the solution, you can specify an existing S3 bucket in your AWS account that’s already storing data that might be sensitive or deploy a new S3 bucket as part of the setup. If you specify an existing S3 bucket, make sure that there are no additional statements in the bucket policy or KMS key policy that will deny the relevant solution components access to the S3 bucket. If no existing S3 bucket is specified, a new S3 bucket will be created with the name `s3-with-sensitive-data-_<account-id>_-_<random-string>_`.

## Prerequisites

Before deploying the solution, make sure the following prerequisites are in place.

- Enable Macie in your account. For instructions, see Getting Started with Amazon Macie.
- Determine the regular expression (regex) pattern for sensitive textual data that you want Macie to detect. This will allow you to create custom data identifiers that complement the managed data identifiers provided by Macie. For more information, see Building custom data identifiers in Amazon Macie. There’s an example in the pre-deployment steps that you can follow, with the sample images that come with the solution.
- Make sure that you have the permissions to deploy the AWS services detailed in the solution: Lambda, Amazon S3, Amazon Textract, Amazon SNS, Macie, Amazon CloudWatch, and DynamoDB.
- Install the AWS SAM CLI, which you will use to deploy the solution. To learn more about how AWS SAM works, see The AWS SAM project and AWS SAM template.

## Pre-deployment steps

With the prerequisites in place, you need to set up one or more custom identifiers through the AWS Management Console for Macie before you can deploy the solution. Use the following steps to set up an example custom identifier for the images provided in this post.

**To set up custom identifiers:**

1. Navigate to the Amazon Macie console.
2. Choose **Custom data identifiers** in the navigation pane, and then choose **Create**.
3. Enter a name and description for the custom identifier, such as the following examples:
    1. **Name:** `Singapore NRIC Number`
    2. **Description:** `This expression can be used to find or validate a Singapore NRIC Number that begins with the character S, F, T, or G, followed by seven digits and ending with any character from A to Z.`
4. For **Regular expression**, enter: `[SFTG]d{7}[A-Z]`.
5. For **Keywords**, enter: `Singapore,Identity, Card`.  
    Keywords are important because they can help to improve the accuracy of the detection and refine the results.
6. Leave the other fields as default and choose **Submit**.
7. Navigate to the newly created custom identifier and note the ID. This ID is required as an input when deploying the AWS SAM solution.  
    
    ![Figure 2: ID of a newly created Macie custom identifier](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/image2-2.jpeg)
    
    Figure 2: ID of a newly created Macie custom identifier
    

## Deploy the solution

With the prerequisites in place and pre-deployment steps complete, you’re ready to deploy the solution.

**To deploy the solution:**

1. Open a CLI window, navigate to your preferred local directory and run `git clone https://github.com/aws-samples/enhancing-macie-with-textract`.
    1. Navigate to this directory by using `cd enhancing-macie-with-textract`.
    2. Run `sam deploy --guided` and follow the step-by-step instructions to indicate the deployment details such as the desired CloudFormation stack name, AWS Region, and other details. The following are descriptions of some of the requested parameters:
        - **ExistingS3BucketName**: This is the name of the S3 bucket that you want the solution to scan. This is an optional parameter. If it’s left blank, the solution will create an S3 bucket for you to store the objects that you want to scan.
        - **MacieCustomCustomIdentifierIDList**: This is the ID that you noted in the final pre-deployment step. Use this field to enter a list of custom identifiers for Macie to detect with. If there is more than one ID, each ID should be separated by a comma (for example, `59fd2814-0ba8-41cc-adb2-1ffec6a0bb3c, 665cf948-ea30-42df-9f63-9a858cbfe1a8`).
        - **EmailAddress**: This is the email address that you want Amazon SNS email notifications to be sent to when a Macie job is complete.
        - **MacieLogGroupExists**: This checks if you have an existing Macie CloudWatch Log Group (`/aws/macie/classificationjobs`). If this is your first time running a Macie job, enter `No` or `n`. Otherwise, enter `Yes` or `y`.
2. When completed, a confirmation request will be presented for the creation of the required resources. AWS SAM creates a default S3 bucket to store the necessary resources and then proceeds to the deployment prompt. Enter `y` to deploy and wait for deployment to complete.
3. After deployment is complete, you should see the following output: `Successfully created/updated stack – {StackName} in {AWSRegion}`. You can review the resources and stack in the CloudFormation console.  
    
    ![Figure 3: CloudFormation console of the deployed stack](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/image3-3.jpeg)
    
    Figure 3: CloudFormation console of the deployed stack
    
4. An email will be sent from no-reply@sns.amazonaws.com to the email address that you entered in step 3. Choose **Confirm subscription** to allow SNS to send you Macie job completion emails.  
    
    ![Figure 4: Sample email from Amazon SNS for subscription confirmation](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/image4-2.jpeg)
    
    Figure 4: Sample email from Amazon SNS for subscription confirmation
    

## Test the solution

With the solution deployed, use a set of sample images to verify that it can detect sensitive data within images.

**To test the solution:**

1. Use the Amazon S3 console to navigate to the bucket you specified during deployment. If you didn’t specify an S3 bucket to scan, look for a new bucket named `s3-with-sensitive-data-_<account-id>_-_<random-string>_`.
2. In your project directory, there are sample images in `sample-images.zip`. Unzip the file and upload the sample images into the S3 bucket. The sample images include a US driver’s license, social security card, passport, and a Singapore National Registration Identity Card (NRIC).
3. Navigate to the AWS Lambda console and select the `{StackName}-TextractTriggerLambda-_<random-string>_` function.
4. Choose the **Test** tab and then choose **Test** to start the automated sensitive data discovery process for the uploaded images.  
    
    ![Figure 5: Trigger an Amazon Textract scan on all images in the S3 bucket](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/image5-2.jpeg)
    
    Figure 5: Trigger an Amazon Textract scan on all images in the S3 bucket
    
5. The whole process will take about 15 minutes to complete. You will receive an email notification after the Macie scan is completed.  
    
    ![Figure 6: Sample email from Amazon SNS for Macie job completion](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/image6-3.jpeg)
    
    Figure 6: Sample email from Amazon SNS for Macie job completion
    
6. Navigate to the Amazon Macie console and select **Jobs** in the navigation pane. You should see the job **Scan for \[number of\] objects \[datetime stamp\]** that matches the job name shown in the email notification.
7. In the details panel, choose **Show results** button and then choose **Show findings**.  
    
    ![Figure 7: Show Macie data discovery job findings](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/image7-2.png)
    
    Figure 7: Show Macie data discovery job findings
    
8. You will see the findings related to the Macie sensitive data discovery job ID that you selected.  
    
    ![Figure 8: Findings from the data discovery job](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/30/Figure-8-r.png)
    
    Figure 8: Findings from the data discovery job
    

## Understanding the findings

In this section, we take a closer look at each finding.

1. In the console, look in the **Resources affected** column for the finding that ends with **singapore-pink-nric-postprocessed.txt** and select it. The finding type **SensitiveData:S3Object/CustomIdentifier** means that the resource contains text that matches the detection criteria of a custom data identifier. The other finding types in this example are from managed data identifiers. See Types of sensitive data findings for more information about Macie finding types.
2. In the finding information panel, you can also see:
    1. In the **Overview** section, the resource indicates which resource contains sensitive data. The resource identifies the text file; however, you can identify the original image file because it has the same object name (other than the file type).
    2. In the **Custom data identifiers** section, you can see the type of sensitive data found. In this case, the finding involves data that matches the regex of a Singapore NRIC.

By using this solution, you can use Macie to detect sensitive data within the images in your S3 bucket and which images each finding corresponds to.

## Using the solution

In this post, you have configured a single custom data identifier and a recommended set of managed identifiers. However, you can create and use multiple custom data identifiers in the solution by providing them as a comma-separated list, as mentioned in step 3 of **Deploy the solution**.

This solution has been designed to enable sensitive data discovery of text in image objects within a single S3 bucket. To expand the scope to include multiple S3 buckets, some additional code and permission changes are required to allow the Lambda functions to process and access multiple existing S3 buckets.

It’s important to note the language capabilities of Amazon Textract. Amazon Textract can extract printed text and handwriting from the standard English alphabet and ASCII symbols. Currently, Amazon Textract supports extraction in English, German, French, Italian, and Portuguese. For more information on what textual information Amazon Textract can identify, see Amazon Textract FAQs.

## Clean up the resources

To clean up the resources that you created for this example:

1. If you didn’t set up the scan on your own S3 bucket, empty the S3 bucket that was created as part of the solution. Open the Amazon S3 console, search for the bucket name `s3-with-sensitive-data-_<account-id>_-_<random-string>_` and choose **Empty**.
2. Use one of the following methods to delete the CloudFormation stack:
    - Use the CloudFormation console to delete the stack.
    - Use AWS SAM CLI to run `sam delete` in your terminal. Follow the instructions and enter y when prompted to delete the stack.

## Conclusion

In this post, you learned how to enhance the capabilities of Amazon Macie to conduct sensitive data discovery within image files. With this solution, you can extend the benefits of Amazon Macie beyond structured file formats.

If you want to extend the benefits of Amazon Macie to scan your databases for sensitive data, you might find these blog posts useful:

- Enabling data classification for Amazon RDS database with Macie
- Detecting sensitive data in DynamoDB with Macie.

If you have feedback about this post, submit comments in the **Comments** section. If you have questions about this post, start a new thread on the AWS Security, Identity, & Compliance re:Post or contact AWS Support.

![ZhiWei Huang](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2023/08/14/ZhiWei-Huang.jpg)

### ZhiWei Huang

ZhiWei is a Financial Services Solutions Architect at AWS. He works with FSI customers across the ASEAN region, providing guidance for establishing robust security controls and networking foundations as customers build on and scale with AWS. Outside of work, he finds joy in travelling the world and spending quality time with his family.

![Edmund Yeo](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/Edmund-Yeo.jpg)

### Edmund Yeo

Edmund is a Security Solutions Architect based in Singapore. He works with customers across ASEAN region, helping them to continually raise the security bar and posture for them to innovate rapidly and securely. Outside of work, Edmund enjoys having a cup of coffee, seeing the world, and bouldering.

![Ying Ting Ng](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/12/26/Ying-Ting-Ng.png)

### Ying Ting Ng

Ying Ting is an Associate Security Solutions Architect at AWS, where she supports ASEAN growth customers in scaling securely on the cloud. She also advises on architectural best practices to help customers meet industry compliance standards. An active member in Amazon Women in Security, Ying Ting shares insights on making an impact as an early-career cybersecurity professional.

Go to Source
