---
title: "Tips for using templating in governance policies: Part 3"
date: 2025-03-19
---

Red Hat Advanced Cluster Management for Kubernetes (RHACM) governance provides an extensible framework for enterprises to introduce their own security and configuration policies that can be applied to managed Red Hat OpenShift or Kubernetes clusters. For more information on RHACM policies, I recommend that you read the Applying Policy-Based Governance at Scale Using Templates and Comply to standards using policy based governance blogs.

In this 3-part series, I will showcase a number of techniques you can use for templates in your RHACM policies. In part one I reviewed practices you can use to make your templates more readable and easier to maintain. Part two of this series took a look at more advanced template functionality and extended use cases for using Policies to manage clusters.

In this final installment, I will cover writing and debugging policies on a local machine and showcase some advanced templating that is available in recent RHACM releases.

## Policytools CLI for template development

The 2.12 release of RHACM introduced a policytools command-line interface (CLI) that you can use to interact with policies on your local machine to validate and debug template code issues. This first release has the ability to resolve templates using the `template-resolver` sub-command using an `oc` login session provided through a kubeconfig file.

The `template-resolver` subcommand accepts a file that contains a `Policy`, `ConfigurationPolicy`, or `object-template-raw` definition, after processing the template against a designated cluster the command outputs the full policy with all templates resolved.

Note: The PolicyGenerator in RHACM 2.11 introduced new ways to manage `object-templates-raw` definition.

When creating policies with `object-templates-raw` definition you can manage your code as a full ConfigurationFile or beginning with RHACM 2.11 you can store just the `object-templates-raw` definition in your source file and the `PolicyGenerator` will create the `ConfigurationPolicy` for you. There are options in the generator that let you have control how the `ConfigurationPolicy` is created, but if you need full control, you can continue to manage the definition in your source code.

Using an example template that has a number of lookups, ranges, and if statements, the `policytools` can help us develop complex policies like this and validate the output before committing to a source repository or applying it to the RHACM hub.

Note:

This policy attempts to find all `ClusterRoles` that are not used in any role bindings. This will never be compliant in a cluster, but does make use of a number of templating techniques.

Create a list of all ClusterRoles and bindings as follows:

```xml
object-templates-raw: |
  {{/* ##  create list of all ClusterRoles and bindings ## */}} 
  {{- $clusterRoles := (lookup "rbac.authorization.k8s.io/v1" "ClusterRole" "" "").items }}
  {{- $bindings := concat (lookup "rbac.authorization.k8s.io/v1" "ClusterRoleBinding" "" "").items 
                          (lookup "rbac.authorization.k8s.io/v1" "RoleBinding" "" "").items }}
  {{- range $cr := $clusterRoles }}
    {{/* ##  try to exit early from looping if role is found in binding ## */}}
    {{- $bindingFound := false }}
    {{- range $bnd := $bindings }}
      {{- if and (eq $bnd.roleRef.name $cr.metadata.name) (eq $bnd.roleRef.kind $cr.kind) }}
        {{- $bindingFound = true }}
        {{- break }}
      {{- end }}
    {{- end }}
    {{/* ##  skip CR if binding found ## */}}
    {{- if $bindingFound }}
      {{- continue }}
    {{- end }}
  - complianceType: mustnothave
    objectDefinition:
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1
      metadata:
        name: {{ $cr.metadata.name }}
  {{- end }}
```

When we run the `policytools template-resolver` against this policy, we will see all of the `ClusterRoles` that are not included in any bindings in our cluster. If there were errors in the template or formatting, those would be included in the output:

```output
$ policytools template-resolver policies/cluster-maintenance/alert-clusterrole-unused.yml
object-templates:
  - complianceType: mustnothave
    objectDefinition:
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRole
      metadata:
        name: access-to-brokers-submariner-crd
  - complianceType: mustnothave
    objectDefinition:
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRole
      metadata:
        name: aggregate-appsub-admin
... output truncated
```

## Combining hub and managed cluster templates

Occasionally, you may need to use data from the hub as part of a template that is executed on the managed cluster. We can embed hub templates within a managed cluster template.

Let’s look at a simple example to see how the templating is formatted. This assigns a value from the hub to a variable that will be evaluated on the managed cluster and used to create a `ConfigMap`:

```xml
object-templates-raw: |
  {{- $clusterName := "{{hub .ManagedClusterName  hub}}" }}
  - complianceType: musthave
    objectDefinition:
      apiVersion: v1
      kind: ConfigMap
      metadata:
        name: example
        namespace: default
     data:
       setkey: '{{ $clusterName }}'
```

## Using dates in policies

There are many use cases where comparing the date of an object is beneficial, such as automatically rotating a secret after a specific age or deleting namespaces that are considered abandoned after a period of time.

RHACM 2.9 added the `date`, `toDate`, `mustToDate`, and `now` functions. These functions return an object of type Time from the go time package. The `Time` type makes available additional functions such as `addDate`, and outputs such as `Seconds`, `Unix`, and `Compare`:

```xml
object-templates-raw: |
  {{- $nt := now }}
  {{- $maxAgeDays := 5 }}
  {{- $maxAgeSeconds := (($nt.AddDate 0 0 $maxAgeDays).Sub $nt).Seconds }}
  {{- range $secret := (lookup "v1" "Secret" "" "" "!kubernetes.io/legacy-token-last-used").items }}
    {{- $secretCreated := (toDate "2006-1-2T15:04:05Z" $secret.metadata.creationTimestamp) }}
    {{- $secretAgeSeconds := ($nt.Sub $secretCreated).Seconds }}
    {{- if and (contains "dev-cluster-access" $secret.metadata.name)
               (gt $secretAgeSeconds $maxAgeSeconds) }}
  - complianceType: mustnothave
    objectDefinition:
      apiVersion: v1
      kind: Secret
      metadata: {{ $secret.metadata | toRawJson | toLiteral }}
    {{- end }}
  {{- end }}
```

Details of the commands used in the template:

- `{{- $nt := now }}`: Creates a `Time` object of the current date and time.
- `(($nt.AddDate 0 0 $maxAgeDays).Sub $nt).Seconds` : Adds 0 years, 0 months, and the `maxAgeDays` to the current date/time using the AddDate function, then subtracts the current date/time leaving a time duration representing `maxAgeDays`, finally returns the seconds of the duration.
- `(toDate "2006-1-2T15:04:05Z" $secret.metadata.creationTimestamp)` : Converts the `creationTimeStamp` value of the secret to a `Time` object.  
- **Note:** 
    
    The first parameter to `toDate` is a format string indicating the format of the date/time to be parsed. Go time parsing is a bit different; 1=month, 2=day, 3=hour, and so on. “`2006-1-2T15:04:05Z”` tells the parser which position of the string to be parsed contains each segment. More details can be found in this blog.
    
- `(gt $secretAgeSeconds $maxAgeSeconds)` : Checks if the secret age in seconds is greater than the allowed max age.    
    This could have made use of the `Compare` function instead like `($secretCreated.Compare ($nt.AddDate 0 0 $maxAgeDays))`.

## Outputting whole objects

The previous example made use of `metadata: {{ $secret.metadata | toRawJson | toLiteral }}` to define the metadata section of the secret. Piping an object to `toRawJson | toLiteral` allows us to take advantage of how policy template resolution is processed. 

All policies are converted to JSON. The templates are resolved. Then they are converted back to YAML. Having our template output a raw JSON string allows us to include an entire element without having to write out each child element in the YAML. In the previous example, it saved us from having to write out the name and namespace, but there are other uses where it can be much more beneficial.

In the next example, we will validate that the `ClusterLogForwarder` is healthy. Looping through each condition, the status is set to `True`, and the whole condition is added to the policy output. This allows us to ensure all conditions have the correct value for the policy to be compliant without knowing which conditions are present in advance.

View the complete policy to see the inclusion of all the status sections of the `ClusterLogForwarder`. Because of how RHACM handles lists of objects, outputting the whole element gives more accurate results.

The value of status is set to `True` using the `set` function in `{{- $_ := set $c "status" "True" }}` from the Sprig library. Set takes a dictionary, the name of a key and a value, and adds the key if it does not exist as well as sets the value. A new instance of the dictionary is returned. Since the dictionary as represented by `$c` is updated, we can trap the return by assigning it to a blank identifier `$_`:

```xml
{{- $clf := (lookup "observability.openshift.io/v1" "ClusterLogForwarder" "openshift-logging" "collector") }}
    - complianceType: musthave
      objectDefinition:
        apiVersion: observability.openshift.io/v1
        kind: ClusterLogForwarder
        metadata:
          name: collector
          namespace: openshift-logging
        status:
          conditions:
       {{- range $c := $clf.status.conditions }}
         {{- $_ := set $c "status" "True" }}
          - {{ $c | toRawJson | toLiteral }}
       {{- end }}
          inputConditions:
       {{- range $c := $clf.status.inputConditions }}
         {{- $_ := set $c "status" "True" }}
          - {{ $c | toRawJson | toLiteral }}
       {{- end }}
```

CVE-2024-7387 remediation recommends removing rules from the admin and edit `ClusterRoles`. This example shows how using `toRawJson | toLiteral` can simplify adding a full object block versus going through all of the elements that define a rule:

```xml
  {{- $clusterRoleList := list 
                        (lookup "rbac.authorization.k8s.io/v1" "ClusterRole" "" "admin")
                        (lookup "rbac.authorization.k8s.io/v1" "ClusterRole" "" "edit")
  }}
  {{- range $cr := $clusterRoleList }}
  - complianceType: mustonlyhave
    objectDefinition:
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRole
      metadata:
        name: {{ $cr.metadata.name }}
      rules:
      {{- range $rule := $cr.rules }}
        {{- if or (not (has "build.openshift.io" $rule.apiGroups)) (not (has "buildconfigs" $rule.resources)) }}
        - {{ $rule | toRawJson | toLiteral }}
        {{- else }}
        - apiGroups: 
          {{- range $rule.apiGroups }}
            - {{ . | quote }}
          {{- end }}
          resources:
          {{- range $r := $rule.resources }}
            {{- if not (eq $r "builds" "builds/source" "builds/docker" "builds/custom") }}
            - {{ $r }}
            {{- end }}
          {{- end }}
            - builds/custom
          verbs: 
          {{- range $rule.verbs }}
            - {{ . }}
          {{- end }}
        {{- end }}
      {{- end }}
  {{- end }}
```

## Debugging techniques

When writing policies, you may come across situations where you are not sure why the output is not what you expect. Or, there is an error that is not clear as to the cause, such as the "failed to convert the resolved template to JSON".

Using the `policytools` CLI locally, we can make use of a few simple methods to identify and correct issues:

- Using `{{/* … */}}`, we can comment out sections, including multiple lines, of the template. This can be used to isolate problems and attempt to resolve them.
- Adding `{{ break }}` at the bottom of a range block when we want to only output the first item returned to make reviewing the output easier.
- `ConfigMaps` can be used to validate return values, and output YAML after the template process that can help visualize the structure without being bound by parsing rules that lead to the JSON errors above. This can be used to verify output values such as:
    - Simple template commands.
    - Complete output from a lookup to validate the structure of the returned object.
    - YAML created with complex ranges, if statements and other template functions.

```xml
object-templates-raw: |
    - complianceType: musthave
      objectDefinition:
        apiVersion: v1
        kind: ConfigMap
        metadata:
          name: test
          namespace: default
        data:
          template-value: |
            {{ fromClusterClaim "env"  }}
          object-raw: |
            {{ (lookup  "storage.k8s.io/v1" "StorageClass" "" "") | toRawJson | toLiteral  }}
          object-as-string: |
            apiVersion: storage.k8s.io/v1
            kind: StorageClass
            metadata:
              annotations:
                storageclass.kubernetes.io/is-default-class: "false"
              name: managed-zrs-file-csi-premium
            provisioner: file.csi.azure.com
            reclaimPolicy: Delete
            volumeBindingMode: WaitForFirstConsumer
            allowVolumeExpansion: true
            parameters:
              secretName: azure-storage-secret
              secretNamespace: kube-system
              skuName: Premium_ZRS
              storageAccount: 'example{{ fromClusterClaim "env" | substr 0 1  }}{{- if eq (fromClusterClaim "region") "eastus2" -}}use2{{- else if eq (fromClusterClaim  "region") "centralus" }}usc{{- end -}}01'
            mountOptions:
              - vers=1.0
          object-bad-indentation: |
            {{- range $pod := (lookup "v1" "Pod" "openshift-multus" "").items }}
              - complianceType: musthave
                objectDefinition:
                  kind: Pod
                  apiVersion: v1
                  metadata:
                    name: {{ $pod.metadata.name }}
                    namespace: openshift-multus
                  status:
                    containerStatuses:
                      {{- range $c := $pod.status.containerStatuses }}
                        {{- $_ := set $c "started" "true" }}
                        {{- $_ := set $c "ready" "true" }}
                      - {{ $c | toRawJson | toLiteral }}
                      {{- end }}
                      phase: Running
              {{ break }}
            {{- end }}
```

## Follow the series

If you haven't already, please check out part one of this series, where I outlined the usage of a number of template functions; and examples you can use to make your templates easier to read and maintain. In part two, we looked at validating cluster health with policies and how to use the `object-templates-raw` to expand templates for more complex use cases. In this final part of the series, you learned about more complex templating techniques and new capabilities like the `policytools` CLI for creating and debugging policies on a local system.

The post Tips for using templating in governance policies: Part 3 appeared first on Red Hat Developer.  
  

Go to Source
