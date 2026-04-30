# Application Manifest Build Config Conversion

> **NextGenCI GitLab Builder** — `NC Manifest Application DD` → `app-manifest-build-config.yml`

## Application Manifest Build Config

Application manifest build config is a YAML file that contains the configuration for the application manifest build.
We need to explore on how to generate this file by during build of entity DD as output artifacts, to pass to the AM Build CLI.

We have two components that can allow us to generate this file:

1. `helm-charts/app-chart` component - QQ: How do we get the value of `valuesPathPrefix` ?? .Values.
2. `nc-manifests/application-dd` component

### Reference field processing:
To genereate the metedata for external artifacts.
How do we get the value of `reference` field ??
- for images its full docker url
- for helm charts its ??

What is the meaning of below ??
    # If specified, the component's attributes should be collected from an external artifact.

```yaml
  - component: $CI_SERVER_FQDN/prod.platform.devops/nextgenci/components/helm-charts/app-chart@1.5.0
    inputs:
      stage: package
      appchart_source_path: "app/cloudbss-bsscm-app-chart"
      application_name: "$APPLICATION_NAME"
      helm_dependency_repos:
        - name: nc.libraries.helm
          path: https://artifactorycn.netcracker.com/nc.libraries.helm
        - name: nc.libraries.helm.staging
          path: https://artifactorycn.netcracker.com/nc.libraries.helm.staging
      sub_charts:
        - name: bss-config-manager
          path: services/bss-config-manager/helm-templates
        - name: bss-config-manager-frontend
          path: services/bss-config-manager-frontend/helm-templates
      nc_manifest_entity_group_id: $APPLICATION_ENTITIES_GROUP_ID
      nc_manifest_entity_name: cloudbss-bsscm-app-chart

  - component: $CI_SERVER_FQDN/prod.platform.devops/nextgenci/components/nc-manifests/application-dd@1.8.2
    inputs:
      stage: publish
      application_artifact_id: "$APPLICATION_ARTIFACT_ID"
      application_name: "$APPLICATION_NAME"
      entities_group_id: "$APPLICATION_ENTITIES_GROUP_ID"
      entities:
        - bss-config-manager
        - bss-config-manager-frontend
        - cloudbss-bsscm-app-chart
      included_entities:
        - gav: "com.netcracker.cloud.core.platform-scripts-app.deployment-descriptor.platform:deployment-artifacts:release-2026.1-1.11.4-20260217.083449-1-RELEASE"
          repository: "pd.saas.mvn"
```

Example: `app-manifest-build-config.yml` file content:

## Component Metadata Generation - During Entity DD Build

Component metadata files are JSON files containing detailed information about individual components (Docker images, Helm charts).
We need to explore on how to generate these files by during build of entity DD as output artifacts, to pass to the AM Build CLI.

This allows incorporating build-time attributes (hashes, versions, registry references) that are only available after the component is built.
QQ: But why do we need these files ??

Example: `application/vnd.docker.image` file content:

```json
{
  "name": "<docker-image-name>",
  "type": "container",
  "mime-type": "application/vnd.docker.image",
  "hashes":[
    {
      "alg": "<hash-algorithm>",
      "content": "<hash-content>"
    }
  ],
  "reference": "registry_host/namespace/image_name:image_tag"
}
```

## AM Build CLI flags - To be used by NC Manifest Application DD Component Build Job

| Attribute             | Type   | Mandatory | Description                                                  |
|-----------------------|--------|-----------|--------------------------------------------------------------|
| `--config`/`-c`       | string | yes       | Path to the Application Manifest Build configuration file    |
| `--version`/`-v`      | string | no        | Application version                                          |
| `--name`/`-n`         | string | mp        | Application name                                             |
| `--out`/`-o`          | string | yes       | Path where to save the generated Application Manifest        |
| positional parameters | string | yes       | Paths to component metadata files as positional parameters   |