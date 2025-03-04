# Node Level Scheduler Quickstart

## Prerequisites

* Kubernetes Cluster with Run:ai installed
* At least one compute cluster added to the Run:ai Control Plane
* While the *Node Level Scheduler* can be enabled when Nodes only have one GPU, it would only make sense to use in the case of multiple GPUs per Node
* **kubectl** command line access to the compute cluster with administrative privileges
* At least one **Node-Pool** that will be used when enabling the *Node Level Scheduler*

## Step 1

Enable the *Node Level Scheduler* at the cluster level by editing the `runaiconfig` file

```yaml
spec: 
  global: 
      core: 
        nodeScheduler:
          enabled: true
```

This can be accomplished by using the below command

```bash
kubectl patch -n runai runaiconfigs.run.ai/runai --type='merge' \
    --patch '{"spec":{"global":{"core":{"nodeScheduler":{"enabled": true}}}}}'
```

## Step 2

Enable **GPU Resource Optimization** in the Run:ai *General Settings*

1. On the left hand side, select **Admin/General Settings**
2. Expand the **Resources** section
3. Slide the toggle next to **GPU resource optimization** to enable the feature. *NOTE: this is currently a Beta feature*

## Step 3

Enable the *Node Level Scheduler* on any of the **Node-Pools** where you want to enable this feature.

1. On the left hand side, select **Resources/Node pools**
2. Select the checkbox next to the **Node-Pool** to be enabled and select the **EDIT** button above in the blue bar.
3. In the bottom section, expand **Resource utilization optimization**
4. Move the slider for **Number of workloads on each GPU** to any value other than **Not enforced**

>*Note: As you move the slider, view the **Heads up** box to the right.  It will explain the value that is being used.*
>
>For example, if the slider is at 3, the **Heads up** box will show
>
>* Each GPU on this node pool will be used by 3 workloads
>* GPU request: Each workload is guaranteed 0.33 GPU