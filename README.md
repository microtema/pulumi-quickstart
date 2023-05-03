# Pulumi - Infrastructure as Code

## Setup 

### Install Pulumi

#### MacOS

```
brew install pulumi/tap/pulumi
```

#### Install Language Runtime

```
brew install node
```

### Configure Pulumi to access your AWS account

```
export AWS_ACCESS_KEY_ID=<YOUR_ACCESS_KEY_ID>
export AWS_SECRET_ACCESS_KEY=<YOUR_SECRET_ACCESS_KEY>
```

### Create new project

```
mkdir quickstart && cd quickstart 
pulumi new aws-typescript
```

The pulumi new command creates a new Pulumi project with some basic scaffolding based on the cloud and language specified.

### Review the New Project

Let’s review some of the generated project files:

* Pulumi.yaml defines the project.
* Pulumi.dev.yaml contains configuration values for the stack you just initialized.
* index.ts is the Pulumi program that defines your stack resources.

```
import * as pulumi from "@pulumi/pulumi";
import * as aws from "@pulumi/aws";
import * as awsx from "@pulumi/awsx";

// Create an AWS resource (S3 Bucket)
const bucket = new aws.s3.Bucket("my-bucket");

// Export the name of the bucket
export const bucketName = bucket.id;
```

This Pulumi program creates a new S3 bucket and exports the name of the bucket.

```
export const bucketName = bucket.id;
```

### Deploy the Stack

```
pulumi up
```

This command evaluates your program and determines the resource updates to make. First, a preview is shown that outlines the changes that will be made when you run the update:

```
Previewing update (dev):

     Type                 Name            Plan
 +   pulumi:pulumi:Stack  quickstart-dev  create
 +   └─ aws:s3:Bucket     my-bucket       create

Resources:
    + 2 to create

Do you want to perform this update?
> yes
  no
  details
```

Once the preview has finished, you are given three options to choose from. Choosing details will show you a rich diff of the changes to be made. Choosing yes will create your new S3 bucket in AWS. Choosing no will return you to the user prompt without performing the update operation.

```
Do you want to perform this update? yes
Updating (dev):

     Type                 Name            Status
 +   pulumi:pulumi:Stack  quickstart-dev  created (4s)
 +   └─ aws:s3:Bucket     my-bucket       created (2s)

Outputs:
    bucketName: "my-bucket-58ce361"

Resources:
    + 2 created

Duration: 5s
```

Remember the output you defined in the previous step? That stack output can be seen in the Outputs: section of your update. You can access your outputs from the CLI by running the pulumi stack output [property-name] command. For example you can print the name of your bucket with the following command:

```
pulumi stack output bucketName
```

### Destroy the Stack

Now that you’ve seen how to deploy changes to our program, let’s clean up and tear down the resources that are part of your stack.

To destroy resources, run the following:

```
pulumi destroy
```