
Getting ScyllaDB up and running on your local laptop is quick and simple, and it can be achieved in relatively 3 simple steps.

1. Let's pull the ScyllaDB container image from [https://hub.docker.com/r/scylladb/scylla](Docker Hub):

```shell
docker pull scylladb/scylla:5.2.2
```{{exec}}

**Note:** You may specify a different version as you see fit. Check [https://hub.docker.com/r/scylladb/scylla](Docker Hub) as needed!

2. After retrieving the container image, we may now start a ScyllaDB container from it:
```shell
docker run -dit --name scylla-labs scylladb/scylla:5.2.2 --smp 1 --memory 1G
```

Let's take a look at each relevant parameter and understand what it does:

- We specify the **container name**, which we will be using later on with `--name scylla-labs`
- Then, we specify the ScyllaDB image to use, as we did in Step 1. This is `scylladb/scylla:5.2.2` 
- Finally, we specify that we want ScyllaDB to restrict itself to use only **1 vCPUs** from the source machine (*--smp 1*) and only a total of 1G of memory (*--memory 1G*). You may omit or adjust these parameters as you see fit.

After our container starts, you may *optionally* check its logs to see what it is doing in the background. This can be accomplished by running the following Docker command: 
```shell
docker logs -f scylla-labs
```{{exec}}

You may press `^C` in order to leave the logs. If you need assistance, simply execute the following code:

```plain
^C
```{{exec}}

3. Our third and final step is to effectively get a shell on the ScyllaDB container and run some commands. To do so, execute:

```shell
docker exec -it scylla-labs bash
```{{exec}}

At this step, we may now run commands such as **nodetool status** to confirm that we indeed have a single-node cluster and that it reports its status as **UN** (Up & Normal), like in the following example output:

> # nodetool status
> Datacenter: datacenter1
> =======================
> Status=Up/Down
> |/ State=Normal/Leaving/Joining/Moving
> --  Address     Load       Tokens       Owns    Host ID                               Rack
> UN  172.17.0.2  216.47 MB  256          ?       f373e2ea-a799-44f5-8e49-687281f02104  rack1

If your node is not under this state yet, or if **nodetool status** is returning you an error, you might want to check the container logs as shown before, or wait for longer as the node may be still bootstrapping.

We may then run the **cqlsh** command in order to interact with the database and send commands to it. Shall we?

```shell
cqlsh
```{{exec}}

But hold on right there! Before you run anything, let's understand a few concepts beforehand!
