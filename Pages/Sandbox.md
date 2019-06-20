# Create the project

https://foresttechnologies.atlassian.net/wiki/spaces/AWS/pages/1414070288/Service+Catalog+Usage

We assume any Front End project will need by default 1 S3 and 1 Lambda builds.

We start building following the skeleton below:
```
Parameters:
pProjectName: my-project-example
pApplicationTemplateName: my-project.cf-template.yml
pApplicationSourcePath: src/my-example-function
pApplicationBuildImage: aws/codebuild/python:3.7.1
pApplicationBuildSpec: buildspecs/buildspec-python.yml
pS3BuildEnable: true
pS3BuildSpec: buildspecs/buildspec-s3.yml
pS3BucketNamePrefix: my-project-example
pDeployment1ConfigName: ci/my-project-example-dev.eu-central-1.cf-config.json
 
Repo structure:
├── buildspecs
│   ├── buildspec-cfn.yml
│   └── buildspec-s3.yml
│   └── buildspec-python.yml
├── ci
│   └── my-project-example-dev.eu-central-1.cf-config.json
├── README.md
├── src
│   └── my-example-function
│   └── my-example-s3-package
└── templates
    └── my-project.cf-template.yml 
```

## Access AWS site

https://adfs.ecs.co.uk/adfs/ls/IdpInitiatedSignOn.aspx?loginToRp=urn:amazon:webservices

Credentials are: ADFS-ConnectSandbox-Pipeline-SolDev
Region: make sure you are in Frankfurt

## Services
Choose `service catalog`

Product: in the product list page, select ECS CS Pipeline
In the Product Detail page, click Launch Product

## Product Version

Name: Make it cool, life is too boring already.


## Parameters

### CodePipeline Configuration
Project Name: same as product name

Leave blank the rest

### S3 Build Configuration

Enable S3 Application build: set to true
S3 Deployment Bucket Name Prefix: same as Project name 


### CloudFormation Build Configuration

CloudFormation Deployment template file: use `templates/<project-name>.cf-template.yml`


### Deployment 1 Configuration

Deployment 1 config file: `ci/PROJECT-NAME.ENV.AWS-REGION.cf-config.json` e.g. `ci/my-project-example-dev.eu-central-1.cf-config.json`

Click next

## TagOptions

Leave empty

## Notifications

Leave unchecked

## Launch

Well, launch.

## Output

If the provision succeeded, you will see the details as outputs.
You need to copy the URL of the code pipeline URL, e.g. https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/equestria-repo 	


# Write some code

# Policies

## To make the s3 bucket available to read: public access

```yaml
rWebsiteBucketPolicy:
  Type: AWS::S3::BucketPolicy
  Metadata:
    cfn_nag:
      rules_to_suppress:
        - id: F16
          reason: "This is a public facing S3 bucket and should be permitted."
  Properties:
    Bucket: !Ref rWebsiteS3Bucket
    PolicyDocument:
      Statement:
        - Sid: SetupCloudFront
          Action:
            - s3:GetObject
          Effect: Allow
          Resource:
            !Join ["", ["arn:aws:s3:::", !Ref pS3DeploymentBucketName, "/*"]]
          Principal:
            AWS: "*"
```