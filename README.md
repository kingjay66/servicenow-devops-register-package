# ServiceNow DevOps Register Package GitHub Action

This custom action needs to be added at step level in a job to register package details in ServiceNow instance.

# Usage
## Step 1: Prepare values for setting up your secrets for Actions
- credentials (username and password for a ServiceNow devops integration user)
- instance URL for your ServiceNow dev, test, prod, etc. environments
- tool_id of your GitHub tool created in ServiceNow DevOps

## Step 2: Configure Secrets in your GitHub Ogranization or GitHub Repository
On GitHub, go in your organization settings or repository settings, click on the _Secrets > Actions_ and create a new secret.

Create secrets called 
- `SN_DEVOPS_USER`
- `SN_DEVOPS_PASSWORD`
- `SN_INSTANCE_NAME` only the **domain** string is required from your ServiceNow instance URL, for example https://**domain**.service-now.com
- `SN_ORCHESTRATION_TOOL_ID` only the **sys_id** is required for the GitHub tool created in your ServiceNow instance

## Step 3: Configure the GitHub Action if need to adapt for your needs or workflows
```yaml
registerpackage:
    name: Register Package
    runs-on: ubuntu-latest
    steps:
      - name: Register Package Step
        uses: ServiceNow/servicenow-devops-register-package@v1
        with:
          devops-integration-user-name: ${{ secrets.SN_DEVOPS_USER }}
          devops-integration-user-passwd: ${{ secrets.SN_DEVOPS_PASSWORD }}
          instance-name: ${{ secrets.SN_INSTANCE_NAME }}
          tool-id: ${{ secrets.SN_ORCHESTRATION_TOOL_ID }}
          context-github: ${{ toJSON(github) }}
          job-name: 'Register Package'
          artifacts: '[{"name": "com:customactiondemo","version": "1.${{ github.run_number }}","semanticVersion": "1.${{ github.run_number }}.0","repositoryName": "${{ github.repository }}"}]'
          package-name: 'registerpackage.war'
```
The values for secrets should be setup in Step 1. Secrets should be created in Step 2.

## Inputs

### `devops-integration-user-name`

**Required**  DevOps Integration Username to ServiceNow instance. 

### `devops-integration-user-password`

**Required**  DevOps Integration User Password to ServiceNow instance. 

### `instance-name`

**Required**  Name of ServiceNow instance to send register package notification. 

### `tool-id`

**Required**  Orchestration Tool Id for GitHub created in ServiceNow DevOps

### `context-github`

**Required**  Github context contains information about the workflow run details.

### `job-name`

**Required**  Display name of the job given for attribute _name_ in which _steps_ have been added for register package custom action.

### `artifacts`

**Required**  The list of artifacts to be registered in ServiceNow instance. Each artifact is a JSON object surrounded by curly braces _{}_ containing key-value pair separated by a comma _,_. A key-value pair consists of a key and a value separated by a colon _:_. The keys supported in key-value pair are _name_, _version_, _semanticVersion_, _repositoryName_ key-value pair separated by comma surrounded by curly braces {}. For example, '[{"name": "com:firstrepo","version": "1.13","semanticVersion": "1.13.0","repositoryName": "sample"}]'

### `package-name`

**Required**  Name of the package that contains artifacts.

## Outputs
No outputs produced.

## Support Model

ServiceNow built this custom action with the intent to help customers get started faster in integrating ServiceNow DevOps Change with GitHub Actions, but __will not be providing formal support__. This integration is therefore considered "use at your own risk", and will rely on the open-source community to help drive fixes and feature enhancements via Issues. Occasionally, ServiceNow may choose to contribute to the open-source project to help address the highest priority Issues, and will do our best to keep the integrations updated with the latest API changes shipped with family releases. This is a good opportunity for our customers and community developers to step up and help drive iteration and improvement on these open-source integrations for everyone's benefit. 

## Governance Model

Initially, ServiceNow product management and engineering representatives will own governance of these integrations to ensure consistency with roadmap direction. In the longer term, we hope that contributors from customers and our community developers will help to guide prioritization and maintenance of these integrations. At that point, this governance model can be updated to reflect a broader pool of contributors and maintainers.
