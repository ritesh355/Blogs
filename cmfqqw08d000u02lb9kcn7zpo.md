---
title: "🚀 Day 51: Introduction to Terraform | My DevOps Journey"
seoTitle: "Terraform Basics with aws"
seoDescription: "Discover Terraform and Infrastructure as Code basics in this introductory guide for beginners and experienced developers exploring cloud automation"
datePublished: Fri Sep 19 2025 11:17:09 GMT+0000 (Coordinated Universal Time)
cuid: cmfqqw08d000u02lb9kcn7zpo
slug: day-51-introduction-to-terraform-my-devops-journey
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/3iT3dnmblGE/upload/b4b2813aa4f5c94c792ebae7a23fd74a.jpeg
tags: aws, automation, terraform, iac

---

Welcome our Terraform series! If you're new to infrastructure management or looking to automate your cloud resources, Terraform is a powerful tool to get started with. In this post, we'll cover the fundamentals: what Infrastructure as Code (IaC) means, Terraform's architecture, providers, and state files. I'll explain each concept in detail with examples to make it easy to understand. By the end, you'll have a solid foundation to build upon.

This series is designed for beginners, but even experienced devs might find some useful insights. Let's dive in!

## What is Infrastructure as Code (IaC)?

**Infrastructure as Code (IaC)** is the practice of managing and provisioning infrastructure (servers, networks, databases, etc.) using **code instead of manual processes**.

It allows infrastructure to be:  
✅ Automated  
✅ Version-controlled (just like application code)  
✅ Reproducible and consistent

### Why Use IaC?

* **Consistency**: Every deployment is identical, reducing "it works on my machine" issues.
    
* **Version Control**: Track changes using tools like Git, allowing rollbacks if something goes wrong.
    
* **Automation**: Integrate with CI/CD pipelines for faster, error-free deployments.
    
* **Collaboration**: Teams can review infrastructure changes via pull requests.
    
* **Scalability**: Easily replicate environments (dev, staging, prod) with minimal effort.
    

Without IaC, you'd manually click through cloud consoles (e.g., AWS Management Console) to create resources, which is prone to human error and hard to audit.

There are two main approaches to IaC:

* **Declarative (What you want)** → Terraform, CloudFormation, Kubernetes
    
* **Imperative (How to do it)** → Bash, AWS CLI
    
* **Hybrid** → Ansible, Puppet, Chef
    

Terraform follows the **Declarative** model.

## 🔹 What is Terraform?

Terraform is an **open-source tool by HashiCorp** that lets you define cloud infrastructure in code using **HCL (HashiCorp Configuration Language)**.

With Terraform, you can:

* Create, update, and delete cloud resources (AWS, Azure, GCP, etc.)
    
* Manage infrastructure state
    
* Automate deployments in a repeatable way
    

### Example of IaC in Action

Imagine you're setting up a simple web server on AWS. Without IaC, you'd log into the AWS console, create an EC2 instance, configure security groups, and attach storage—repeating this for every environment.

With IaC using Terraform, you write a configuration file like this:

```plaintext
# main.tf
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"  # Amazon Linux 2 AMI
  instance_type = "t2.micro"
}
```

Running `terraform apply` provisions the server automatically. You can commit this file to Git for version history.

IaC tools like Terraform, Ansible, or Pulumi make this possible, but Terraform stands out for its declarative approach (you describe *what* you want, not *how* to achieve it).

## Terraform Architecture

Terraform, developed by HashiCorp, is an open-source IaC tool that allows you to build, change, and version infrastructure safely and efficiently. It supports multiple cloud providers (AWS, Azure, Google Cloud, etc.) and on-premises solutions.

### Core Components of Terraform Architecture

Terraform's architecture is modular and extensible. Here's a breakdown:

1. **Terraform CLI**: The command-line interface you install on your machine. Commands like `terraform init`, `terraform plan`, and `terraform apply` drive the workflow.
    
2. **Configuration Files**: Written in HashiCorp Configuration Language (HCL) or JSON. These files define your infrastructure (e.g., [`main.tf`](http://main.tf), [`variables.tf`](http://variables.tf)).
    
3. **Providers**: Plugins that interact with external APIs (e.g., AWS, Kubernetes). Terraform downloads providers during `terraform init`.
    
4. **State Management**: Tracks the current state of your infrastructure in a state file (more on this later).
    
5. **Backend**: Where the state is stored (local file, S3, Consul, etc.) for collaboration and security.
    
6. **Modules**: Reusable packages of Terraform configurations to avoid repetition.
    

### Terraform Workflow

The typical workflow is:

* **Write**: Create configuration files.
    
* **Init**: Run `terraform init` to download providers and modules.
    
* **Plan**: `terraform plan` to preview changes.
    
* **Apply**: `terraform apply` to execute changes.
    
* **Destroy**: `terraform destroy` to tear down resources.
    

Terraform uses a **declarative** model: You specify the desired end state, and Terraform figures out the steps to get there. It builds a dependency graph internally to apply changes in the right order.

### Example Architecture in Practice

Suppose you're deploying a VPC and an EC2 instance on AWS. Your config might look like:

```plaintext
# terraform.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# main.tf
resource "aws_vpc" "my_vpc" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_instance" "my_instance" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  vpc_security_group_ids = [aws_vpc.my_vpc.id]  # Dependency on VPC
}
```

Terraform automatically handles the order: VPC first, then instance.

## Providers in Terraform

Providers are the bridge between Terraform and the services you want to manage. Each provider is a plugin that implements Terraform's resource types and data sources for a specific platform.

### Key Features of Providers

* **Extensibility**: Terraform has over 1,000 official providers for clouds, SaaS, and hardware.
    
* **Versioning**: Specify versions in your config to ensure compatibility.
    
* **Configuration**: Providers often need credentials (e.g., API keys) passed via environment variables or config blocks.
    
* **Resources and Data Sources**: Resources create/manage objects (e.g., `aws_instance`), while data sources query existing ones (e.g., fetch an AMI ID).
    

Providers are downloaded from the Terraform Registry during `terraform init`. Official ones are maintained by HashiCorp or partners.

### Example Using a Provider

To use the AWS provider:

1. Add to your config:
    

```plaintext
provider "aws" {
  region     = "us-east-1"
  access_key = var.aws_access_key  # Use variables for secrets
  secret_key = var.aws_secret_key
}
```

2. Define a resource:
    

```plaintext
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-unique-bucket-name"
  acl    = "private"
}
```

Run `terraform init` to install the provider, then `apply` to create the bucket. You can mix providers, e.g., AWS + Google Cloud in one config.

## State Files in Terraform

The state file is Terraform's "memory"—a JSON file that maps your configuration to real-world resources. It records metadata like resource IDs, attributes, and dependencies.

### Why State Files Matter

* **Tracking Changes**: During `plan`, Terraform compares config to state to detect drifts.
    
* **Collaboration**: Remote backends (e.g., S3 with locking) allow teams to share state safely.
    
* **Sensitivity**: States can contain secrets, so never commit them to Git—use `.gitignore`.
    
* **Locking**: Prevents concurrent modifications in team environments.
    

By default, state is stored locally as `terraform.tfstate`. For production, use remote backends like:

```plaintext
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-west-2"
  }
}
```

### Example with State Files

After applying a config for an EC2 instance, your state file might include:

```json
{
  "version": 4,
  "terraform_version": "1.9.5",
  "serial": 1,
  "lineage": "abcd1234",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "aws_instance",
      "name": "web_server",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 1,
          "attributes": {
            "id": "i-0123456789abcdef0",
            "ami": "ami-0c55b159cbfafe1f0",
            // ... more attributes
          }
        }
      ]
    }
  ]
}
```

If you update the instance type in config and run `plan`, Terraform uses this state to show only the diff.

To view state: `terraform show`. To refresh: `terraform refresh` (updates state without changes).

* ## 👨‍💻 Author
    
    **Ritesh Singh**  
    🌐 [LinkedIn](https://www.linkedin.com/in/ritesh-singh-092b84340/) | 📝 [Hashnode](https://ritesh-devops.hashnode.dev/) | [GitHub](https://github.com/ritesh355/Devops-journal)