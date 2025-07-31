# compliant-vagrant
Demo on how to integrate `complyctl` during the deployment of a Vagrant box.

This is a simplistic Demo where `complyctl` is included in the pipeline to make sure a provisioned VM is 100% compliant with a custom Assessment Plan based on [CUSP Profile](https://github.com/ComplianceAsCode/oscal-content/blob/main/profiles/fedora-cusp_fedora-default/profile.json), which is available out-of-box with `complyctl` in Fedora.

## Context

The initial Assessment Plan is based on [CUSP Profile](https://github.com/ComplianceAsCode/oscal-content/blob/main/profiles/fedora-cusp_fedora-default/profile.json) available in [oscal-content](https://github.com/ComplianceAsCode/oscal-content/tree/main) repository, which is automatically transformed into OSCAL from [ComplianceAsCode/content](https://github.com/ComplianceAsCode/content/blob/master/controls/cusp_fedora.yml) repository.

### Problem

In [complytime-demos](https://github.com/marcusburghardt/complytime-demos) repository we strongly rely on Vagrant boxes for our demos.

Currently, multiple images are directly consumed after a human and manual analysis of each image. This does not scale well.

We would like to adopt newer images much faster with the minimal possible friction and also increase security standards.

### Solution

Currently the CUSP profile includes multiple security recommendations that could improve considerably the security of our Demo VMs. We would like to adopt these hardening recommendations.

However, we use a very specific environment and naturally not all requirements from CUSP are relevant. For that reason, a customized Assessment Plan with a minimal coverage (only 3 rules for the purposes of this demo) will be used.

Once this Assessment Plan is created in collaboration between Compliance and Fedora experts (see [demo_complyctl_fedora.yml](https://github.com/marcusburghardt/complytime-demos/blob/main/base_ansible_env/demo_complyctl_fedora.yml)), we want to use it to check if our provisioned VMs are 100% compliant with the desired state.

There are many moving pieces involving compliance even in a relatively simple environment like this, and manual assessment may be overwhelming.
Thanks to hosting all the content in Git we can much easily adopt GitOps approach to make our lives easier.

Whenever a new image is introduced or the Assessment Plan is updated, CI tests will check if the desired state was preserved. Otherwise, they will bring awareness so the issues can be easily reproduced locally for deeper analysis.

### Workflow

Take a look on this in practice: https://github.com/marcusburghardt/compliant-vagrant/actions

Here is an example when the changes don't affect the desired state:
- https://github.com/marcusburghardt/compliant-vagrant/actions/runs/16625397990/job/47040320793

Here is an example when a change affects the desired state:
- https://github.com/marcusburghardt/compliant-vagrant/actions/runs/16625818235/job/47041820715?pr=4

P.S.: If the links from the above examples are expired, you can check the "Actions" page or propose a PR for testing.
