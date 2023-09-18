# pmap-template

**This is not an official Google product.**

This template is for creating an PMAP(privacy data mapping) repository to do
privacy data management, see more details
[here](https://github.com/abcxyz/pmap#background).

## How PMAP works

Please refer to the high level flow
[here](https://github.com/abcxyz/pmap#architecture).

## Prerequisites

The central privacy/compliance eng team need to complete the steps below:

1.  Create an pmap-template repository using this template, only copy main
    branch is required.

2.  Set up
    [Workload Identity Federation](https://cloud.google.com/iam/docs/workload-identity-federation),
    and a service account with adequate condition and permission, see details
    [here](https://github.com/abcxyz/pmap#workload-identity-federation). Please
    restrict any human access to this service account, it should only be used by
    your PMAP instance.

3.  Follow steps
    [here](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository)
    to set the repository variables for WORKLOAD_IDENTITY_PROVIDER and
    SERVICE_ACCOUNT with outputs from step 2 and set the repository variables
    for GCS_BUCKET with the output from
    [PMAP Terraform modules](https://github.com/abcxyz/pmap/blob/b1105ccaa211a3f0bba7c25edbe0f794dc92d54f/terraform/e2e/outputs.tf#L55)
    if you are following the
    [instructions](https://github.com/abcxyz/pmap#infrastructure-for-pmap) to
    create the infrastructure needed by PMAP instance.

4.  It is critical to enable the following repo settings:

    -   Disable forking
    -   Set up
        [CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
        with the group to approve requests
    -   Branch protection on the `main` (default) branch
        -   Require a pull request before merging
            -   Require approvals
            -   Dismiss stale pull request approvals when new commits are pushed
            -   Require review from Code Owners
            -   Require approval of the most recent reviewable push
        -   Require status checks to pass before merging
        -   Require signed commits
        -   Disallow force pushes

## End User Workflows

### Policy/Control Owner

*   Create a policy/control (e.g. a wipeout plan) by opening a PR and add a
    `yaml` file under the [sub folder](./example_org/policy) where stores all
    the policies/controls. See example
    [here](./example_org/policy/wipeout/default.yaml).

### Data Owner

*   Register and annotate resources to associate the resources to its specific
    policies/controls by opening a PR and add a mapping `yaml` file under the
    [sub folder](./example_org/mapping) where stores all the data mappings.
    The association of the resource to the corresponding policies/controls is
    achieved via `annotations` field. See example
    [here](./example_org/mapping/product_x/gcs_bucket.yaml).

    **NOTE:** The central privacy/compliance eng team has the flexibility to determine how to group
    mappings, they don’t have to follow the group mappings in the above
    [example](./example_org/mapping)(Product at level 1 and Resource at level 2).

### Data Governor

*   Approve the registered policy/control and resources by approving the related opening PRs
    created by policy/control owners and data owners.
*   Query the policy/contrl and resources stored in BigQuery, see details [here](https://github.com/abcxyz/pmap#data-governortodo).