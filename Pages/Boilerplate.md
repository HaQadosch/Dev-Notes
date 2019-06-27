```bash
> git clone https://github.ecs-digital.co.uk/connect/ecs-cx-skeleton-s3.git
> mv ecs-cx-skeleton canterlot
```

Go to Amazon Sign In
Chose the ADFS-ConnectSandbox-Pipeline-SolDev account
Go to Service Catalog
In Product List:

  * ECS CC Pipeline
  * Launch Product
  * Name: Canterlot and Select V1, Next
  
In Parameters:

  * Project Name: canterlot
  * S3 build configuration:
    * enable: true
  * CloudFormation:
    * template file templates/PROJECT-NAME.cf-template.yml
    * rename the file
  * Deployment 1 Configuration:
    * config file ci/PROJECT-NAME-dev.eu-central-1.cf-config.json
    * rename the file
    * rename the parameters inside the file

Parameters:
pProjectName: my-project-example
pS3BuildEnable: true
pS3BuildSpec: buildspecs/buildspec-s3.yml
 
Repo structure:
├── buildspecs
│   ├── buildspec-cfn.yml
│   └── buildspec-s3.yml
├── ci
│   └── my-project-example-dev.eu-central-1.cf-config.json
├── README.md
├── src
│   └── my-example-s3-package
└── templates
    └── my-project.cf-template.yml


Click Next
Add a tag?

Wait for the project to build // Show another one with the commit url

```bash
> git remote add aws https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/canterlot-repo
```

Go build the App
```bash
> cd src/react-test
> npm i
```

Show the package.json

Storybook example: http://react.carbondesignsystem.com/?path=/story/buttons--default

```bash
> npm t
> npm start
> npm run storybook // no npx
> npx plop rcts MyComp // no npm
```

Get saml-auth at https://foresttechnologies.atlassian.net/wiki/spaces/AWS/pages/1429307518/ECS+AWS+SAML+API+Authentication+Tool
Activate your samlauth and push
```bash
> cd ~ && ./saml_aws_auth.pex 
> git add, push
```

Go to AWS Pipeline and look at the build.


## Working with CodePipeline

### Renaming

#### Repo folder
`ecs-cx-skeleton-s3/` to `my-project/`

#### Deployment configuration file
`ci/ecs-cx-skeleton-s3-dev.eu-central-1.cf-config.json` to `ci/my-project-dev.eu-central-1.cf-config.json`

Inside the file
```json
    "pS3WebsiteHomePath": "/ecs-cx-skeleton-s3-s3-build-dev/",
```
Replace with
```json
    "pS3WebsiteHomePath": "/my-project-s3-build-dev/",
```

#### CloudFormation file
`templates/ecs-cx-skeleton-s3.cf-template.yml` to `templates/my-project.cf-template.yml`

