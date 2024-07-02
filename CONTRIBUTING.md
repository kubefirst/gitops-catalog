# Contributing

To contribute to the GitOps Catalog, you can add any applications, whether you are the creator, maintainers or not, by following the "[Add your application](#add-your-application)" steps. If, for any reasons, you would prefer us to add the application, feel free to [create an issue with your request](https://github.com/kubefirst/gitops-catalog/issues/new), and we'll see what we can do.

[![Video](https://img.youtube.com/vi/O8pTLnqIAuk/maxresdefault.jpg)](https://www.youtube.com/live/O8pTLnqIAuk)
_Click to listen to a livestream we did on how to add an application to the catalog_

- [Acceptance Criteria](#acceptance-criteria)
- [Add your application](#add-your-application)
- [Kubefirst Tokens](#kubefirst-tokens)
- [Application Maintenance and Removal](#application-maintenance-and-removal)
- [Testing](#testing)
- [Debugging](#debugging)
- [Need Help](#need-help)

## Acceptance Criteria

We will approve all GitOps Catalog application submissions as long as they are working with the latest version of kubefirst.

## Add your application

To make a new application available for installation, you'll need to:

- Fork this repository.
- Create a new directory with your new application's name in your fork.
  - Create a `components` folder, and inside, another folder with your application's name.
  - In that latest folder, add, and organize your Argo CD manifest file(s) into it.
    - Don't forget to look at the [Kubefirst Tokens](#kubefirst-tokens) section as you can dynamically add specific values to your manifests like domain or cluster names.
  - In the root folder of your application create an "App of Apps" YAML file that will point to the component folder.
- Add a SVG file of the application's logo, named `<appName>.svg` under the [logos folder](https://github.com/kubefirst/gitops-catalog/tree/main/logos).
  - If you don't have a SVG file, PNG and JPEG are also accepted. The application logo will be displayed in the GitOps catalog at a size of 32x32 pixels.
- Add a new entry to the [index.yaml](index.yaml) file with:
  - **name**: application name as described in your YAML file.
  - **displayName**: name to be displayed in the GitOps catalog (120 characters maximum).
  - **website**: application website or GitHub repository.
  - **imageUrl**: `https://raw.githubusercontent.com/kubefirst/gitops-catalog/main/logos/<appName>.<svg|png|jpeg|jpg>`
  - **description**: an insightful description about your application. It will be displayed in the GitOps Catalog (200 characters maximum).
  - **category**: one category amongst the following ones (if no category seems to be a good fit, add them to `Miscellaneous`):
    - App management
    - Architecture
    - CI/CD
    - Database
    - End user application
    - FinOps
    - Infrastructure
    - Miscellaneous
    - Monitoring
    - Observability
    - Security
    - Storage
    - Testing
- Create a pull request with the changes from your fork to our repository main branch. Be sure to [sign your commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits).

Feel free to check the other applications to find examples.

### Kubefirst Tokens

Any GitOps Catalog application can use the following tokens in their application's YAML so they can be replaced with the provisioned cluster information:

| Token                                             | Description                                                                                                                      |
|---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| `ARGOCD_INGRESS_NO_HTTP_URL`                      | The non-HTTP URL for accessing the Argo CD UI, typically for secure access.                                                      |
| `ARGOCD_INGRESS_URL`                              | The URL for accessing the Argo CD UI via ingress.                                                                                |
| `ARGO_WORKFLOWS_INGRESS_NO_HTTPS_URL`             | The non-HTTPS URL for accessing the Argo Workflows UI, for secure access.                                                        |
| `ARGO_WORKFLOWS_INGRESS_URL`                      | The URL for accessing the Argo Workflows UI via ingress.                                                                         |
| `ATLANTIS_ALLOW_LIST`                             | A list of allowed entities for Atlantis operations, specifying who can interact with Atlantis within the configured environment. |
| `ATLANTIS_INGRESS_NO_HTTPS_URL`                   | The non-HTTPS URL for accessing the Atlantis UI, for secure access.                                                              |
| `ATLANTIS_INGRESS_URL`                            | The URL for accessing the Atlantis UI via ingress.                                                                               |
| `AWS_NODE_CAPACITY_TYPE`                          | Indicates the capacity type of AWS nodes, such as on-demand or spot instances.                                                   |
| `CERT_MANAGER_ISSUER_ANNOTATION_1`                | An annotation for cert-manager to specify the issuer for the first set of certificates.                                          |
| `CERT_MANAGER_ISSUER_ANNOTATION_2`                | An annotation for cert-manager to specify the issuer for the second set of certificates.                                         |
| `CERT_MANAGER_ISSUER_ANNOTATION_3`                | An annotation for cert-manager to specify the issuer for the third set of certificates.                                          |
| `CERT_MANAGER_ISSUER_ANNOTATION_4`                | An annotation for cert-manager to specify the issuer for the fourth set of certificates.                                         |
| `CHARTMUSEUM_INGRESS_URL`                         | The URL for accessing the ChartMuseum UI via ingress.                                                                            |
| `CLOUD_PROVIDER`                                  | The cloud service provider where the cluster is hosted, such as AWS, Civo, DigitalOcean, Google Cloud or Vultr.                  |
| `CLOUD_REGION`                                    | The geographical region of the cloud provider where the cluster is deployed.                                                     |
| `CLUSTER_ID`                                      | A unique identifier for the cluster, assigned by the cloud provider or the management system.                                    |
| `CLUSTER_NAME`                                    | The name of the Kubernetes cluster, used to identify it within the cloud provider or the local environment.                      |
| `CLUSTER_TYPE`                                    | The type of the cluster, indicating whether it is a production, development, or testing cluster.                                 |
| `CONTAINER_REGISTRY_URL`                          | The URL of the container registry where container images are stored and retrieved.                                               |
| `DOMAIN_NAME`                                     | The domain name associated with the cluster or its services.                                                                     |
| `EXTERNAL_DNS_DOMAIN_NAME`                        | The domain name managed by the external DNS provider.                                                                            |
| `EXTERNAL_DNS_PROVIDER_NAME`                      | The name of the external DNS provider, such as Cloudflare or Route53.                                                            |
| `EXTERNAL_DNS_PROVIDER_SECRET_KEY`                | The key within the Kubernetes secret that stores the external DNS provider's credentials.                                        |
| `EXTERNAL_DNS_PROVIDER_SECRET_NAME`               | The name of the Kubernetes secret storing the external DNS provider's credentials.                                               |
| `EXTERNAL_DNS_PROVIDER_TOKEN_ENV_NAME`            | The environment variable name holding the token for the external DNS provider.                                                   |
| `GIT_DESCRIPTION`                                 | A description of the Git repository.                                                                                             |
| `GIT_FQDN`                                        | The Fully Qualified Domain Name associated with the Git service.                                                                 |
| `GIT_NAMESPACE`                                   | The namespace within the Git provider where the repository resides.                                                              |
| `GIT_PROVIDER`                                    | The platform or service hosting the Git repository, e.g., GitHub, GitLab.                                                        |
| `GIT_RUNNER`                                      | The runner used for executing Git CI/CD pipelines.                                                                               |
| `GIT_RUNNER_DESCRIPTION`                          | A description of the Git runner.                                                                                                 |
| `GIT_RUNNER_NS`                                   | The namespace where the Git runner is deployed.                                                                                  |
| `GIT_URL`                                         | The URL of the Git repository.                                                                                                   |
| `GITHUB_HOST`                                     | The hostname of the GitHub instance, for GitHub Enterprise users.                                                                |
| `GITHUB_OWNER`                                    | The owner of the GitHub repository, which can be a user or organization.                                                         |
| `GITHUB_USER`                                     | The GitHub username associated with the deployment.                                                                              |
| `GITLAB_HOST`                                     | The hostname of the GitLab instance, for GitLab self-hosted users.                                                               |
| `GITLAB_OWNER`                                    | The owner of the GitLab repository, which can be a user or group.                                                                |
| `GITLAB_OWNER_GROUP_ID`                           | The group ID of the GitLab repository owner, if applicable.                                                                      |
| `GITLAB_USER`                                     | The GitLab username associated with the deployment.                                                                              |
| `GITOPS_REPO_ATLANTIS_WEBHOOK_URL`                | The URL for the Atlantis webhook associated with the GitOps repository.                                                          |
| `GITOPS_REPO_GIT_URL`                             | The Git URL of the repository used for GitOps.                                                                                   |
| `GITOPS_REPO_NO_HTTPS_URL`                        | The non-HTTPS URL of the GitOps repository.                                                                                      |
| `GITOPS_REPO_URL`                                 | The URL of the repository used for GitOps operations.                                                                            |
| `GOOGLE_PROJECT`                                  | The Google Cloud project ID where the resources are deployed.                                                                    |
| `GOOGLE_UNIQUENESS`                               | A token to ensure uniqueness of resource names in Google Cloud deployments.                                                      |
| `KUBEFIRST_ARTIFACTS_BUCKET`                      | The cloud storage bucket used for storing artifacts related to Kubefirst deployments.                                            |
| `KUBEFIRST_STATE_STORE_BUCKET`                    | The cloud storage bucket used for storing the state of Kubefirst deployments.                                                    |
| `KUBEFIRST_STATE_STORE_BUCKET_HOSTNAME`           | The hostname of the cloud storage bucket used for storing Kubefirst deployment state.                                            |
| `KUBEFIRST_VERSION`                               | The version of the Kubefirst platform being used.                                                                                |
| `METAPHOR_DEVELOPMENT_INGRESS_URL`                | The ingress URL for accessing the development environment of the Metaphor application.                                           |
| `METAPHOR_PRODUCTION_INGRESS_URL`                 | The ingress URL for accessing the production environment of the Metaphor application.                                            |
| `METAPHOR_STAGING_INGRESS_URL`                    | The ingress URL for accessing the staging environment of the Metaphor application.                                               |
| `NODE_COUNT`                                      | The number of nodes within the cluster.                                                                                          |
| `NODE_TYPE`                                       | The type of node within the cluster, such as compute, memory-optimized, etc.                                                     |
| `TERRAFORM_FORCE_DESTROY`                         | A flag indicating whether Terraform should force the destruction of resources during teardown.                                   |
| `USE_TELEMETRY`                                   | A flag indicating whether telemetry data should be collected and sent.                                                           |
| `VAULT_DATA_BUCKET`                               | The cloud storage bucket used for storing Vault data.                                                                            |
| `VAULT_INGRESS_NO_HTTPS_URL`                      | The non-HTTPS URL for accessing the Vault UI, for secure access.                                                                 |
| `VAULT_INGRESS_URL`                               | The URL for accessing the Vault UI via ingress.                                                                                  |
| `VOUCH_INGRESS_URL`                               | The URL for accessing the Vouch proxy via ingress.                                                                               |
| `WORKLOAD_CLUSTER_BOOTSTRAP_TERRAFORM_MODULE_URL` | The URL of the Terraform module used for bootstrapping workload clusters.                                                        |
| `WORKLOAD_CLUSTER_TERRAFORM_MODULE_URL`           | The URL of the Terraform module used for deploying workload clusters.                                                            |

## Application Maintenance and Removal

We may remove an application from the GitOps Catalog with no notice if a severe vulnerability is discovered. Another reason for removing an application is if an application becomes abandoned or unmaintained by the upstream project/company/maintainer(s).

## Testing

If you want to test the application you are adding to the catalog, you need to run a couple of things locally. The process is a bit complicated, so feel free to submit a pull request without all the local tests, and we'll happily do the testing for you. If you want to proceed by yourself, follow these steps.

Firstly, if it's not already done, you need to [create a fork](https://github.com/kubefirst/kubefirst-api/fork) of the gitops-catalog repository.

Secondly, you need to clone the [Kubefirst API](https://github.com/kubefirst/kubefirst-api/) repository locally, and edit the file `internal/gitShim/gitopsCatalog.go`:

1. Update the `KubefirstGitHubOrganization` constant with the organization or username on which you forked the gitops-catalog repository.
2. Update the `KubefirstGitopsCatalogRepository` constant if you change the repository name when you forked it into your account.

Once it's done, follow [the instructions from the README](https://github.com/kubefirst/kubefirst-api/#running-locally) to run the API locally.

Thirdly, you will also need to run the [console application](https://github.com/kubefirst/console) locally. To do so, clone the repository locally, and follow [the instructions from the README](https://github.com/kubefirst/console#setup-instructions). It will be the equivalent of using the CLI with the `launch up` command.

Lastly, you will need to create a new cluster like you would usually do, using the UI installation. Once your new cluster is created, you'll see the [GitOps Catalog](https://docs.kubefirst.io/civo/gitops-catalog) tab, and you should see your new application listed, and try to install it.

### Debugging

If you need to refresh the GitOps catalog list of applications, instead of restarting the whole process, which can be cumbersome with the creation of a new cluster, you can connect to the MongoDB instance (using their [CLI](https://www.mongodb.com/docs/mongodb-shell/connect/) or [UI client](https://www.mongodb.com/docs/compass/current/connect/)), delete the `gitops-catalog` collection from the `api` database, and restart the API. You can now refresh the console browser tab, and you should see a new list. If you use the CLI, you can run `echo 'use api;\ndb.getCollection("gitops-catalog").drop();' | mongosh mongodb://<username>:<password>@<hostname>:<port>` in the terminal after replacing the `<username>`, `<password>`, `<hostname>`, and `<port>` with your database connection information.

If you already installed the application, to reinstall it another time without restarting the whole process, you'll need to follow these two steps:

1. Go into your `gitops` repository, the one created when you created your cluster from the console application. In the repository, go into the `registry` folder, followed by going into the folder named after your cluster name. You will need to delete the application YAML file inside this directory. Check also inside the `components` folder: if there is a folder with the name of the application you want to remove, delete it also. Once your changes are committed, Argo CD should sync, and remove the application. This is the usual process for removing an application from your cluster, GitOps catalog or not.
2. To be able to see the application as installable again in the catalog, you will need to update the `services` collection within the `api` database in MongoDB. You will need to remove the specific object associated with the application from the `services` array. If you use the CLI, you can run `echo 'use api;\ndb.services.updateOne({'cluster_name': "<clustername>" }, { $pull: { services: { name: "<appname>" } } } );' | mongosh mongodb://<username>:<password>@<hostname>:<port>` in the terminal after, as before, replacing the `<username>`, `<password>`, `<hostname>`, and `<port>` with your database connection information. You will also need to update the `<clustername>` with the name of the cluster you created, and the `<appname>` value in the query, with the name of the application to remove from the document.

## Need Help

As always, we are on our [Slack community](https://kubefirst.io/slack) in the #gitops-catalog channel if you need any help. We also welcome any constructive feedback, feature or application suggestions.
