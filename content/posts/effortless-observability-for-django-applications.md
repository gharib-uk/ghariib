---
title: "Effortless observability for Django applications"
date: 2025-03-19
---

Observability is critical for web operations to ensure that the application is working as expected and to identify any potential issues. However, setting up observability has traditionally been challenging because it can take hours to set up all the infrastructure, instrument your code and enable observability in production. But now there is a better way using native support for Django in Charmcraft and Rockcraft which has observability built in and ready to go!

In this post, we will start with a Django application built using the native support for Django in Rockcraft and Charmcraft and deployed with Juju, and add observability to it using just a few commands. This shows a key benefit of bringing the Juju ecosystem (which includes most of the plumbing you’d expect from a modern cloud, like observability, databases, ingressing and so on) to a web application using charms as we will be able to set up observability in a matter of minutes rather than hours. Rockcraft and Charmcraft also natively support Flask, FastAPI and Go and the following instructions will work similarly for those as well.

We have set up a Django application by following the Write your first Kubernetes charm for a Django app tutorial, completing it up to finishing The development cycle section. By this stage, the Django app should be deployed, integrated with PostgreSQL and the Nginx ingress integrator, and should look something like this:

```
$ juju status --relations

Model               Controller      Cloud/Region        Version  SLA          Timestamp
django-hello-world  dev-controller  microk8s/localhost  3.6.3    unsupported  00:52:54+11:00

App                       Version  Status  Scale  Charm                     Channel        Rev  Address         Exposed  Message
django-hello-world                 active      1  django-hello-world                         1  10.152.183.175  no       
nginx-ingress-integrator  24.2.0   active      1  nginx-ingress-integrator  latest/stable  130  10.152.183.214  no       Ingress IP(s): 127.0.0.1
postgresql-k8s            14.15    active      1  postgresql-k8s            14/stable      495  10.152.183.83   no       

Unit                         Workload  Agent  Address      Ports  Message
django-hello-world/0*        active    idle   10.1.157.79         
nginx-ingress-integrator/0*  active    idle   10.1.157.78         Ingress IP(s): 127.0.0.1
postgresql-k8s/0*            active    idle   10.1.157.77         Primary

Integration provider                  Requirer                              Interface          Type     Message
django-hello-world:secret-storage     django-hello-world:secret-storage     secret-storage     peer     
nginx-ingress-integrator:ingress      django-hello-world:ingress            ingress            regular  
nginx-ingress-integrator:nginx-peers  nginx-ingress-integrator:nginx-peers  nginx-instance     peer     
postgresql-k8s:database               django-hello-world:postgresql         postgresql_client  regular  
postgresql-k8s:database-peers         postgresql-k8s:database-peers         postgresql_peers   peer     
postgresql-k8s:restart                postgresql-k8s:restart                rolling_op         peer     
postgresql-k8s:upgrade                postgresql-k8s:upgrade                upgrade            peer
```

At this point, if you send a request using `curl`, it should return `Hello, world!`

```
$ curl http://django-hello-world 
    --resolve django-hello-world:80:127.0.0.1 
Hello, world!
```

The next step is to deploy observability and integrate it with your Django app. For that, we need to enable a load balancer on Microk8s which is required for Traefik which will serve as ingress for the Canonical observability stack:

```
$ IPADDR=$(ip -4 -j route get 2.2.2.2 | jq -r '.[] | .prefsrc')
$ sudo microk8s enable metallb:$IPADDR-$IPADDR
$ microk8s status --wait-ready
```

Now we can deploy the Canonical Observability Stack which will deploy Grafana, Loki, Prometheus and Alertmanager as well as a catalogue to access all of the services and Traefik as an ingress:

```
$ juju deploy cos-lite --trust
$ juju integrate django-hello-world grafana
$ juju integrate django-hello-world prometheus
$ juju integrate django-hello-world loki
```

We have just tasked Juju with quite a bit so let’s give it a few minutes to complete. Meanwhile, we can monitor the progress:

```
$ juju status --watch 2s
```

Once everything is in `active` and `idle`, let’s take a look at the dashboards that have been created. Run the following command to retrieve the observability endpoints:

```
$ juju show-unit catalogue/0 | grep url
$ juju run grafana/leader get-admin-password
```

This will display a few URLs along with the default admin password. The one we are interested in has the `grafana` postfix and should look something like http://10.18.66.154/django-hello-world-grafana (your IP address will vary). To access the dashboards overview append the `/dashboards` postfix, then click General and select the Django Operator dashboard. You should see something like this:

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_2420,h_1172/https://ubuntu.com/wp-content/uploads/7999/Screenshot-2025-03-13-at-1.46.39%E2%80%AFpm.jpg)

Since we haven’t made any requests yet, the dashboard is currently reporting no activity. Let’s send the following curl command a few times to generate traffic:

```
$ curl http://django-hello-world --resolve django-hello-world:80:127.0.0.1 
```

Over the next few minutes, the data will update and should look something like this:

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_2418,h_1168/https://ubuntu.com/wp-content/uploads/f12b/Screenshot-2025-03-13-at-1.47.14%E2%80%AFpm.jpg)

So far, we have successfully deployed our app and the observability stack – getting dashboards without having to define them ourselves! We can also access the application logs. To view them, select  `django-hello-world` from the dropdown menu next to the Juju application filter at the top of the page:

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_2056,h_856/https://ubuntu.com/wp-content/uploads/e01e/Screenshot-2025-03-13-at-1.47.56%E2%80%AFpm.jpg)

When you scroll down, you will see the logs of the application:

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_2418,h_406/https://ubuntu.com/wp-content/uploads/318a/Screenshot-2025-03-13-at-1.48.28%E2%80%AFpm.jpg)

And that’s a wrap! We created a Django application and integrated it with observability. We explored the default dashboard giving us insights into how the application is being used, and also accessed the application logs. With a few commands we added a full suite of observability tools to our Django app without needing to manually handle the deployment and configuration of each of the apps. 

If you want to learn more about creating OCI images and charms for your Django apps, you can follow the Write your first Kubernetes charm for a Django app tutorial and take advantage of the complete Juju ecosystem. Feel free to reach out on our community matrix channel if you have any questions or would like to get in touch!

Go to Source
