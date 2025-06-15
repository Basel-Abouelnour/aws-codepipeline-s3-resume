# AWS CodePipeline and S3 static website hosting
>Region: `UAE me-central-1` because I'll probably forget

This is a simple aws codepipeline automation to continuously deploy any changes in the source code to the [static website hosted in AWS S3 bucket](http://basel-resume-s3.s3-website.me-central-1.amazonaws.com). 

1. [Create a S3 bucket](#creaet-a-s3-bucket)
2. [Create a CodePipeline](#create-a-codepipeline)

## Creaet a S3 bucket
1. create an S3 from the AWS console and untick the `Block all public access` checkbox, you can keep all the other defaults.
2. Go to properties, scroll all the way down to `static website hosting` and edit it,  choose to enable and type your html main page.
3. then go to permissions and edit the bucket policy to the as following:
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "Statement1",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::<bucket-name>/*"
            }
        ]
    }
    ```
    > this is a bucket policy to allow any public access to  all objects in the bucket, you can customize it more using the policy generator.
## Create a CodePipeline
> steps may change slightly in the console over time.
1. create a pipeline and choose to build a custom pipeline
2. choose to create a new service role
3. choose the source provider (GitHub in my case) and do the authenitcation steps and choose your repository
4. will skip the build and test stages, because there's no backend code in my case.
5. deploy to the previously created S3 bucket, and enable `Extract file before deploy` then create the pipeline.

Finally to access the website you can copy the `Bucket website endpoint` from the properties tab in you S3 bucket page and it will be something like this http://basel-resume-s3.s3-website.me-central-1.amazonaws.com