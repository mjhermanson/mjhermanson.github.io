---
title: Lab 3 - Deploying an App with S2I
workshops: ocp4_dcmetromap
workshop_weight: 13
layout: lab
---

# Source to Image (S2I)
One of the useful components of OpenShift is its source-to-image capability.  S2I is a framework that makes it easy to turn your source code into runnable images.  The main advantage of using S2I for building reproducible docker images is the ease of use for developers.  You'll see just how simple it can be in this lab.

## Let's build a node.js web server using S2I
We can do this either via the command line or the web console.  You decide which you'd rather do and follow the steps below.

{{< panel_group >}}
{{% panel "CLI Steps" %}}

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>

```bash
$ oc new-app --name=dc-metro-map https://github.com/RedHatGov/openshift-workshops.git --context-dir=dc-metro-map
$ oc expose service dc-metro-map
```

{{% /panel %}}

{{% alert info %}}
When using the CLI, OpenShift automatically detects the source code type and select the nodejs builder image.
{{% /alert %}}

{{% panel "Web Console Steps" %}}

<blockquote>
> If needed, switch to the developer view
</blockquote>
<img src="../images/dev_view.png" width="600"><br/>

<blockquote>

>Click 'Add"
</blockquote>

<img src="../images/add.png" width="150"><br/>

<blockquote>
Click "From Catalog" and type filter 'nodejs' in the 'Search Catalog' field.
</blockquote>

<img src="../images/from_catalog.png" width="700"><br/>

<blockquote>
Select Node.js 
</blockquote>

<img src="../images/ocp-lab-s2i-filternode2.png" width="800"><br/>

<blockquote>
Select 'Create Application', then select version Builder Image version '8'
</blockquote>
<blockquote>
Select 'advanced options'
</blockquote>

<img src="../images/ocp-lab-s2i-advancedoptions.png" width="700"><br/>

<p>
<table>
<tr><td><b>Git Repository URL</b></td><td><a href>https://github.com/RedHatGov/openshift-workshops.git</a></td></tr>
<tr><td><b>Context Dir</b></td><td>/dc-metro-map</td></tr>
<tr><td><b>Name</b></td><td>dc-metro-map</td></tr>
</table>
</p>

<blockquote>
Scroll to the bottom and click "Create"
</blockquote>

{{% /panel %}}
{{< /panel_group >}}

## Check out the build details
We can see the details of what the S2I builder did.  This can be helpful to diagnose issues if builds are failing.

<i class="fa fa-magic fa-2x"></i> TIP: For a node.js app, running "npm shrinkwrap" is a good practice to perform on your branch before releasing changes that you plan to build/deploy as an image with S2I

{{< panel_group >}}
{{% panel "CLI Steps" %}}

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>

```bash
$ oc get builds
```

<blockquote>
Note the name of your build from the above command output and use it to see the logs with:
</blockquote>

```bash
$ oc logs builds/[BUILD_NAME]
```

The console will print out the full log for your build.  Note, you could pipe this to more or less for easier viewing in the CLI.

{{% /panel %}}
{{% panel "Web Console Steps" %}}


<blockquote>
Click on "Builds" 
</blockquote>
<img src="../images/ocp-lab-s2i-builds.png" width="800"><br/>

<blockquote>
Click on the "dc-metro-map" link to view the overview of our build config. A build config contains the definitions for how to build our image. The previous lab deployed an existing image so no build was needed. This time we built a new image from our source code and we define that with a build config.
</blockquote>
<img src="../images/ocp-lab-s2i-buildoverview.png" width="900"><br/>

<blockquote>
Click on the Builds tab and select the most recent, and perhaps only, build attempt: dc-metro-map-1. You create builds from a build config.
</blockquote>
<img src="../images/ocp-lab-s2i-buildsList.png" width="900"><br/>

<blockquote>
Click on the "Logs" link to see the details of your build attempt.
</blockquote>
<img src="../images/ocp-lab-s2i-viewLog.png" width="700"><br/>

{{% /panel %}}
{{< /panel_group >}}

## See the app in action
Let's see this app in action!

{{< panel_group >}}
{{% panel "CLI Steps" %}}

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>

```bash
$ oc get routes
```

<blockquote>
Copy the HOST/PORT and paste into your favorite web browser:
</blockquote>

{{% /panel %}}

{{% panel "Web Console Steps" %}}

<blockquote>
Click Topology and view the application
</blockquote>

<blockquote>
Click dc-metro-map application to see an overview, including the route to applicaiton. You can use either the arrow on the circle or the link in the overview to open the app.
</blockquote>
<img src="../images/ocp-lab-s2i-dcmetromapsvc.png" width="900"><br/>

{{% /panel %}}
{{< /panel_group >}}

The app should look like this in your web browser:

<img src="../images/ocp-lab-s2i-apprunning.png" width="900"><br/>

Clicking the checkboxes will toggle on/off the individual metro stations on each colored line.  A numbered icon indicates there is more than one metro station in that area and they have been consolidated - click the number or zoom in to see more.

# Summary
In this lab we deployed a sample application using source to image.  This process built our code and wrapped that in a docker image.  It then deployed the image into our OpenShift platform in a pod and exposed a route to allow outside web traffic to access our application.  In the next lab we will look at some details of this app's deployment and make some changes to see how OpenShift can help to automate our development processes. More information about creating new applications can be found [here][1].

[1]: https://docs.openshift.com/container-platform/3.4/dev_guide/application_lifecycle/new_app.html

{{< importPartial "footer/footer.html" >}}
