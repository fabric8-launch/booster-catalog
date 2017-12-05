# Booster Catalog
Set of known example applications (Boosters) conforming to the minimal set of requirements necessary to be served by developers.redhat.com/launch.

Branching and tagging strategy:
- `master`:
    - should always point to the latest development branches of the boosters
        - if a particular variant of a booster isn't available in a branch, then it should point to the latest tag
    - can be updated at any time, because it's not used in any Fabric8 Launcher instance we run
- `next`:
    - should always point to the latest tags of the boosters
    - can be updated at any time, because it's only used in the staging Fabric8 Launcher instance we run
    - booster catalog releases are cut from this branch
        - the release is only cut when all the boosters dependencies are available publicly
        - the catalog release is used in the production Fabric8 Launcher instance we run, but only when you target an OpenShift Online Pro cluster
        - the catalog release is also used when you install Fabric8 Launcher locally from the template
- `openshift-online-free`:
    - should only contain the boosters that can successfully run in OpenShift Online Starter
    - should always point to the latest tags of the boosters
    - is used in the production Fabric8 Launcher instance we run, if you target an OpenShift Online Starter cluster
    - can only be updated when all the boosters dependencies are available publicly
        - this is because updates to this branch can percolate to the production Fabric8 Launcher instance at any time

The repository has a `metadata.json` file in the root containing a list of the supported missions and runtimes along with their human-readable names.

IMPORTANT: If a new mission or runtime is introduced, you MUST change the `metadata.json` file too. 

This repository is organized by `{mission}/{runtime}/{booster-catalog-entry}.yaml` or  `{mission}/{runtime}/{version}/{booster-catalog-entry}.yaml` depending if your booster supports runtime versions or not:

For each booster (example application), create a YAML file in the respective `{mission}/{runtime}` (or  `{mission}/{runtime}/{version}`) directory with information containing:

Name   | Description 
------ | -----------
githubRepo| The GitHub repository location
gitRef | The git reference (tag/branch/SHA1)
boosterDescriptorPath| (Optional) Path in the repository specified to `booster.yaml` (defaults to `.openshiftio/booster.yaml`)
buildProfile| (Optional) The Maven profile that should be activated in the booster's `pom.xml` file

The `booster.yaml` file beforementioned is expected to have the following structure:

Name   | Description | Required | Size
------ | ----------- | -----    | ----
name | The booster name  |  Yes  |  50
descriptionPath  |  (Optional) Link to file in repo containing adoc for the description (assumed default: `.openshiftio/description.adoc` ) |No  |  255
jenkinsfilePath | (Optional) Link to file in repo, relative to the repo root, for the Jenkins Pipeline Definition file (assumed default: `Jenkinsfile`) | No | 255
versions | A list of associative arrays containing `id` and `name` elements that map version ids to their human-readable names |  No  |  --

For an in-depth explanation of how to declare runtime versions see [HOWTO Add Runtime Versions to Boosters](https://github.com/openshiftio/booster-catalog/wiki/HOWTO-Add-Runtime-Versions-to-Boosters)
