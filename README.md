# Block Serverless Spark in Azure Machine Learning with Azure Policy
This guide will help you create an Azure Policy definition to block users from utilising Serverless Spark compute targets within Azure Machine Learning.

### Pre-requisites:
- An Azure subscription with appropriate permissions to create policies;
- Familiarity with setup of Azure Policies.

### Configuration Steps:

1. Define the Policy JSON:

Create a new file e.g., [blockServerlessSpark.json](blockServerlessSpark.json) and paste the following JSON code, replacing <location> with your desired resource location:

``` JSON
{
    "if": {
        "allOf": [
            {
                "field": "Microsoft.MachineLearningServices/workspaces/jobs/jobType",
                "in": [
                    "Spark"
                ]
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

This policy checks for resources of type "Microsoft.MachineLearning/workspaces/computes" with the property "computeType" set to "Serverless".
The "IfNotExists" mode ensures the policy only applies when creating new compute targets, not modifying existing ones.
Deploy the Policy Definition:

2. Create the policy definition.

Here's an example using Azure CLI:
``` Bash
az policy definition create --name BlockServerlessSparkCompute --location <location> --from-file blockServerlessSpark.json
```

3. Assign the Policy:

To restrict the policy to a specific resource group or subscription, assign it using Azure Policy initiatives. Refer to Microsoft documentation for details on assigning policies: https://learn.microsoft.com/en-us/azure/governance/policy/concepts/assignment-structure

4. Testing:

Try creating a new compute target with Serverless type in Azure Machine Learning studio. The policy should deny the creation.

> Note: This is a basic policy example. You can customize it further based on your needs.
