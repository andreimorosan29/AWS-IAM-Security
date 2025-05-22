# AWS-IAM-Security
This project is to demonstrate the basic of IAM security through usage of rules and policies.

# Main Goals:
💻 Launching of EC2 instances. <br>
🏷️ Using tags for easy identification.<br>
💂 Set up IAM policies accessing EC2 instances based on the environment (development or production).<br>
👩‍👩‍👧‍👧 Create an IAM user and assign them to the appropriate user group with the necessary permissions for their role.<br>
🔓 Test IAM access for the User you've created.

# Introduction
In this project we will go over some cloud security basics. Throughout this project keep in mind that we are putting in practice AWS skills and tools to leverage the creation of a secure development environment for our company that goes by what a better name than Snowlynx.

We will leverage EC2, IAM, Users, Groups and Policies functionality of AWS to demonstrate in a simple environment how server data can be protected in organizations.

# Tools and Concepts
-> EC2 used for instance creation - the resources the members of the organization will have or NOT have access to.
-> IAM for user management - identify user level access and create proper policies for the way they are able to access the cloud resources of the company.

The key takeaway of this project would be that it represents a very good first EASY step in understanding what Zero Trust methodology for security is and how to start building it in the first steps.

# Reflecting on the project

This project is a short one that anyone can start with to understand how to use AWS and use some basic components of AWS to create secure environments. I believe this to be the very first stepping stone when talking about Cloud DevOps and DevOpsSec.

# Concepts

## Tags


In AWS, tags are key-value metadata labels that you assign to your cloud resources. They function as a labeling system, helping members of an organization to better understand the allocation of resources and to track work, load, and costs associated with those resources. 

Tags enable you to categorize resources by criteria such as purpose, owner, or environment, which aids in organization, cost management, security, automation, and operational support. Each tag consists of a user-defined key and an optional, user-defined value. It's important to note that tags are not encrypted and should not be used to store sensitive information.

For this project in specific I went ahead and created two EC2 resources which represent the **production** server of an application and the **development** server of an application this being said I went ahead and baptized the EC2s as simple as possible:

**Production Server**: snowlynx-prod-IAMsecurity
**Development Server**: snwolynx-dev-IAMsecurity

Please keep an eye also on the tag used in the picture in specific for the development server as we will use those later as well.

![1-ss-EC2-nametags](https://github.com/user-attachments/assets/f9ddcf99-3d49-4d78-a136-e90afb41385d)

## Managing Access with IAM Policies

In this project, I've implemented AWS Identity and Access Management (IAM) Policies to securely control access to our cloud resources. The primary aim was to establish clear rules for IAM Users, Groups, and specific Roles, moving our security framework towards a zero-trust model. This ensures that entities only have the permissions they absolutely need.

### Crafting the Project's Policy:

For this particular project, I opted to define the IAM policy using JSON. While using a visual web interface can be more straightforward in some scenarios, I find that constructing policies directly in JSON provides a much deeper understanding of how they function "under the hood." This approach gives a broader perspective on the intricacies of permission management.

The policy I've set up here is tailored for the development environment. It allows the team to identify existing resources by granting 'Read' permissions. Crucially, it then explicitly 'Denies' the ability to create or remove resources. This helps maintain the integrity of the dev environment and adheres to the principle of least privilege.

### Key Components of a JSON Policy:

When you're building an IAM policy using JSON, you need to define three main attributes for each statement:

**Effect**: This specifies whether the policy statement will Allow or Deny access.

**Action**: This lists the specific API operations that the policy statement applies to (e.g., s3:GetObject, ec2:StartInstances).

**Resource**: This indicates the specific AWS resource(s) that the statement pertains to (e.g., a particular S3 bucket ARN or EC2 instance ID).

Understanding these elements is fundamental to creating effective and granular access controls within AWS.

![2-ss-EC2-Policy](https://github.com/user-attachments/assets/8654fd45-1134-46cf-bdd4-8a3bfc8a8884)

## Simplifying AWS Access with Account Aliases

When working with AWS, one of the first things I like to set up for easier management is an Account Alias. Essentially, instead of having to remember or share a long, numerical AWS Account ID for logging into the AWS Management Console, an Account Alias lets you create a friendly, memorable name.

In this project, I configured an Account Alias to streamline the sign-in process. It's a quick win – took me just a few seconds to set up, and after about a minute for it to propagate, I had a much cleaner sign-in URL. For instance, instead of something like 123456789012.signin.aws.amazon.com/console, you get a custom URL. In my case, it ended up looking something like https://myproject-alias.signin.aws.amazon.com/console (I used "snowlynx-alias-1above" as an example in my initial setup).

This is particularly handy when you're collaborating with a team or even just for your own convenience, making it simpler and more professional to access the project's AWS resources. It's a small detail, but it contributes to a smoother workflow and demonstrates attention to good account hygiene from the get-go.

![3-ss-IAM-Alias](https://github.com/user-attachments/assets/091ee2a5-23c5-4b2a-ad53-06ef3b021cd3)

## Managing Project Access with IAM Users and User Groups

For this project, I've leveraged AWS Identity and Access Management (IAM) to ensure secure and organized access to our cloud resources. Here's how I've structured it:

### IAM Users:

Instead of using the root account for day-to-day tasks (which is a security best practice!), I've set up individual IAM Users. Think of these as specific accounts for each person or service that needs to interact with the project's AWS environment. This allows for granular control over who can do what. For example, a developer on the team would have an IAM User account that grants them access to the specific services they need, like EC2 or S3, but perhaps not billing information.

### IAM User Groups for Efficient Permission Management:

To make permission management scalable and easier to maintain, I've organized IAM Users into User Groups. A User Group is essentially a collection of IAM Users. Instead of assigning permissions (via IAM Policies) to each user individually, I assign them to a group. For instance, I created a "Developers" group and attached a policy that grants common development-related permissions.

### How Policies Apply:

By attaching an IAM Policy to a User Group, I define what actions members of that group are allowed or denied to perform on the AWS resources created for this project. If a new developer joins, I simply add their IAM User to the "Developers" group, and they automatically inherit all the necessary permissions. This is much more efficient than manually configuring permissions for each new team member and ensures consistency in access rights. This approach is key to maintaining the principle of least privilege while keeping operations smooth.

### IAM User Access and Permissions

For this project, new team members get access via IAM User accounts. They'll receive credentials and a sign-in URL, or an invite to set their own password.

Upon logging into the AWS console, users will find their access is tailored by the IAM policies I've configured. This means certain resources might not be visible or usable, demonstrating the principle of least privilege in action to secure our environment.


![4-IAM-internuser](https://github.com/user-attachments/assets/1e3f57f1-3b8e-405b-a67b-8c3eb51be305)


## Testing IAM Policies

To confirm our IAM policy was working as intended, I tested it by attempting to perform a restricted action: stopping the production instance.

As expected, the "Stop Instance" action failed. This successfully validated the policy we configured earlier in the project, which explicitly denies editing rights on instances, ensuring our production environment remains secure and stable.


![5-creds-error](https://github.com/user-attachments/assets/02d366dd-7944-43c9-9c46-604cc1a9264a)

To confirm our development environment controls, I attempted to stop the development instance.

The action was successful, and the instance stopped. This demonstrates that the IAM policy for our development users is correctly configured, allowing them the necessary control over their specific environment.

![6-creds-pass](https://github.com/user-attachments/assets/65e4fb39-6ec6-40b2-af5a-fbb3465aa25f)
