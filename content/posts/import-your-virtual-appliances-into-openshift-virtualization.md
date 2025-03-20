---
title: "Import your virtual appliances into OpenShift Virtualization"
date: 2025-01-07
---

Virtual appliances (pre-configured virtual machine images ready to run on a hypervisor) have been commonly used for many years as a preferred mechanism to package and distribute applications. Virtual appliances eliminate the installation, configuration, and maintenance costs associated with running complex software stacks. 

The Open Virtual Appliance (OVA) format is a standardized way to package and distribute virtual appliances. OVA files can be deployed across various virtualization platforms like VMware, VirtualBox, KVM, and more. An OVA file is basically an archive file, which includes an OVF file with all the metadata about the virtual appliance, and one or more virtual disk images. The Open Virtual Format file (OVF) within the OVA archive is basically an XML file with all the metadata needed by a virtualization platform to create virtual machines based on the OVA. Throughout this article we will use the LAMP OVA as an example. LAMP OVA is publicly available from Bitnami—a well known distributor of virtual appliances. Looking into the OVF inside this OVA, we can see the hardware requirements as well as some basic information about what's in the OVA (for readability, the file has been truncated and only XML elements for basic requirements are shown):

```xml
<VirtualSystem ovf:id="common-lampstack-8.3-8.3.12-r4-debian-12-amd64">
    <Info>A virtual machine</Info>
    <Name>common-lampstack-8.3-8.3.12-r4-debian-12-amd64</Name>
    <OperatingSystemSection ovf:id="96" ovf:version="10" vmw:osType="debian10_64Guest">
      <Info>The kind of installed guest operating system</Info>
      <Description>Debian GNU/Linux 10 (64-bit)</Description>
    </OperatingSystemSection>
   
    <VirtualHardwareSection ovf:transport="com.vmware.guestInfo">
      <Info>Virtual hardware requirements</Info>
      <Item>
        <rasd:AllocationUnits>hertz * 10^6</rasd:AllocationUnits>
        <rasd:Description>Number of Virtual CPUs</rasd:Description>
        <rasd:ElementName>1 virtual CPU(s)</rasd:ElementName>
        <rasd:InstanceID>1</rasd:InstanceID>
        <rasd:ResourceType>3</rasd:ResourceType>
        <rasd:VirtualQuantity>1</rasd:VirtualQuantity>
      </Item>
      <Item>
        <rasd:AllocationUnits>byte * 2^20</rasd:AllocationUnits>
        <rasd:Description>Memory Size</rasd:Description>
        <rasd:ElementName>1024MB of memory</rasd:ElementName>
        <rasd:InstanceID>2</rasd:InstanceID>
        <rasd:Reservation>1024</rasd:Reservation>
        <rasd:ResourceType>4</rasd:ResourceType>
        <rasd:VirtualQuantity>1024</rasd:VirtualQuantity>
      </Item>
    </VirtualHardwareSection>
</VirtualSystem>
```

## Some simplification

When buying hardware, we have the option of fully specifying our needs and getting a custom build machine, but it is more common to select from a limited set of predefined hardware configurations.  Similarly, the OVF standard enables a very detailed specification of a virtual appliance, but in most cases, it is enough to select from a predefined set of virtual hardware configurations and match them with a set of cloneable disks which include the appliance’s software and configuration. 

This simplification practice is very common in the cloud where to the process of creating a virtual machine usually consists of 3 steps: selecting the desired software from pre-installed and pre-configured cloneable virtual disk images, then selecting the virtual hardware desired from a predefined set of virtual hardware configurations (instance types in AWS terminology or workload types in Azure terminology), and finally configuring the virtual networks and security. In this process, all the components covered in an OVA are present but separated into logical configuration steps. 

## Virtual appliances in Red Hat OpenShift Virtualization

Red Hat OpenShift Virtualization supports importing of OVAs through the migration toolkit for virtualization (MTV) operator, which works very well when you like to migrate virtual machines and OVA files from VMware vSphere. However, for a single OVA or a small number of OVAs, the overhead of the additional operator and the required supporting services (e.g., NFS) may be too much. Instead, OVAs can be imported directly by a relatively simple process described below. 

In OpenShift Virtualization, as is common in the cloud, the OVA is broken into one or more virtual disk images and the virtual hardware configuration. In OpenShift Virtualization the virtual disk images are saved as BootableVolumes, i.e., PVCs, onto which the virtual disk images have been copied. Virtual hardware configuration can be defined using two different mechanisms: a simple virtual appliance (a single virtual disk and a single virtual NIC) can be defined using an InstanceType, while more complex virtual appliances with multiple disks and NICs should be defined using VM Templates .

In this article we are going to cover the manual steps needed to import a simple OVA into a bootable volume, which basically is a pairing between the DataVolume with a cloneable, bootable virtual disk image and the recommended InstanceType which can be changed when a VM is created. 

### Step-by-step import process 

As mentioned above, we are going to use the publicly available bitnami LAMP OVA as an example. After downloading the OVA, the first step is to extract the files from the OVA archive, to do this we use the standard `tar` command:

```input
tar xvf bitnami-lampstack-8.3.12-r4-debian-12-amd64.ova
```

```output
common-lampstack-8.3-8.3.12-r4-debian-12-amd64.ovf                                                                                              
common-lampstack-8.3-8.3.12-r4-debian-12-amd64.mf                                                                                               
common-lampstack-8.3-8.3.12-r4-debian-12-amd64-disk-0.vmdk
```

As expected, the OVA includes the OVF metadata file and, in this case, a single virtual image disk in the VMDK format. OpenShift Virtualization supports IMG or QCOW images, therefore we need to convert the virtual disk; this conversion can be achieved with either the virt-v2v or qemu-img tools. virt-v2v is a powerful tool designed to transfer VMs from one virtualization platform to another, including converting the virtual disk to the right format. For our purposes, the simpler qemu-img tool is good enough, so we issue the following command:

```input
qemu-img convert -cpf vmdk -O qcow2 
      common-lampstack-8.3-8.3.12-r4-debian-12-amd64-disk-0.vmdk 
      bitnami-lampstack-8.3.12.qcow2
```

Now we need to extract the basic requirements from the OVF file. First we look for the minimal number of vCPUs required:

```input
grep -A 4 "Number of Virtual CPUs" common-lampstack-8.3-8.3.12-r4-debian-12-amd64.ovf 
```

```xml
       <rasd:Description>Number of Virtual CPUs</rasd:Description>
           <rasd:ElementName>1 virtual CPU(s)</rasd:ElementName>
           <rasd:InstanceID>1</rasd:InstanceID>
           <rasd:ResourceType>3</rasd:ResourceType>
           <rasd:VirtualQuantity>1</rasd:VirtualQuantity>
```

Then, we check for minimum amount of memory needed:

```input
grep -A 4 "Memory Size" common-lampstack-8.3-8.3.12-r4-debian-12-amd64.ovf
```

```xml
       <rasd:Description>Memory Size</rasd:Description>
           <rasd:ElementName>1024MB of memory</rasd:ElementName>
           <rasd:InstanceID>2</rasd:InstanceID>
           <rasd:Reservation>1024</rasd:Reservation>
           <rasd:ResourceType>4</rasd:ResourceType>
```

Finally, we look for the disk size:

```input
grep -A 4 "Disk" common-lampstack-8.3-8.3.12-r4-debian-12-amd64.ovf 
```

```xml
 <DiskSection>
       <Info>Virtual disk information</Info>
       <Disk ovf:capacity="10" ovf:capacityAllocationUnits="byte * 2^30" ovf:diskId="vmdisk1" ovf:fileRef="file1" ovf:format="http://www.vmware.com/interfaces/specifications/vmdk.html#streamOptimized"/>
     </DiskSection>
```

Hence, this OVA requires one virtual CPU, 1 GiB of memory and 10 GiBs of disk. To meet these requirements, we could use one the OpenShift Virtualization pre-defined instance types, in particular for this OVA the u1.small (1 vCPUs, 2GB of memory) instance type seems appropriate. Alternatively, we can create a custom instance type to match the requirements exactly, which can be achieved with the virtctl command, which greatly simplifies management and use of OpenShift Virtualization specific CRDs. However, before invoking `virtctl`, we need to log in to our target OpenShift Virtualization cluster. For this we’ll use the API token (which can be easily generated from the cluster webUI); and select the project/namespace (if different from the one selected when you log in):

```input
oc login --token=<YOUR-API-TOKEN> --server=<YOUR OCP SERVER API URL>
```

```output
Logged into <YOUR-OCP-SERVER> as "<YOUR-OCP-USERID>" using the token provided.
    You have access to the following projects and can switch between them with 'oc project <projectname>':
      * br-scratch
        openshift-virtualization-os-images
        ova-import-demo
        windows-builder
    Using project "br-scratch".
```

```input
oc project ova-import-demo 
```

```output
Now using project "ova-import-demo" on server <YOUR-OCP-SERVER-URL>
```

Now we could create a new instance type with the hardware requirements from the OVA with `virtctl` as follows:

```input
virtctl create instancetype --name small-instance-for-ova --cpu 1 --memory 1Gi
```

```output
apiVersion: instancetype.kubevirt.io/v1beta1
    kind: VirtualMachineClusterInstancetype
    metadata:
     creationTimestamp: null
     name: small-instance-for-ova
    spec:
     cpu:
       guest: 1
     memory:
       guest: 1Gi
```

The next step is to upload the virtual disk image. For this, we use `virtctl` again:

```input
virtctl image-upload dv bitnami-lampstack-8.3.12 --force-bind --insecure --size=10Gi 
    --image-path bitnami-lampstack-8.3.12.qcow2
```

```output
PVC br-scratch/bitnami-lampstack-8.3.12 not found 
    DataVolume br-scratch/bitnami-lampstack-8.3.12 created
    Waiting for PVC bitnami-lampstack-8.3.12 upload pod to be ready...
    Pod now ready
    Uploading data to https://cdi-uploadproxy-openshift-cnv.apps.cnv2.engineering.redhat.com
    822.99 MiB / 822.99 MiB [=========================================================================] 100.00% 1m14s
    Uploading data completed successfully, waiting for processing to complete, you can hit ctrl-c without interrupting the progress
    Processing completed successfully
```

As can be seen from the output, the command created a DataVolume (backed by a PVC) and populated it with the data from the disk image; now all that is left for us is to create a bootable volume (AKA, data source) that pairs between the PVC and an InstanceType. Unfortunately, `virtctl` doesn’t generate manifests for DataSource, so we need to create it manually to match the following YAML:

```input
apiVersion: cdi.kubevirt.io/v1beta1
    kind: DataSource
    metadata:
     generation: 4
     labels:
       instancetype.kubevirt.io/default-instancetype: u1.small
       instancetype.kubevirt.io/default-preference: ubuntu
     name: bitnami-lamp-stack-8.3.12-u1.small
    spec:
     source:
       pvc:
         name: bitnami-lampstack-8.3.12
         namespace: <YOUR-NAMESPACE>
```

Let's verify that the data source has been properly created. First we check whether the instance type has been properly set:

```input
oc get datasources.cdi.kubevirt.io bitnami-lampstack-
    8.3.12-r4-debian-12-amd64 -o json | jq '.metadata.labels' 
```

```output
{ 
     "instancetype.kubevirt.io/default-instancetype": "u1.large", 
     "instancetype.kubevirt.io/default-preference": "ubuntu" 
    }
```

And that the data source is pointing to the PVC where the OVA's virtual disk was upload to:

```input
oc get datasources.cdi.kubevirt.io bitnami-lampstack-8.3.12-r4-debian-12-amd64 -o json | jq '.status.source'
```

```output
{
      "pvc": {
        "name": "bitnami-lampstack-8.3.12-r4-debian-12-amd64",
        "namespace": "ova-import-demo"
      }
    }
```

We are now ready to start creating virtual machines for our imported OVA. To demonstrate this, we will select the **Create VirtualMachine -> From InstanceType** function in the OpenShift Virtualization console as shown in Figure 1, then select the **project (namespace)** where the OVA was imported to as shown in Figure 2. Then select the **bootable volume** that was created as part of the import process, validate that the default instance type is selected (and if desired, change it), then validate the rest of the VM configuration and click the **Create VirtualMachine** button (Figure 3) and wait until the new virtual machine is running.

<figure>

![Creating VMs from imported OVA](https://developers.redhat.com/sites/default/files/create-vm-from-instance-type_0.png)

<figcaption>

Figure 1: Creating VMs from imported OVA: (1) From the main menu select Virtualization; (2) then select Virtual Machines; (3) click Create VirtualMachine; (4) and select From InstanceType.

</figcaption>

</figure>

<figure>

![Creating VMs from imported OVA](https://developers.redhat.com/sites/default/files/create-vm-from-instance-type-select-namespace.png)

<figcaption>

Figure 2: Selecting volume to boot from: (1) Open the volumes project dropdown; and (2) select the project where the virtual disk from the OVA was uploaded to.

</figcaption>

</figure>

<figure>

![create vm configuration](https://developers.redhat.com/sites/default/files/fig3-create-vm-configuration.png)

<figcaption>

Figure 3: New virtual machine configuration page: (1) select the volume where virtual disk from OVA was upload to; (2) validate the default instance type (which can be changed if desired); (3) validate the rest of the VM configuration parameters; (4) and click the Create VirtualMachine button.

</figcaption>

</figure>

After a while the new VM is running and ready to use. Clearly for OVA usage instructions, you will need to get the documentation from the OVA provider. For this particular OVA, we see in the console a link to documentation as well as initial credentials (see Figure 4).

<figure>

![virtual-machine-details](https://developers.redhat.com/sites/default/files/fig4-create-vm-status.png)

<figcaption>

Figure 4: Virtual Machine details page: Once the status reaches "Running", the VM is ready to use; application specific configuration can can be done in the VM console.

</figcaption>

</figure>

### Putting the pieces together

While the step-by-step process described above is fairly straightforward, it would be better to have a tool that automates the entire process. To demonstrate this concept, we developed `ovaimporter`, a small utility (not an official utility), that runs as a Pod in the cluster that takes an OVAs either from your machine or directly from the OVA’s publisher web site, and automatically creates a bootable volume. This utility is available here. The following Figures show `ovaimporter` at work.

First, we need to log in to the cluster and select a **project/namespace.** We need to do the same for this tool, as shown in Figure 5. Once logged in you can either download an OVA directly from the vendor by filling up the URL file (Figure 6), or uploading from the local file system by drag and drop, or by clicking the **Select File** button and selecting the file (Figure 7).

<figure>

![ovaimporter-ocp-login-page](https://developers.redhat.com/sites/default/files/fig5-ocp-login.png)

<figcaption>

Figure 5: The  OpenShift Container Platform login page where (1) the user enters the full URL to OCP API server, (2) the token and; (3) the project where OVA components are uploaded to.

</figcaption>

</figure>

<figure>

![import-ova-from-url](https://developers.redhat.com/sites/default/files/fig6-ova-from-url.png)

<figcaption>

Figure 6: OVAs can be imported directly from the web by giving the full URL to the online OVA.

</figcaption>

</figure>

<figure>

![import-ova-from-file](https://developers.redhat.com/sites/default/files/fig7-ova-from-file.png)

<figcaption>

Figure 7: Alternatively, OVAs can be upload to OpenShift Virtualization from local file system on your machine.

</figcaption>

</figure>

In both cases, after the file has been saved by the `ovaimporter`, clicking the **Proceed** button will open up the progress page (Figure 8) and initiate the automated importing process. Once the process is finished, clicking the **Next OVA** button will bring you back to the OVA source page where you can repeat the import process for another OVA.

<figure>

![import-ova-progress-page](https://developers.redhat.com/sites/default/files/fig8-progress.png)

<figcaption>

Figure 8: Once the OVA has been selected, the progress page shows how the importing process progresses; and, when the process finishes, user can proceed and start again with another OVA.

</figcaption>

</figure>

## Wrapping up

That's it! While the terminology may require some getting used to, in this article we’ve shown how easy it is to import an OVA into OpenShift Virtualization

The post Import your virtual appliances into OpenShift Virtualization appeared first on Red Hat Developer.  
  

Go to Source
