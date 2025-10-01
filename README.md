# Ansible playbooks for the Squonk2 Job Operator

[![build](https://github.com/informaticsmatters/squonk2-data-manager-job-operator/actions/workflows/build.yaml/badge.svg)](https://github.com/informaticsmatters/squonk2-data-manager-job-operator/actions/workflows/build.yaml)
[![build tag](https://github.com/informaticsmatters/squonk2-data-manager-job-operator/actions/workflows/build-tag.yaml/badge.svg)](https://github.com/informaticsmatters/squonk2-data-manager-job-operator/actions/workflows/build-tag.yaml)

![GitHub](https://img.shields.io/github/license/informaticsmatters/squonk2-data-manager-job-operator-ansible)

![GitHub tag (latest SemVer pre-release)](https://img.shields.io/github/v/tag/informaticsmatters/squonk2-data-manager-job-operator-ansible?include_prereleases)

[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://conventionalcommits.org)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)

This repo contains playbooks for the Squonk2 Data Manager Job Operator.
Prerequisites: -

## Contributing
The project uses: -

- [pre-commit] to enforce linting of files prior to committing them to the
  upstream repository
- [Commitizen] to enforce a [Conventional Commit] commit message format

You **MUST** comply with these choices in order to  contribute to the project.

To get started review the pre-commit utility and the conventional commit style
and then set-up your local clone by following the **Installation** and
**Quick Start** sections: -

    pip install -r build-requirements.txt
    pre-commit install -t commit-msg -t pre-commit

Now the project's rules will run on every commit, and you can check the
current health of your clone with: -

    pre-commit run --all-files

## Deploying into the Data Manager API
We use [Ansible] and is done via a suitable Python
environment using the requirements in the root of the project...

    python -m venv venv
    source venv/bin/activate
    pip install --upgrade pip
    pip install -r requirements.txt

Set your KUBECONFIG for the cluster and verify it's as expected by listing the nodes: -

    export KUBECONFIG=~/k8s-config/config-local
    kubectl get no
    [...]

Now, create a parameter file (i.e. `parameters.yaml`) based on the project's
`parameters-template.yaml`, setting values for the operator that match your
needs. Then deploy, using Ansible, from the root of the project: -

    PARAMS=parameters
    ansible-playbook -e @${PARAMS}.yaml site.yaml

To remove the operator (assuming there are no operator-derived instances)...

    ansible-playbook -e @${PARAMS}.yaml -e jo_state=absent site.yaml

>   The current Data Manager API assumes that once an Application (operator)
    has been installed it is not removed. So, removing the operator here
    is described simply to illustrate a 'clean-up' - you would not
    normally remove an Application operator in a production environment.

---

[ansible]: https://pypi.org/project/ansible/
[commitizen]: https://pypi.org/project/commitizen
[conventional commit]: https://www.conventionalcommits.org/en/v1.0.0/
[pre-commit]: https://pre-commit.com
