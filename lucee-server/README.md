Lucee Server on AWS with SQL Server RDS

This project provides an AWS CloudFormation template that deploys a Lucee Server on an Amazon EC2 instance, connected to an Amazon RDS SQL Server database within a secure VPC. The setup includes:

	•	A fully-configured Lucee Server running on an EC2 instance with automatic installation via UserData.
	•	Amazon RDS SQL Server deployed within a VPC with multi-AZ support for high availability.
	•	Integration with Amazon S3 for file storage and IAM roles for secure access between services.
	•	Security Groups to allow HTTP/HTTPS traffic and secure database connections.
	•	CloudWatch Monitoring for detailed EC2 and RDS performance metrics.
	•	Best practices for security with parameterized inputs for sensitive information like database credentials.

	Note: This template is designed to meet Amazon RDS requirements by ensuring that the DB subnet group spans at least two Availability Zones (AZs) for high availability and redundancy. This is critical for maintaining fault tolerance and minimizing downtime.

Key Features:

	•	EC2 instance running Lucee, a powerful open-source CFML engine.
	•	SQL Server Express RDS instance for database storage.
	•	Automatic deployment of a sample index.cfm file, returning “Hello World”.
	•	Detailed CloudWatch monitoring of EC2 and RDS resources.

Get started by deploying this CloudFormation template to your AWS environment to quickly spin up a Lucee and SQL Server-based web application.