# compliant-vagrant
Demo on how to integrate complyctl during the deployment of a Vagrant box

This is a simplistic Demo where `complyctl` is included in the pipeline to make sure a provisioned VM is 100% compliant with a custom Assessment Plan.

## Context

The initial Assessment Plan is based on [CUSP Profile](https://github.com/ComplianceAsCode/oscal-content/blob/main/profiles/fedora-cusp_fedora-default/profile.json) available in [oscal-content](https://github.com/ComplianceAsCode/oscal-content/tree/main) repository, which is automatically transformed into OSCAL from [ComplianceAsCode/content](https://github.com/ComplianceAsCode/content/blob/master/controls/cusp_fedora.yml) repository.

At this point Compliance and Product experts collaborate to refine an Assessment Plan for Fedora 42 according to the site policies.

The outcome of this collaboration is a customized Assessment Plan, that can be used by SREs to increment pipelines.

Take a look in the [demo_complyctl_fedora.yml](https://github.com/marcusburghardt/complytime-demos/blob/main/base_ansible_env/demo_complyctl_fedora.yml) for a complete automated workflow showing how the customized Assessment Plan was created and tested.

### Problem

In [complytime-demos](https://github.com/marcusburghardt/complytime-demos) repository we strongly rely on Vagrant boxes for our demos.

Currently, multiple images are directly consumed after a human and manual analysis of each image. This is not scalable.

Ideally we would like to adopt newer images much faster with the minimal possible friction and high security standards.

### Solution

Currently there is a CUSP profile available for Fedora, which includes multiple security recommendations that could improve considerably the security of our Demo VMs. We would like to adopt these hardening recommendations.

However, we use a very specific environment and naturally not all requirements from CUSP are relevant. For that reason, a customized Assessment Plan was created to include a minimal coverage (only 3 rules for the purposes of this demo).

Once this Assessment Plan is created in collaboration between Compliance and Fedora experts, we want to use this Assessment Plan to check if the provisioned VM is 100% compliant with our desired state.

Once this is confirmed, we want to ensure this state whenever a new image is introduced or the Assessment Plan is updated. In case any issue is found, the same environment can be reproduced locally for deeper analysis.

All the content is available in a GitHub repository, so we can much easily adopt GitOps approach to make our lives easier.

### Workflow

Take a look on this in practice: https://github.com/marcusburghardt/compliant-vagrant/actions
