# API Reference

## Packages
- [feast.dev/v1](#feastdevv1)


## feast.dev/v1

Package v1 contains API Schema definitions for the v1 API group

### Resource Types
- [FeatureStore](#featurestore)



#### AuthzConfig



AuthzConfig defines the authorization settings for the deployed Feast services.

_Appears in:_
- [FeatureStoreSpec](#featurestorespec)

| Field | Description |
| --- | --- |
| `kubernetes` _[KubernetesAuthz](#kubernetesauthz)_ | kubernetes configures Kubernetes RBAC authorization. |
| `oidc` _[OidcAuthz](#oidcauthz)_ | oidc configures OpenID Connect authorization. |


#### AutoscalingConfig



AutoscalingConfig defines HPA settings for the FeatureStore deployment.

_Appears in:_
- [ScalingConfig](#scalingconfig)

| Field | Description |
| --- | --- |
| `minReplicas` _integer_ | minReplicas is the lower limit for the number of replicas. Defaults to 1. |
| `maxReplicas` _integer_ | maxReplicas is the upper limit for the number of replicas. Required. |
| `metrics` _[MetricSpec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#metricspec-v2-autoscaling) array_ | metrics contains the specifications for which to use to calculate the desired replica count.
If not set, defaults to 80% CPU utilization. |
| `behavior` _[HorizontalPodAutoscalerBehavior](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#horizontalpodautoscalerbehavior-v2-autoscaling)_ | behavior configures the scaling behavior of the target. |


#### BatchEngineConfig



BatchEngineConfig defines the batch compute engine configuration.

_Appears in:_
- [FeatureStoreSpec](#featurestorespec)

| Field | Description |
| --- | --- |
| `configMapRef` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | configMapRef references a ConfigMap containing the batch engine configuration.
The ConfigMap should contain YAML-formatted config with 'type' and engine-specific fields. |
| `configMapKey` _string_ | configMapKey is the key name in the ConfigMap. Defaults to "config" if not specified. |


#### ContainerConfigs



ContainerConfigs k8s container settings for the server

_Appears in:_
- [CronJobContainerConfigs](#cronjobcontainerconfigs)
- [RegistryServerConfigs](#registryserverconfigs)
- [ServerConfigs](#serverconfigs)

| Field | Description |
| --- | --- |
| `image` _string_ | image overrides the default container image. |
| `env` _[EnvVar](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envvar-v1-core)_ | env overrides container environment variables. |
| `envFrom` _[EnvFromSource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envfromsource-v1-core)_ | envFrom overrides container environment sources. |
| `imagePullPolicy` _[PullPolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#pullpolicy-v1-core)_ | imagePullPolicy overrides the container image pull policy. |
| `resources` _[ResourceRequirements](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#resourcerequirements-v1-core)_ | resources overrides the container resource requirements. |
| `nodeSelector` _map[string]string_ | nodeSelector overrides the container node selector. |


#### CronJobContainerConfigs



CronJobContainerConfigs k8s container settings for the CronJob

_Appears in:_
- [FeastCronJob](#feastcronjob)

| Field | Description |
| --- | --- |
| `image` _string_ | image overrides the default container image. |
| `env` _[EnvVar](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envvar-v1-core)_ | env overrides container environment variables. |
| `envFrom` _[EnvFromSource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envfromsource-v1-core)_ | envFrom overrides container environment sources. |
| `imagePullPolicy` _[PullPolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#pullpolicy-v1-core)_ | imagePullPolicy overrides the container image pull policy. |
| `resources` _[ResourceRequirements](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#resourcerequirements-v1-core)_ | resources overrides the container resource requirements. |
| `nodeSelector` _map[string]string_ | nodeSelector overrides the container node selector. |
| `commands` _string array_ | commands are executed (in order) against a Feature Store deployment.
Defaults to "feast apply" & "feast materialize-incremental $(date -u +'%Y-%m-%dT%H:%M:%S')" |


#### DefaultCtrConfigs



DefaultCtrConfigs k8s container settings that are applied by default

_Appears in:_
- [ContainerConfigs](#containerconfigs)
- [CronJobContainerConfigs](#cronjobcontainerconfigs)
- [RegistryServerConfigs](#registryserverconfigs)
- [ServerConfigs](#serverconfigs)

| Field | Description |
| --- | --- |
| `image` _string_ | image overrides the default container image. |


#### FeastCronJob



FeastCronJob defines a CronJob to execute against a Feature Store deployment.

_Appears in:_
- [FeatureStoreSpec](#featurestorespec)

| Field | Description |
| --- | --- |
| `annotations` _object (keys:string, values:string)_ | annotations are added to the CronJob metadata. |
| `jobSpec` _[JobSpec](#jobspec)_ | jobSpec specifies the desired behavior of a job. |
| `containerConfigs` _[CronJobContainerConfigs](#cronjobcontainerconfigs)_ | containerConfigs configures the CronJob container settings. |
| `schedule` _string_ | schedule is in Cron format, see https://en.wikipedia.org/wiki/Cron. |
| `timeZone` _string_ | timeZone is the time zone name for the given schedule, see https://en.wikipedia.org/wiki/List_of_tz_database_time_zones.
If not specified, this will default to the time zone of the kube-controller-manager process.
The set of valid time zone names and the time zone offset is loaded from the system-wide time zone
database by the API server during CronJob validation and the controller manager during execution.
If no system-wide time zone database can be found a bundled version of the database is used instead.
If the time zone name becomes invalid during the lifetime of a CronJob or due to a change in host
configuration, the controller will stop creating new new Jobs and will create a system event with the
reason UnknownTimeZone.
More information can be found in https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/#time-zones |
| `startingDeadlineSeconds` _integer_ | startingDeadlineSeconds is an optional deadline in seconds for starting the job if it misses scheduled
time for any reason.  Missed jobs executions will be counted as failed ones. |
| `concurrencyPolicy` _[ConcurrencyPolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#concurrencypolicy-v1-batch)_ | concurrencyPolicy specifies how to treat concurrent executions of a Job.
Valid values are:

- "Allow" (default): allows CronJobs to run concurrently;
- "Forbid": forbids concurrent runs, skipping next run if previous run hasn't finished yet;
- "Replace": cancels currently running job and replaces it with a new one |
| `suspend` _boolean_ | suspend tells the controller to suspend subsequent executions, it does
not apply to already started executions. |
| `successfulJobsHistoryLimit` _integer_ | successfulJobsHistoryLimit is the number of successful finished jobs to retain. Value must be non-negative integer. |
| `failedJobsHistoryLimit` _integer_ | failedJobsHistoryLimit is the number of failed finished jobs to retain. Value must be non-negative integer. |


#### FeastInitOptions



FeastInitOptions defines how to run a `feast init`.

_Appears in:_
- [FeastProjectDir](#feastprojectdir)

| Field | Description |
| --- | --- |
| `minimal` _boolean_ | minimal configures whether to use the minimal project template. |
| `template` _string_ | template selects the created project template. |


#### FeastProjectDir



FeastProjectDir defines how to create the feast project directory.

_Appears in:_
- [FeatureStoreSpec](#featurestorespec)

| Field | Description |
| --- | --- |
| `git` _[GitCloneOptions](#gitcloneoptions)_ | git configures cloning the feature repository. |
| `init` _[FeastInitOptions](#feastinitoptions)_ | init configures `feast init` project creation. |


#### FeatureStore



FeatureStore is the Schema for the featurestores API



| Field | Description |
| --- | --- |
| `apiVersion` _string_ | `feast.dev/v1`
| `kind` _string_ | `FeatureStore`
| `metadata` _[ObjectMeta](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#objectmeta-v1-meta)_ | Refer to Kubernetes API documentation for fields of `metadata`. |
| `spec` _[FeatureStoreSpec](#featurestorespec)_ | spec defines the desired state of FeatureStore. |
| `status` _[FeatureStoreStatus](#featurestorestatus)_ | status defines the observed state of FeatureStore. |


#### FeatureStoreRef



FeatureStoreRef defines which existing FeatureStore's registry should be used

_Appears in:_
- [RemoteRegistryConfig](#remoteregistryconfig)

| Field | Description |
| --- | --- |
| `name` _string_ | name is the name of the FeatureStore. |
| `namespace` _string_ | namespace is the namespace of the FeatureStore. |


#### FeatureStoreServices



FeatureStoreServices defines the desired feast services. An ephemeral onlineStore feature server is deployed by default.

_Appears in:_
- [FeatureStoreSpec](#featurestorespec)

| Field | Description |
| --- | --- |
| `offlineStore` _[OfflineStore](#offlinestore)_ | offlineStore configures the offline store service. |
| `onlineStore` _[OnlineStore](#onlinestore)_ | onlineStore configures the online store service. |
| `registry` _[Registry](#registry)_ | registry configures the registry service. |
| `ui` _[ServerConfigs](#serverconfigs)_ | ui creates a UI server container. |
| `deploymentStrategy` _[DeploymentStrategy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#deploymentstrategy-v1-apps)_ | deploymentStrategy configures the deployment strategy for Feast services. |
| `securityContext` _[PodSecurityContext](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#podsecuritycontext-v1-core)_ | securityContext configures the pod security context for Feast services. |
| `disableInitContainers` _boolean_ | disableInitContainers disables the 'feast repo initialization' initContainer. |
| `volumes` _[Volume](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#volume-v1-core) array_ | volumes specifies the volumes to mount in the FeatureStore deployment. A corresponding `VolumeMount` should be added to whichever feast service(s) require access to said volume(s). |
| `scaling` _[ScalingConfig](#scalingconfig)_ | scaling configures horizontal scaling for the FeatureStore deployment (e.g. HPA autoscaling).
For static replicas, use spec.replicas instead. |
| `podDisruptionBudgets` _[PDBConfig](#pdbconfig)_ | podDisruptionBudgets configures a PodDisruptionBudget for the FeatureStore deployment.
Only created when scaling is enabled (replicas > 1 or autoscaling). |
| `topologySpreadConstraints` _[TopologySpreadConstraint](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#topologyspreadconstraint-v1-core) array_ | topologySpreadConstraints defines how pods are spread across topology domains.
When scaling is enabled and this is not set, the operator auto-injects a soft
zone-spread constraint (whenUnsatisfiable: ScheduleAnyway).
Set to an empty array to disable auto-injection. |
| `affinity` _[Affinity](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#affinity-v1-core)_ | affinity defines the pod scheduling constraints for the FeatureStore deployment.
When scaling is enabled and this is not set, the operator auto-injects a soft
pod anti-affinity rule to prefer spreading pods across nodes. |


#### FeatureStoreSpec



FeatureStoreSpec defines the desired state of FeatureStore

_Appears in:_
- [FeatureStore](#featurestore)
- [FeatureStoreStatus](#featurestorestatus)

| Field | Description |
| --- | --- |
| `feastProject` _string_ | feastProject is the Feast project id. This can be any alphanumeric string with underscores and hyphens, but it cannot start with an underscore or hyphen. Required. |
| `feastProjectDir` _[FeastProjectDir](#feastprojectdir)_ | feastProjectDir defines how to create the feast project directory. |
| `services` _[FeatureStoreServices](#featurestoreservices)_ | services configures the Feast services for this FeatureStore. |
| `authz` _[AuthzConfig](#authzconfig)_ | authz configures authorization for Feast services. |
| `cronJob` _[FeastCronJob](#feastcronjob)_ | cronJob configures CronJobs to run against this FeatureStore deployment. |
| `batchEngine` _[BatchEngineConfig](#batchengineconfig)_ | batchEngine configures the batch compute engine settings. |
| `replicas` _integer_ | replicas is the desired number of pod replicas. Used by the scale sub-resource.
Mutually exclusive with services.scaling.autoscaling. |


#### FeatureStoreStatus



FeatureStoreStatus defines the observed state of FeatureStore

_Appears in:_
- [FeatureStore](#featurestore)

| Field | Description |
| --- | --- |
| `conditions` _[Condition](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#condition-v1-meta) array_ | conditions report the current status conditions. |
| `applied` _[FeatureStoreSpec](#featurestorespec)_ | applied shows the currently applied feast configuration, including any pertinent defaults. |
| `clientConfigMap` _string_ | clientConfigMap is the ConfigMap containing a client `feature_store.yaml` for this feast deployment. |
| `cronJob` _string_ | cronJob is the CronJob in this namespace for this feast deployment. |
| `feastVersion` _string_ | feastVersion is the resolved Feast version for this deployment. |
| `phase` _string_ | phase is the current lifecycle phase. |
| `serviceHostnames` _[ServiceHostnames](#servicehostnames)_ | serviceHostnames contains resolved service hostnames. |
| `replicas` _integer_ | replicas is the current number of ready pod replicas (used by the scale sub-resource). |
| `selector` _string_ | selector is the label selector for pods managed by the FeatureStore deployment (used by the scale sub-resource). |
| `scalingStatus` _[ScalingStatus](#scalingstatus)_ | scalingStatus reports the current scaling state of the FeatureStore deployment. |


#### GitCloneOptions



GitCloneOptions describes how a clone should be performed.

_Appears in:_
- [FeastProjectDir](#feastprojectdir)

| Field | Description |
| --- | --- |
| `url` _string_ | url is the repository URL to clone from. |
| `ref` _string_ | ref is a reference to a branch / tag / commit. |
| `configs` _object (keys:string, values:string)_ | configs are passed to git via `-c`.
e.g. http.sslVerify: 'false'
OR 'url."https://api:\${TOKEN}@github.com/".insteadOf': 'https://github.com/' |
| `featureRepoPath` _string_ | featureRepoPath is the relative path to the feature repo subdirectory. Default is 'feature_repo'. |
| `env` _[EnvVar](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envvar-v1-core)_ | env adds environment variables for cloning the repository. |
| `envFrom` _[EnvFromSource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envfromsource-v1-core)_ | envFrom adds environment sources for cloning the repository. |


#### JobSpec



JobSpec describes how the job execution will look like.

_Appears in:_
- [FeastCronJob](#feastcronjob)

| Field | Description |
| --- | --- |
| `podTemplateAnnotations` _object (keys:string, values:string)_ | podTemplateAnnotations are annotations to be applied to the CronJob's PodTemplate
metadata. This is separate from the CronJob-level annotations and must be
set explicitly by users if they want annotations on the PodTemplate. |
| `parallelism` _integer_ | parallelism specifies the maximum desired number of pods the job should
run at any given time. The actual number of pods running in steady state will
be less than this number when ((.spec.completions - .status.successful) < .spec.parallelism),
i.e. when the work left to do is less than max parallelism.
More info: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/ |
| `completions` _integer_ | completions specifies the desired number of successfully finished pods the
job should be run with.  Setting to null means that the success of any
pod signals the success of all pods, and allows parallelism to have any positive
value.  Setting to 1 means that parallelism is limited to 1 and the success of that
pod signals the success of the job.
More info: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/ |
| `activeDeadlineSeconds` _integer_ | activeDeadlineSeconds specifies the duration in seconds relative to the startTime that the job
may be continuously active before the system tries to terminate it; value
must be positive integer. If a Job is suspended (at creation or through an
update), this timer will effectively be stopped and reset when the Job is
resumed again. |
| `podFailurePolicy` _[PodFailurePolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#podfailurepolicy-v1-batch)_ | podFailurePolicy specifies the policy of handling failed pods. In particular, it allows to
specify the set of actions and conditions which need to be
satisfied to take the associated action.
If empty, the default behaviour applies - the counter of failed pods,
represented by the jobs's .status.failed field, is incremented and it is
checked against the backoffLimit. This field cannot be used in combination
with restartPolicy=OnFailure.

This field is beta-level. It can be used when the `JobPodFailurePolicy`
feature gate is enabled (enabled by default). |
| `backoffLimit` _integer_ | backoffLimit specifies the number of retries before marking this job failed. |
| `backoffLimitPerIndex` _integer_ | backoffLimitPerIndex specifies the limit for the number of retries within an
index before marking this index as failed. When enabled the number of
failures per index is kept in the pod's
batch.kubernetes.io/job-index-failure-count annotation. It can only
be set when Job's completionMode=Indexed, and the Pod's restart
policy is Never. The field is immutable.
This field is beta-level. It can be used when the `JobBackoffLimitPerIndex`
feature gate is enabled (enabled by default). |
| `maxFailedIndexes` _integer_ | maxFailedIndexes specifies the maximal number of failed indexes before marking the Job as
failed, when backoffLimitPerIndex is set. Once the number of failed
indexes exceeds this number the entire Job is marked as Failed and its
execution is terminated. When left as null the job continues execution of
all of its indexes and is marked with the `Complete` Job condition.
It can only be specified when backoffLimitPerIndex is set.
It can be null or up to completions. It is required and must be
less than or equal to 10^4 when is completions greater than 10^5.
This field is beta-level. It can be used when the `JobBackoffLimitPerIndex`
feature gate is enabled (enabled by default). |
| `ttlSecondsAfterFinished` _integer_ | ttlSecondsAfterFinished limits the lifetime of a Job that has finished
execution (either Complete or Failed). If this field is set,
ttlSecondsAfterFinished after the Job finishes, it is eligible to be
automatically deleted. When the Job is being deleted, its lifecycle
guarantees (e.g. finalizers) will be honored. If this field is unset,
the Job won't be automatically deleted. If this field is set to zero,
the Job becomes eligible to be deleted immediately after it finishes. |
| `completionMode` _[CompletionMode](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#completionmode-v1-batch)_ | completionMode specifies how Pod completions are tracked. It can be
`NonIndexed` (default) or `Indexed`.

`NonIndexed` means that the Job is considered complete when there have
been .spec.completions successfully completed Pods. Each Pod completion is
homologous to each other.

`Indexed` means that the Pods of a
Job get an associated completion index from 0 to (.spec.completions - 1),
available in the annotation batch.kubernetes.io/job-completion-index.
The Job is considered complete when there is one successfully completed Pod
for each index.
When value is `Indexed`, .spec.completions must be specified and
`.spec.parallelism` must be less than or equal to 10^5.
In addition, The Pod name takes the form
`$(job-name)-$(index)-$(random-string)`,
the Pod hostname takes the form `$(job-name)-$(index)`.

More completion modes can be added in the future.
If the Job controller observes a mode that it doesn't recognize, which
is possible during upgrades due to version skew, the controller
skips updates for the Job. |
| `suspend` _boolean_ | suspend specifies whether the Job controller should create Pods or not. If
a Job is created with suspend set to true, no Pods are created by the Job
controller. If a Job is suspended after creation (i.e. the flag goes from
false to true), the Job controller will delete all active Pods associated
with this Job. Users must design their workload to gracefully handle this.
Suspending a Job will reset the StartTime field of the Job, effectively
resetting the ActiveDeadlineSeconds timer too. |
| `podReplacementPolicy` _[PodReplacementPolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#podreplacementpolicy-v1-batch)_ | podReplacementPolicy specifies when to create replacement Pods.
Possible values are:
- TerminatingOrFailed means that we recreate pods
  when they are terminating (has a metadata.deletionTimestamp) or failed.
- Failed means to wait until a previously created Pod is fully terminated (has phase
  Failed or Succeeded) before creating a replacement Pod.

When using podFailurePolicy, Failed is the the only allowed value.
TerminatingOrFailed and Failed are allowed values when podFailurePolicy is not in use.
This is an beta field. To use this, enable the JobPodReplacementPolicy feature toggle.
This is on by default. |


#### KubernetesAuthz



KubernetesAuthz provides a way to define the authorization settings using Kubernetes RBAC resources.
https://kubernetes.io/docs/reference/access-authn-authz/rbac/

_Appears in:_
- [AuthzConfig](#authzconfig)

| Field | Description |
| --- | --- |
| `roles` _string array_ | roles are the Kubernetes RBAC roles to be deployed in the same namespace of the FeatureStore.
Roles are managed by the operator and created with an empty list of rules.
See the Feast permission model at https://docs.feast.dev/getting-started/concepts/permission
The feature store admin is not obligated to manage roles using the Feast operator, roles can be managed independently.
This configuration option is only providing a way to automate this procedure.
Important note: the operator cannot ensure that these roles will match the ones used in the configured Feast permissions. |


#### LocalRegistryConfig



LocalRegistryConfig configures the registry service

_Appears in:_
- [Registry](#registry)

| Field | Description |
| --- | --- |
| `server` _[RegistryServerConfigs](#registryserverconfigs)_ | server creates a registry server container. |
| `persistence` _[RegistryPersistence](#registrypersistence)_ | persistence configures registry persistence. |


#### OfflineStore



OfflineStore configures the offline store service

_Appears in:_
- [FeatureStoreServices](#featurestoreservices)

| Field | Description |
| --- | --- |
| `server` _[ServerConfigs](#serverconfigs)_ | server creates a remote offline server container. |
| `persistence` _[OfflineStorePersistence](#offlinestorepersistence)_ | persistence configures offline store persistence. |


#### OfflineStoreDBStorePersistence



OfflineStoreDBStorePersistence configures the DB store persistence for the offline store service

_Appears in:_
- [OfflineStorePersistence](#offlinestorepersistence)

| Field | Description |
| --- | --- |
| `type` _string_ | type is the persistence type you want to use. |
| `secretRef` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | secretRef holds data store parameters from "feature_store.yaml". "registry_type" & "type" fields should be removed. |
| `secretKeyName` _string_ | secretKeyName defaults to the selected store "type". |


#### OfflineStoreFilePersistence



OfflineStoreFilePersistence configures the file-based persistence for the offline store service

_Appears in:_
- [OfflineStorePersistence](#offlinestorepersistence)

| Field | Description |
| --- | --- |
| `type` _string_ | type is the file-based persistence type. |
| `pvc` _[PvcConfig](#pvcconfig)_ | pvc configures persistent volume claims for file-based persistence. |


#### OfflineStorePersistence



OfflineStorePersistence configures the persistence settings for the offline store service

_Appears in:_
- [OfflineStore](#offlinestore)

| Field | Description |
| --- | --- |
| `file` _[OfflineStoreFilePersistence](#offlinestorefilepersistence)_ | file configures file-based persistence. |
| `store` _[OfflineStoreDBStorePersistence](#offlinestoredbstorepersistence)_ | store configures store-based persistence. |


#### OidcAuthz



OidcAuthz defines the authorization settings for deployments using an Open ID Connect identity provider.
https://auth0.com/docs/authenticate/protocols/openid-connect-protocol

_Appears in:_
- [AuthzConfig](#authzconfig)

| Field | Description |
| --- | --- |
| `secretRef` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | secretRef references the secret containing OIDC configuration. |


#### OnlineStore



OnlineStore configures the online store service

_Appears in:_
- [FeatureStoreServices](#featurestoreservices)

| Field | Description |
| --- | --- |
| `server` _[ServerConfigs](#serverconfigs)_ | server creates a feature server container. |
| `persistence` _[OnlineStorePersistence](#onlinestorepersistence)_ | persistence configures online store persistence. |


#### OnlineStoreDBStorePersistence



OnlineStoreDBStorePersistence configures the DB store persistence for the online store service

_Appears in:_
- [OnlineStorePersistence](#onlinestorepersistence)

| Field | Description |
| --- | --- |
| `type` _string_ | type is the persistence type you want to use. |
| `secretRef` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | secretRef holds data store parameters from "feature_store.yaml". "registry_type" & "type" fields should be removed. |
| `secretKeyName` _string_ | secretKeyName defaults to the selected store "type". |


#### OnlineStoreFilePersistence



OnlineStoreFilePersistence configures the file-based persistence for the online store service

_Appears in:_
- [OnlineStorePersistence](#onlinestorepersistence)

| Field | Description |
| --- | --- |
| `path` _string_ | path is the filesystem path for file-based persistence. |
| `pvc` _[PvcConfig](#pvcconfig)_ | pvc configures persistent volume claims for file-based persistence. |


#### OnlineStorePersistence



OnlineStorePersistence configures the persistence settings for the online store service

_Appears in:_
- [OnlineStore](#onlinestore)

| Field | Description |
| --- | --- |
| `file` _[OnlineStoreFilePersistence](#onlinestorefilepersistence)_ | file configures file-based persistence. |
| `store` _[OnlineStoreDBStorePersistence](#onlinestoredbstorepersistence)_ | store configures store-based persistence. |


#### OptionalCtrConfigs



OptionalCtrConfigs k8s container settings that are optional

_Appears in:_
- [ContainerConfigs](#containerconfigs)
- [CronJobContainerConfigs](#cronjobcontainerconfigs)
- [RegistryServerConfigs](#registryserverconfigs)
- [ServerConfigs](#serverconfigs)

| Field | Description |
| --- | --- |
| `env` _[EnvVar](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envvar-v1-core)_ | env overrides container environment variables. |
| `envFrom` _[EnvFromSource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envfromsource-v1-core)_ | envFrom overrides container environment sources. |
| `imagePullPolicy` _[PullPolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#pullpolicy-v1-core)_ | imagePullPolicy overrides the container image pull policy. |
| `resources` _[ResourceRequirements](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#resourcerequirements-v1-core)_ | resources overrides the container resource requirements. |
| `nodeSelector` _map[string]string_ | nodeSelector overrides the container node selector. |


#### PDBConfig



PDBConfig configures a PodDisruptionBudget for the FeatureStore deployment.
Exactly one of minAvailable or maxUnavailable must be set.

_Appears in:_
- [FeatureStoreServices](#featurestoreservices)

| Field | Description |
| --- | --- |
| `minAvailable` _[IntOrString](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#intorstring-intstr-util)_ | minAvailable specifies the minimum number/percentage of pods that must remain available.
Mutually exclusive with maxUnavailable. |
| `maxUnavailable` _[IntOrString](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#intorstring-intstr-util)_ | maxUnavailable specifies the maximum number/percentage of pods that can be unavailable.
Mutually exclusive with minAvailable. |


#### PvcConfig



PvcConfig defines the settings for a persistent file store based on PVCs.
We can refer to an existing PVC using the `Ref` field, or create a new one using the `Create` field.

_Appears in:_
- [OfflineStoreFilePersistence](#offlinestorefilepersistence)
- [OnlineStoreFilePersistence](#onlinestorefilepersistence)
- [RegistryFilePersistence](#registryfilepersistence)

| Field | Description |
| --- | --- |
| `ref` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | ref references an existing PVC. |
| `create` _[PvcCreate](#pvccreate)_ | create specifies settings for creating a new PVC. |
| `mountPath` _string_ | mountPath is the path within the container at which the volume should be mounted.
Must start by "/" and cannot contain ':'. |


#### PvcCreate



PvcCreate defines the immutable settings to create a new PVC mounted at the given path.
The PVC name is the same as the associated deployment & feast service name.

_Appears in:_
- [PvcConfig](#pvcconfig)

| Field | Description |
| --- | --- |
| `accessModes` _[PersistentVolumeAccessMode](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#persistentvolumeaccessmode-v1-core) array_ | accessModes are the k8s persistent volume access modes. Defaults to ["ReadWriteOnce"]. |
| `storageClassName` _string_ | storageClassName is the name of an existing StorageClass to which this persistent volume belongs. Empty value
means that this volume does not belong to any StorageClass and the cluster default will be used. |
| `resources` _[VolumeResourceRequirements](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#volumeresourcerequirements-v1-core)_ | resources describes the storage resource requirements for a volume.
Default requested storage size depends on the associated service:
- 10Gi for offline store
- 5Gi for online store
- 5Gi for registry |


#### Registry



Registry configures the registry service. One selection is required. Local is the default setting.

_Appears in:_
- [FeatureStoreServices](#featurestoreservices)

| Field | Description |
| --- | --- |
| `local` _[LocalRegistryConfig](#localregistryconfig)_ | local configures a local registry deployment. |
| `remote` _[RemoteRegistryConfig](#remoteregistryconfig)_ | remote configures a remote registry endpoint. |


#### RegistryDBStorePersistence



RegistryDBStorePersistence configures the DB store persistence for the registry service

_Appears in:_
- [RegistryPersistence](#registrypersistence)

| Field | Description |
| --- | --- |
| `type` _string_ | type is the persistence type you want to use. |
| `secretRef` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | secretRef holds data store parameters from "feature_store.yaml". "registry_type" & "type" fields should be removed. |
| `secretKeyName` _string_ | secretKeyName defaults to the selected store "type". |


#### RegistryFilePersistence



RegistryFilePersistence configures the file-based persistence for the registry service

_Appears in:_
- [RegistryPersistence](#registrypersistence)

| Field | Description |
| --- | --- |
| `path` _string_ | path is the filesystem path or object store URI for registry persistence. |
| `pvc` _[PvcConfig](#pvcconfig)_ | pvc configures persistent volume claims for registry persistence. |
| `s3_additional_kwargs` _map[string]string_ | s3_additional_kwargs configures additional S3 settings for registry persistence. |
| `cache_ttl_seconds` _integer_ | cache_ttl_seconds defines the TTL (in seconds) for the registry cache. |
| `cache_mode` _string_ | cache_mode defines the registry cache update strategy.
Allowed values are "sync" and "thread". |


#### RegistryPersistence



RegistryPersistence configures the persistence settings for the registry service

_Appears in:_
- [LocalRegistryConfig](#localregistryconfig)

| Field | Description |
| --- | --- |
| `file` _[RegistryFilePersistence](#registryfilepersistence)_ | file configures file-based persistence. |
| `store` _[RegistryDBStorePersistence](#registrydbstorepersistence)_ | store configures store-based persistence. |


#### RegistryServerConfigs



RegistryServerConfigs creates a registry server for the feast service, with specified container configurations.

_Appears in:_
- [LocalRegistryConfig](#localregistryconfig)

| Field | Description |
| --- | --- |
| `image` _string_ | image overrides the default container image. |
| `env` _[EnvVar](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envvar-v1-core)_ | env overrides container environment variables. |
| `envFrom` _[EnvFromSource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envfromsource-v1-core)_ | envFrom overrides container environment sources. |
| `imagePullPolicy` _[PullPolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#pullpolicy-v1-core)_ | imagePullPolicy overrides the container image pull policy. |
| `resources` _[ResourceRequirements](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#resourcerequirements-v1-core)_ | resources overrides the container resource requirements. |
| `nodeSelector` _map[string]string_ | nodeSelector overrides the container node selector. |
| `tls` _[TlsConfigs](#tlsconfigs)_ | tls configures TLS settings for the server. |
| `logLevel` _string_ | logLevel sets the logging level for the server.
Allowed values: "debug", "info", "warning", "error", "critical". |
| `metrics` _boolean_ | metrics exposes Prometheus-compatible metrics for the Feast server when enabled. |
| `volumeMounts` _[VolumeMount](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#volumemount-v1-core) array_ | volumeMounts defines the list of volumes that should be mounted into the feast container.
This allows attaching persistent storage, config files, secrets, or other resources
required by the Feast components. Ensure that each volume mount has a corresponding
volume definition in the Volumes field. |
| `workerConfigs` _[WorkerConfigs](#workerconfigs)_ | workerConfigs defines the worker configuration for the Feast server.
These options are primarily used for production deployments to optimize performance. |
| `restAPI` _boolean_ | restAPI enables the REST API registry server. |
| `grpc` _boolean_ | grpc enables the gRPC registry server. Defaults to true if unset. |


#### RemoteRegistryConfig



RemoteRegistryConfig points to a remote feast registry server. When set, the operator will not deploy a registry for this FeatureStore CR.
Instead, this FeatureStore CR's online/offline services will use a remote registry. One selection is required.

_Appears in:_
- [Registry](#registry)

| Field | Description |
| --- | --- |
| `hostname` _string_ | hostname is the host address of the remote registry service - <domain>:<port>, e.g. `registry.<namespace>.svc.cluster.local:80`. |
| `feastRef` _[FeatureStoreRef](#featurestoreref)_ | feastRef references an existing `FeatureStore` CR in the same k8s cluster. |
| `tls` _[TlsRemoteRegistryConfigs](#tlsremoteregistryconfigs)_ | tls configures TLS settings for the remote registry. |


#### ScalingConfig



ScalingConfig configures horizontal scaling for the FeatureStore deployment.

_Appears in:_
- [FeatureStoreServices](#featurestoreservices)

| Field | Description |
| --- | --- |
| `autoscaling` _[AutoscalingConfig](#autoscalingconfig)_ | autoscaling configures a HorizontalPodAutoscaler for the FeatureStore deployment.
Mutually exclusive with spec.replicas. |


#### ScalingStatus



ScalingStatus reports the observed scaling state.

_Appears in:_
- [FeatureStoreStatus](#featurestorestatus)

| Field | Description |
| --- | --- |
| `currentReplicas` _integer_ | currentReplicas is the current number of pod replicas. |
| `desiredReplicas` _integer_ | desiredReplicas is the desired number of pod replicas. |


#### SecretKeyNames



SecretKeyNames defines the secret key names for the TLS key and cert.

_Appears in:_
- [TlsConfigs](#tlsconfigs)

| Field | Description |
| --- | --- |
| `tlsCrt` _string_ | tlsCrt defaults to "tls.crt". |
| `tlsKey` _string_ | tlsKey defaults to "tls.key". |


#### ServerConfigs



ServerConfigs creates a server for the feast service, with specified container configurations.

_Appears in:_
- [FeatureStoreServices](#featurestoreservices)
- [OfflineStore](#offlinestore)
- [OnlineStore](#onlinestore)
- [RegistryServerConfigs](#registryserverconfigs)

| Field | Description |
| --- | --- |
| `image` _string_ | image overrides the default container image. |
| `env` _[EnvVar](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envvar-v1-core)_ | env overrides container environment variables. |
| `envFrom` _[EnvFromSource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#envfromsource-v1-core)_ | envFrom overrides container environment sources. |
| `imagePullPolicy` _[PullPolicy](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#pullpolicy-v1-core)_ | imagePullPolicy overrides the container image pull policy. |
| `resources` _[ResourceRequirements](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#resourcerequirements-v1-core)_ | resources overrides the container resource requirements. |
| `nodeSelector` _map[string]string_ | nodeSelector overrides the container node selector. |
| `tls` _[TlsConfigs](#tlsconfigs)_ | tls configures TLS settings for the server. |
| `logLevel` _string_ | logLevel sets the logging level for the server.
Allowed values: "debug", "info", "warning", "error", "critical". |
| `metrics` _boolean_ | metrics exposes Prometheus-compatible metrics for the Feast server when enabled. |
| `volumeMounts` _[VolumeMount](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#volumemount-v1-core) array_ | volumeMounts defines the list of volumes that should be mounted into the feast container.
This allows attaching persistent storage, config files, secrets, or other resources
required by the Feast components. Ensure that each volume mount has a corresponding
volume definition in the Volumes field. |
| `workerConfigs` _[WorkerConfigs](#workerconfigs)_ | workerConfigs defines the worker configuration for the Feast server.
These options are primarily used for production deployments to optimize performance. |


#### ServiceHostnames



ServiceHostnames defines the service hostnames in the format of <domain>:<port>, e.g. example.svc.cluster.local:80

_Appears in:_
- [FeatureStoreStatus](#featurestorestatus)

| Field | Description |
| --- | --- |
| `offlineStore` _string_ | offlineStore is the offline store service hostname. |
| `onlineStore` _string_ | onlineStore is the online store service hostname. |
| `registry` _string_ | registry is the registry service hostname. |
| `registryRest` _string_ | registryRest is the registry REST service hostname. |
| `ui` _string_ | ui is the UI service hostname. |


#### TlsConfigs



TlsConfigs configures server TLS for a feast service. in an openshift cluster, this is configured by default using service serving certificates.

_Appears in:_
- [RegistryServerConfigs](#registryserverconfigs)
- [ServerConfigs](#serverconfigs)

| Field | Description |
| --- | --- |
| `secretRef` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | secretRef references the local k8s secret where the TLS key and cert reside. |
| `secretKeyNames` _[SecretKeyNames](#secretkeynames)_ | secretKeyNames defines the secret key names for TLS. |
| `disable` _boolean_ | disable will disable TLS for the feast service. useful in an openshift cluster, for example, where TLS is configured by default. |


#### TlsRemoteRegistryConfigs



TlsRemoteRegistryConfigs configures client TLS for a remote feast registry. in an openshift cluster, this is configured by default when the remote feast registry is using service serving certificates.

_Appears in:_
- [RemoteRegistryConfig](#remoteregistryconfig)

| Field | Description |
| --- | --- |
| `configMapRef` _[LocalObjectReference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#localobjectreference-v1-core)_ | configMapRef references the local k8s configmap where the TLS cert resides. |
| `certName` _string_ | certName defines the configmap key name for the client TLS cert. |


#### WorkerConfigs



WorkerConfigs defines the worker configuration for Feast servers.
These settings control gunicorn worker processes for production deployments.

_Appears in:_
- [RegistryServerConfigs](#registryserverconfigs)
- [ServerConfigs](#serverconfigs)

| Field | Description |
| --- | --- |
| `workers` _integer_ | workers is the number of worker processes. Use -1 to auto-calculate based on CPU cores (2 * CPU + 1).
Defaults to 1 if not specified. |
| `workerConnections` _integer_ | workerConnections is the maximum number of simultaneous clients per worker process.
Defaults to 1000. |
| `maxRequests` _integer_ | maxRequests is the maximum number of requests a worker will process before restarting.
This helps prevent memory leaks. Defaults to 1000. |
| `maxRequestsJitter` _integer_ | maxRequestsJitter is the maximum jitter to add to max-requests to prevent
thundering herd effect on worker restart. Defaults to 50. |
| `keepAliveTimeout` _integer_ | keepAliveTimeout is the timeout for keep-alive connections in seconds.
Defaults to 30. |
| `registryTTLSeconds` _integer_ | registryTTLSeconds is the number of seconds after which the registry is refreshed.
Higher values reduce refresh overhead but increase staleness. Defaults to 60. |


