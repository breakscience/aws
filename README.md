# Lucee on EKS with RDS SQL Server and S3 - AWS CloudFormation

This CloudFormation template provisions an Amazon EKS (Elastic Kubernetes Service) cluster, an RDS SQL Server instance, and an S3 bucket to host a Lucee server. It deploys the necessary resources, including IAM roles, security groups, and networking, to run Lucee in a Kubernetes environment on AWS.

## Features

- **Amazon EKS Cluster**: Deploys an EKS cluster and worker nodes to run the Lucee server in a Kubernetes environment.
- **Amazon RDS (SQL Server)**: Provisions a highly available RDS SQL Server instance with Multi-AZ for backend storage.
- **Amazon S3 Bucket**: Creates an S3 bucket to store the `index.cfm` file, which is mounted to Lucee as its home directory.
- **Security Groups**: Configures security groups to allow communication between EKS worker nodes, the EKS control plane, and the RDS instance.
- **IAM Roles and Policies**: Provides necessary IAM roles for the EKS cluster, worker nodes, and Lambda functions.
- **Automated S3 Deployment**: Uses a Lambda function to upload an `index.cfm` file to the S3 bucket during stack creation.

## Prerequisites

Before deploying this CloudFormation template, ensure the following prerequisites are met:

### 1. AWS Account
You will need access to an AWS account with the following services enabled:

- Amazon EKS
- Amazon RDS
- Amazon S3
- AWS CloudFormation
- Amazon EC2

### 2. AWS CLI or AWS Management Console
You can either deploy this template using the AWS CLI or through the AWS Management Console.
- [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- [AWS Management Console](https://aws.amazon.com/console/)

### 3. EC2 Key Pair
You will need an existing EC2 Key Pair to SSH into the EKS worker nodes (if needed). If you don’t have a key pair, you can create one in the EC2 Dashboard under **Key Pairs**.

### 4. IAM Permissions
Ensure that the user or role deploying the CloudFormation stack has the following permissions:

- `cloudformation:CreateStack`
- `ec2:*` (for VPC, subnets, security groups, and EC2 instances)
- `eks:*` (for EKS cluster and node group creation)
- `rds:*` (for provisioning the RDS instance)
- `s3:*` (for creating the S3 bucket and uploading files)
- `lambda:*` (for creating Lambda functions)

### 5. Supported AWS Region
Ensure that you are deploying the stack in a supported region for Amazon EKS, RDS, and S3. This template is configured for `us-west-2` but can be adapted for other regions.

## Deployment Instructions

### 1. Clone or Download the Repository

Clone the GitHub repository to your local machine or download the CloudFormation template file (`ilovelucee.yaml`):

	git clone https://github.com/breakscience/lucee-eks-rds-s3.git

Navigate to the project directory:

	cd lucee-eks-rds-s3

### 2. Customize Parameters

The CloudFormation template comes with parameters for customization, including:

- DBUsername: Username for the RDS SQL Server.
- DBPassword: Password for the RDS SQL Server (use a secure password).
- VPCName: Name of the VPC.
- ClusterName: Name of the EKS Cluster.

You can modify these parameters directly in the `ilovelucee.yaml` file or pass them as parameters during deployment.

### 3. Deploy the CloudFormation Stack

You can deploy the CloudFormation stack using either the AWS CLI or the AWS Management Console.

### AWS CLI Deployment

Make sure you have the AWS CLI configured with the correct region and credentials:

	aws configure

Then, deploy the stack:

	aws cloudformation create-stack \
	  --stack-name lucee-eks-rds-s3-stack \
	  --template-body file://cloudformation.yaml \
	  --parameters ParameterKey=DBUsername,ParameterValue=your-username \
	               ParameterKey=DBPassword,ParameterValue=your-password \
	  --capabilities CAPABILITY_NAMED_IAM

Replace your-username and your-password with your desired RDS SQL Server credentials.
The stack may take several minutes to create.

### AWS Management Console Deployment

	1.	Go to the CloudFormation Console.
	2.	Click Create Stack and upload the ilovelucee.yaml file.
	3.	Enter the stack parameters, such as DBUsername, DBPassword, and other values.
	4.	Click Next and proceed with the default options.
	5.	Review the stack and click Create Stack.
	6.	Wait for the stack to be created. You can monitor progress from the Events tab in the CloudFormation console.
   
### 4. Post-Deployment

Once the stack is deployed, you can retrieve the following outputs from the CloudFormation console or AWS CLI:

- LuceeServiceURL: The URL to access the Lucee server running on EKS.
- RDSInstanceEndpoint: The endpoint of the RDS SQL Server.
- S3BucketName: The name of the S3 bucket where index.cfm is stored.

### 5. Accessing the Lucee Server

Once the deployment is complete, you can access the Lucee server by navigating to the LuceeServiceURL in a browser. You should see the output of the index.cfm file, which will display “Hello World”.

### 6. Managing the EKS Cluster

To interact with the EKS cluster, you can use `kubectl`. Make sure you have the AWS CLI and kubectl configured for EKS.

Update the kubeconfig file for the EKS cluster:

	aws eks update-kubeconfig --region us-west-2 --name your-cluster-name

You can now run `kubectl` commands to interact with the Kubernetes resources on the EKS cluster.

## Cleanup

To delete the stack and all associated resources:

	1.	Go to the CloudFormation Console or use the AWS CLI.
	2.	Select the stack and click Delete.

Alternatively, using the AWS CLI:

	aws cloudformation delete-stack --stack-name lucee-eks-rds-s3-stack

This will delete the EKS cluster, RDS instance, S3 bucket, and all other resources created by the stack.

### Troubleshooting

If the stack fails to create or the instances fail to join the Kubernetes cluster:

	1.	Check CloudWatch Logs for Lambda functions and EC2 worker nodes.
	2.	Ensure the subnets are public and have Internet access via the Internet Gateway.
	3.	Verify IAM roles are properly assigned to the EKS cluster and worker nodes.
	4.	Verify Security Group rules to ensure proper communication between the EKS control plane, worker nodes, and RDS.

### Contributing

Feel free to submit issues or pull requests if you find any bugs or improvements to be made.

### Author

José Gomez

## License

This project is licensed under the MIT License - see the LICENSE file for details.
