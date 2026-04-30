# Registry Config Conversion Spec

> **NextGenCI GitLab Builder** — `registry-config.yaml` → `regdef.yml` / `regdef-v2.yml`

---

## Part 1 — registry-config.yaml → regdef.yml (v1)

### Field Mapping

| registry-config.yaml Variable | regdef.yml Field | Notes |
|---|---|---|
| -- | `name` | Not in registry-config.yaml. Supply externally. |
| -- | `credentialsId` | ⚠️ Must be identified how to actually set via GitLab CI Credentials as Credential ID. |
| `NG_CI_REGISTRY_HOST` | `mavenConfig.repositoryDomainName` | Use as-is (`https://...`). |
| `NG_CI_REGISTRY_HOST` | `mavenConfig.fullRepositoryUrl` | Same value as `repositoryDomainName`. |
| `NG_CI_MAVEN_SNAPSHOT_REPO_NAME` | `mavenConfig.targetSnapshot` | |
| `NG_CI_MAVEN_STAGING_REPO_NAME` | `mavenConfig.targetStaging` | |
| `NG_CI_MAVEN_RELEASE_REPO_NAME` | `mavenConfig.targetRelease` | |
| `NG_CI_MAVEN_GROUP_REPO_NAME` | `mavenConfig.snapshotGroup` | |
| `NG_CI_MAVEN_RELEASE_PROXY_REPO_NAME` | `mavenConfig.releaseGroup` | |
| `NG_CI_DOCKER_GROUP_REGISTRY_HOST` | `dockerConfig.groupUri` | |
| `NG_CI_DOCKER_GROUP_REPO_NAME` | `dockerConfig.groupName` | |
| `NG_CI_DOCKER_SNAPSHOT_REGISTRY_HOST` | `dockerConfig.snapshotUri` | |
| `NG_CI_DOCKER_SNAPSHOT_REPO_NAME` | `dockerConfig.snapshotRepoName` | |
| `NG_CI_DOCKER_STAGING_REGISTRY_HOST` | `dockerConfig.stagingUri` | |
| `NG_CI_DOCKER_STAGING_REPO_NAME` | `dockerConfig.stagingRepoName` | |
| `NG_CI_DOCKER_RELEASE_REGISTRY_HOST` | `dockerConfig.releaseUri` | |
| `NG_CI_DOCKER_RELEASE_REPO_NAME` | `dockerConfig.releaseRepoName` | |
| `NG_CI_NPM_SNAPSHOT_REPO_NAME` | `npmConfig.npmTargetSnapshot` | |
| `NG_CI_NPM_RELEASE_REPO_NAME` | `npmConfig.npmTargetRelease` | |
| `NG_CI_NPM_GROUP_REPO_NAME` | *(excluded)* | ⚠️ No `groupRepo` field in schema ??. |
| `NG_CI_GOLANG_SNAPSHOT_REPO_NAME` | `goConfig.goTargetSnapshot` | |
| `NG_CI_GOLANG_RELEASE_REPO_NAME` | `goConfig.goTargetRelease` | |
| `NG_CI_GOLANG_GROUP_REPO_NAME` | `goConfig.goProxyRepository` | |
| `NG_CI_RAW_SNAPSHOT_REPO_NAME` | `rawConfig.rawTargetSnapshot` | |
| `NG_CI_RAW_STAGING_REPO_NAME` | `rawConfig.rawTargetStaging` | |
| `NG_CI_RAW_RELEASE_REPO_NAME` | `rawConfig.rawTargetRelease` | |
| `NG_CI_RAW_PROXY_REPO_NAME` | `rawConfig.rawTargetProxy` | |
| `NG_CI_HELM_LIB_SNAPSHOT_REPO_NAME` | `helmConfig.helmTargetStaging` | Library charts. |
| `NG_CI_HELM_LIB_REPO_NAME` | `helmConfig.helmTargetRelease` | Library charts. |
| `NG_CI_HELM_STAGING_REPO_NAME` | `helmAppConfig.helmStagingRepoName` | Application charts. |
| `NG_CI_HELM_RELEASE_REPO_NAME` | `helmAppConfig.helmReleaseRepoName` | Application charts. |
| `NG_CI_HELM_GROUP_REPO_NAME` | `helmAppConfig.helmGroupRepoName` | Application charts. |
| `NG_CI_HELM_SNAPSHOT_REPO_NAME` | `helmAppConfig.helmDevRepoName` | Application charts. |
| `NG_CI_DOCKER_THIRD_PARTY_REGISTRY_HOST` | *(excluded)* | No schema field. |

### Example Output

```yaml
name: "Shared Platform Registry"
credentialsId: ""  # NOTE: replace with actual GitLab CI credential ID

mavenConfig:
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  fullRepositoryUrl:    "https://artifactorycn.netcracker.com"
  targetSnapshot:  "pd.saas.mvn.snapshots"
  targetStaging:   "pd.saas.mvn.staging"
  targetRelease:   "pd.saas.mvn"
  snapshotGroup:   "pd.saas.mvn.group"
  releaseGroup:    "pd.saas-release.mvn.group"

dockerConfig:
  groupUri:         "artifactorycn.netcracker.com:17004"
  groupName:        "pd.saas.docker.group"
  snapshotUri:      "artifactorycn.netcracker.com:17001"
  snapshotRepoName: "pd.saas.docker.dev"
  stagingUri:       "artifactorycn.netcracker.com:17002"
  stagingRepoName:  "pd.saas.docker.staging"
  releaseUri:       "artifactorycn.netcracker.com:17003"
  releaseRepoName:  "pd.saas.docker"

npmConfig:
  npmTargetSnapshot: "pd.saas.npm.dev"
  npmTargetRelease:  "pd.saas.npm"
  # NOTE: NG_CI_NPM_GROUP_REPO_NAME excluded — no groupRepo field in schema

goConfig:
  goTargetSnapshot:  "pd.saas.golang.dev"
  goTargetRelease:   "pd.saas.golang"
  goProxyRepository: "pd.saas-release.golang.group"

rawConfig:
  rawTargetSnapshot: "pd.saas.raw.dev"
  rawTargetStaging:  "pd.saas.raw.staging"
  rawTargetRelease:  "pd.saas.raw"
  rawTargetProxy:    "pd.saas-raw.group"

helmConfig:
  helmTargetStaging: "nc.libraries.helm.staging"
  helmTargetRelease: "nc.libraries.helm"

helmAppConfig:
  helmStagingRepoName: "nc.helm.charts.staging"
  helmReleaseRepoName: "nc.helm.charts.release"
  helmGroupRepoName:   "nc.helm.charts"
  helmDevRepoName:     "nc.helm.charts.dev"
```

---

## Part 2 — registry-config.yaml → regdef-v2.yml (v2)

### Field Mapping

| registry-config.yaml Variable | regdef-v2.yml Field | Notes |
|---|---|---|
| *(hardcoded)* | `version` | Always `"2.0"`. |
| *(generation-time input)* | `name` | Not in registry-config.yaml. Supply externally. |
| *(generation-time input)* | `authConfig.default.credentialsId` | ⚠️ Must be identified how to actually set via GitLab CI Credentials as Credential ID. |
| `NG_CI_REGISTRY_AUTH_TYPE` | `authConfig.default.authMethod` | `"basic"` → `"user_pass"` ⚠️ Others we don't support right now. |
| `NG_CI_REGISTRY_HOST` | `mavenConfig.repositoryDomainName` | Use as-is (`https://...`). |
| `NG_CI_MAVEN_SNAPSHOT_REPO_NAME` | `mavenConfig.targetSnapshot` | |
| `NG_CI_MAVEN_STAGING_REPO_NAME` | `mavenConfig.targetStaging` | |
| `NG_CI_MAVEN_RELEASE_REPO_NAME` | `mavenConfig.targetRelease` | |
| `NG_CI_MAVEN_GROUP_REPO_NAME` | `mavenConfig.snapshotGroup` | |
| `NG_CI_MAVEN_RELEASE_PROXY_REPO_NAME` | `mavenConfig.releaseGroup` | |
| `NG_CI_REGISTRY_HOST` | `dockerConfig.repositoryDomainName` | |
| `NG_CI_DOCKER_GROUP_REGISTRY_HOST` | `dockerConfig.groupUri` | |
| `NG_CI_DOCKER_GROUP_REPO_NAME` | `dockerConfig.groupName` | |
| `NG_CI_DOCKER_SNAPSHOT_REGISTRY_HOST` | `dockerConfig.snapshotUri` | |
| `NG_CI_DOCKER_SNAPSHOT_REPO_NAME` | `dockerConfig.snapshotRepoName` | |
| `NG_CI_DOCKER_STAGING_REGISTRY_HOST` | `dockerConfig.stagingUri` | |
| `NG_CI_DOCKER_STAGING_REPO_NAME` | `dockerConfig.stagingRepoName` | |
| `NG_CI_DOCKER_RELEASE_REGISTRY_HOST` | `dockerConfig.releaseUri` | |
| `NG_CI_DOCKER_RELEASE_REPO_NAME` | `dockerConfig.releaseRepoName` | |
| `NG_CI_REGISTRY_HOST` | `npmConfig.repositoryDomainName` | |
| `NG_CI_NPM_SNAPSHOT_REPO_NAME` | `npmConfig.npmTargetSnapshot` | |
| `NG_CI_NPM_RELEASE_REPO_NAME` | `npmConfig.npmTargetRelease` | |
| `NG_CI_NPM_GROUP_REPO_NAME` | *(excluded)* | ⚠️ No `groupRepo` field in schema. |
| `NG_CI_REGISTRY_HOST` | `goConfig.repositoryDomainName` | |
| `NG_CI_GOLANG_SNAPSHOT_REPO_NAME` | `goConfig.goTargetSnapshot` | |
| `NG_CI_GOLANG_RELEASE_REPO_NAME` | `goConfig.goTargetRelease` | |
| `NG_CI_GOLANG_GROUP_REPO_NAME` | `goConfig.goProxyRepository` | |
| `NG_CI_REGISTRY_HOST` | `rawConfig.repositoryDomainName` | |
| `NG_CI_RAW_SNAPSHOT_REPO_NAME` | `rawConfig.rawTargetSnapshot` | |
| `NG_CI_RAW_STAGING_REPO_NAME` | `rawConfig.rawTargetStaging` | |
| `NG_CI_RAW_RELEASE_REPO_NAME` | `rawConfig.rawTargetRelease` | |
| `NG_CI_RAW_PROXY_REPO_NAME` | `rawConfig.rawTargetProxy` | |
| `NG_CI_REGISTRY_HOST` | `helmConfig.repositoryDomainName` | |
| `NG_CI_HELM_LIB_SNAPSHOT_REPO_NAME` | `helmConfig.helmTargetStaging` | Library charts. |
| `NG_CI_HELM_LIB_REPO_NAME` | `helmConfig.helmTargetRelease` | Library charts. |
| `NG_CI_REGISTRY_HOST` | `helmAppConfig.repositoryDomainName` | |
| `NG_CI_HELM_STAGING_REPO_NAME` | `helmAppConfig.helmStagingRepoName` | Application charts. |
| `NG_CI_HELM_RELEASE_REPO_NAME` | `helmAppConfig.helmReleaseRepoName` | Application charts. |
| `NG_CI_HELM_GROUP_REPO_NAME` | `helmAppConfig.helmGroupRepoName` | Application charts. |
| `NG_CI_HELM_SNAPSHOT_REPO_NAME` | `helmAppConfig.helmDevRepoName` | Application charts. |
| `NG_CI_DOCKER_THIRD_PARTY_REGISTRY_HOST` | *(excluded)* | No schema field. |

### Example Output

```yaml
version: "2.0"
name: "Shared Platform Registry"

authConfig:
  default:
    credentialsId: ""        # NOTE: replace with actual GitLab CI credential ID
    authMethod: "user_pass"  # mapped from NG_CI_REGISTRY_AUTH_TYPE: "basic"

mavenConfig:
  authConfig:           "default"
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  targetSnapshot:  "pd.saas.mvn.snapshots"
  targetStaging:   "pd.saas.mvn.staging"
  targetRelease:   "pd.saas.mvn"
  snapshotGroup:   "pd.saas.mvn.group"
  releaseGroup:    "pd.saas-release.mvn.group"

dockerConfig:
  authConfig:       "default"
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  groupUri:         "artifactorycn.netcracker.com:17004"
  groupName:        "pd.saas.docker.group"
  snapshotUri:      "artifactorycn.netcracker.com:17001"
  snapshotRepoName: "pd.saas.docker.dev"
  stagingUri:       "artifactorycn.netcracker.com:17002"
  stagingRepoName:  "pd.saas.docker.staging"
  releaseUri:       "artifactorycn.netcracker.com:17003"
  releaseRepoName:  "pd.saas.docker"

npmConfig:
  authConfig:           "default"
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  npmTargetSnapshot: "pd.saas.npm.dev"
  npmTargetRelease:  "pd.saas.npm"
  # NOTE: NG_CI_NPM_GROUP_REPO_NAME excluded — no groupRepo field in schema

goConfig:
  authConfig:           "default"
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  goTargetSnapshot:  "pd.saas.golang.dev"
  goTargetRelease:   "pd.saas.golang"
  goProxyRepository: "pd.saas-release.golang.group"

rawConfig:
  authConfig:           "default"
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  rawTargetSnapshot: "pd.saas.raw.dev"
  rawTargetStaging:  "pd.saas.raw.staging"
  rawTargetRelease:  "pd.saas.raw"
  rawTargetProxy:    "pd.saas-raw.group"

helmConfig:
  authConfig:           "default"
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  helmTargetStaging: "nc.libraries.helm.staging"
  helmTargetRelease: "nc.libraries.helm"

helmAppConfig:
  authConfig:           "default"
  repositoryDomainName: "https://artifactorycn.netcracker.com"
  helmStagingRepoName: "nc.helm.charts.staging"
  helmReleaseRepoName: "nc.helm.charts.release"
  helmGroupRepoName:   "nc.helm.charts"
  helmDevRepoName:     "nc.helm.charts.dev"
```