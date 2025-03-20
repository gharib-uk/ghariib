---
title: "Build a Developer platform with Cyclops and Kubernetes"
date: 2025-03-07T00:00:00.000Z
draft: false
type: posts
categories: 
- kubernetes
---
# Build a Developer platform with Cyclops and Kubernetes

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Kubernetes can be complex to manage, but with [Cyclops](https://cyclops-ui.com/), you can deploy applications to [Kubernetes](https://docs.digitalocean.com/products/kubernetes/) in just a few clicks. Cyclops abstracts the complexity of Kubernetes, providing a reliable and customizable user experience for your developers.

Running [Cyclops](https://cyclops-ui.com/) on [DigitalOcean Managed Kubernetes](https://docs.digitalocean.com/products/kubernetes/) offers several benefits. You can manage your apps, databases, and networking, making Cyclops and Kubernetes the only tools your developers need to ship applications and products.

In this tutorial, you will:

-   Create a Kubernetes cluster on [DigitalOcean](https://cloud.digitalocean.com/).
-   Set up Cyclops.
-   Deploy an application through Cyclops.
-   Provision a database and connect your application to it.

Once you complete the tutorial, you will have built a developer platform where your developers can deploy applications and provision databases without the hassle of managing Kubernetes and the cloud. You can also extend Cyclops with your own templates to cover more use cases specific to your applications and organization.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

To complete this tutorial, you will need:

-   [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) to manage your Kubernetes cluster.
-   DigitalOcean account with [Kubernetes cluster](https://cloud.digitalocean.com/kubernetes/clusters) deployed.

[Step 1 - Create a Kubernetes cluster](#step-1-create-a-kubernetes-cluster)[](#step-1-create-a-kubernetes-cluster)
------------------------------------------------------------------------------------------------------------------

To create a Kubernetes cluster on DigitalOcean, visit [https://cloud.digitalocean.com/kubernetes/clusters/new](https://cloud.digitalocean.com/kubernetes/clusters/new).

You can follow the steps in this guide on [Setting up a Kubernetes Cluster on DigitalOcean](https://docs.digitalocean.com/products/kubernetes/getting-started/).

Here, you can configure the region, the version of your cluster, and how many nodes you will need. You can leave all the options on default.

Before clicking the `Create cluster` button, **make sure you tick the** `Add database operator` so you can deploy databases later on in the tutorial.

![Image 1](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.21.05%E2%80%AFPM.png)

You can add a special name to your Kubernetes cluster, then click the `Create cluster` and give it a couple of minutes until the Kubernetes cluster is fully up.

[Step 2 - Install Cyclops into your cluster](#step-2-install-cyclops-into-your-cluster)[](#step-2-install-cyclops-into-your-cluster)
------------------------------------------------------------------------------------------------------------------------------------

Now that your Kubernetes cluster is up and running on DigitalOcean, you can inspect it by clicking on it in the Kubernetes cluster list.

![Image 2](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.21.16%E2%80%AFPM.png)

Cyclops is part of the DigitalOcean Kubernetes marketplace, so you can install it by going to the `Marketplace` tab and searching for Cyclops. You can install it by simply clicking the install button, and everything Cyclops needs will be installed into your Kubernetes cluster.

![Image 3](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.21.27%E2%80%AFPM.png)

To access the Cyclops UI, you will need to expose it outside of the cluster. To do that, you will need to download the config file from the `Overview` tab by clicking the `Download Config File` button.

After you download the kubeconfig file, you can configure your `kubectl` to access your DO cluster by setting the environment variable `KUBECONFIG` to point to your kubeconfig file:

```
export KUBECONFIG=~/Downloads/<name of your cluster>-kubeconfig.yaml  
```

You can now check if Cyclops is running as expected in your cluster. You can do it with the following command:

```
kubectl get pods -n cyclops  
```

Which will have an output similar to this:

```
[secondary_label Output]  
NAME                            READY   STATUS    RESTARTS   AGE  
cyclops-ctrl-5b8bf97fc8-mh6vw   1/1     Running   0          144m  
cyclops-ui-64c4cdd7f7-mzgqh     1/1     Running   0          144m  
```

Once both pods are running, you can port forward Cyclops to access it:

```
kubectl port-forward svc/cyclops-ui 3000:3000 -n cyclops  
```

Cyclops is now available on [http://localhost:3000/](http://localhost:3000/).

[Step 3 - Deploy an app](#step-3-deploy-an-app)[](#step-3-deploy-an-app)
------------------------------------------------------------------------

Once you have your Cyclops ready, you can deploy a brand-new application using it. When you open Cyclops ([http://localhost:3000/](http://localhost:3000/)), you can create a new app by clicking the `Add module` button.

Here, you can choose from a couple of templates to create your applications from. You can add your own templates to cover your use cases whenever you want. You can learn more on how to do that in this official documentation on [Cyclops Templates](https://cyclops-ui.com/docs/templates/).

For the sake of the demo, you can use the `app-template`, which comes with a couple of fields you can play with:

-   `General` - you can set the docker image you want to run and the env variables
-   `Scaling` - set the number of instances of your applications and how many resources you need
-   `Networking` - configure how your app can be accessed

![Image 4](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.21.42%E2%80%AFPM.png)

As shown in the image above, you can set `Expose` to `external`, which will expose your app so you can access it outside of your Kubernetes cluster.

You can now click `Deploy` to bring your app to life.

![Image 5](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.21.53%E2%80%AFPM.png)

After a couple of minutes, your application is up, and if you check your `Service`, you will find an IP address where you can access your application (If the `externalIP` is saying pending, give it a minute to create a [load balancer](https://cloud.digitalocean.com/networking/load_balancers)). Copy the IP and paste it into your browser.

Without getting into the intricacies of Kubernetes or DigitalOcean, you are now running an application on Kubernetes that anybody can access, and you can simply scale, update, or manage it however you want.

[Step 4 - Provision a database](#step-4-provision-a-database)[](#step-4-provision-a-database)
---------------------------------------------------------------------------------------------

If you are developing an application, it’s likely your app will need some kind of storage. DigitalOcean provides a bunch of [managed databases](/products/managed-databases) you can use.

You can create a new application on DigitalOcean and connect your application to it, all from the Cyclops UI.

In the same way you created your first app in Cyclops, **you can provision a database**. From the templates, pick the `mysql` template, which is specifically made for the [DigitalOcean database operator](/products/managed-databases-mysql).

![Image 6](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.22.07%E2%80%AFPM.png)

After you select the size of the database and its region, you can hit `Deploy`.

![Image 7](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.22.18%E2%80%AFPM.png)

You can now see your new database in Cyclops, as well as in DigitalOcean, at [https://cloud.digitalocean.com/databases](https://cloud.digitalocean.com/databases).

Usually, you would now need to figure out the credentials of your database and make sure that your application can access it. However, the Database operator from DigitalOcean stored all of that info in your Kubernetes cluster, and the template you used for the demo app can reuse that data to connect to your database **without you having to manually handle secrets.**

The `app-template` you used will mount all of the database credentials to your application via environment variables you can use in your code to access the database. The only thing you need to do is tell Cyclops which database to connect to.

You need to open your `my-demo-app` in Cyclops and click `Edit`. You just need to populate the MySQL connection data with the name of your MySQL instance and the database you want to connect to (`defaultdb` gets created automatically).

![Image 8](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.22.34%E2%80%AFPM.png)

When you hit `Deploy`, your app will have all the credentials populated via env variables and your app can access the database you just created.

You can check that yourself by viewing the Kubernetes deployment through Cyclops and clicking the `View Manifest`. You will see a pop-up window with your deployment configuration now containing the following environment variables:

![Image 9](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/Cyclops-Digitalocean/Screenshot%202025-03-05%20at%2012.22.46%E2%80%AFPM.png)

No secrets handling or copy-pasting stuff; leave it to DigitalOcean, Kubernetes, and Cyclops.

All of these templates can be extended to support other databases or other kinds of applications. It’s completely up to you. This tutorial shows how Cyclops combined with [Kubernetes operators](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/#operators-in-kubernetes) can make developers power users when it comes to deploying without making them onboard to a ton of new technologies.

[Clean up](#clean-up)[](#clean-up)
----------------------------------

To delete all the resources, you will need to delete the [Kubernetes cluster](https://cloud.digitalocean.com/kubernetes/clusters), the [managed database](https://cloud.digitalocean.com/databases), and the [load balancer](https://cloud.digitalocean.com/networking/load_balancers) if you have exposed your application outside of the cluster.

[Am I a Platform Engineer now?](#am-i-a-platform-engineer-now)[](#am-i-a-platform-engineer-now)
-----------------------------------------------------------------------------------------------

With the setup you just created, you made a skeleton of a developer platform. You have enabled self-service for other developers so they can provision resources and deploy applications on their own to move faster.

You can now expand on this concept by creating more templates for different use cases for your teams and play around with different Kubernetes operators to get more flexibility.

**Cyclops is open-source**, so feel free to check its [GitHub repository](https://github.com/cyclops-ui/cyclops) and leave a star! ⭐

#### [Source](https://www.digitalocean.com/community/tutorials/build-developer-platform-kubernetes-cyclops)

<br/>
---
