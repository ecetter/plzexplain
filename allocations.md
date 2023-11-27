
# Allocations

## Open Queue

All Roar Collab users have access to the **open** queue, which allows users to submit jobs free of charge. The **open** queue has a time request limit of 48 hours and also has several other resource limits per user. The per-user resource limits for the **open** queue can be displayed with the following command:

```
$ sacctmgr show qos open format=name%10,maxtrespu%40
      Name                                MaxTRESPU 
---------- ---------------------------------------- 
      open      cpu=100,gres/gpu=0,mem=800G,node=20 
```

Jobs on the **open** queue will start and run only when sufficient idle compute resources are available. For this reason, there is no guarantee on when an **open** queue job will start. All users have equal priority on the **open** queue, but **open** queue jobs have a lower priority than jobs submitted to a paid compute allocation. If compute resources are required for higher priority jobs, then an **open** queue job may be cancelled so that the higher priority job can be placed. Roar Collab has somewhat low utilization, however, so the vast majority of **open** queue jobs can be submitted and placed in a resonable amount of time. The **open** queue is entirely adequate for most individual users and for many use cases.

[//]: # (add info on preemption, --requeue option, saving state)


## Compute Allocations

A paid compute allocation provides access to specific compute resources for an individual user or for a group of users. A paid compute allocation provides the following benefits:

- Guaranteed job start time within one hour
- No time request limit
- Burst capability up to 4x of the allocation's compute resources
- 5 TB of active group storage

A compute allocation results in the creation of a compute account on Roar Collab. The `mybalance` command on Roar Collab lists accessible compute accounts and resource information associated with those compute accounts. Use the `mybalance -h` option for additional command usage information.


### Compute Resources Available

A paid compute allocation will typically cover a certain number of cores across a certain timeframe. The resources associated with a compute allocation are in units of **core-hours**. The compute allocation has an associated **Limit** in **core-hours** based on the initial compute allocation agreement. Any amount of compute resources used on the compute allocation results in an accrual of the compute allocation's **Usage**, again in **core-hours**. The compute allocation's **Balance** is simply the **Limit** minus its **Usage**.

```
Balance [core-hours] = Limit [core-hours] - Usage [core-hours]
```

At the start of the compute allocation, 60 days-worth of compute resources are added to the compute allocation's **Limit**. Each day thereafter, 1 day-worth of compute resources are added to the **Limit**.

```
Initial Resources   [core-hours] = # cores * 24 hours/day * 60 days
Daily Replenishment [core-hours] = # cores * 24 hours/day
```

The daily replenishment scheme continues on schedule for the life of the compute allocation. Near the very end of the compute allocation, the replenishment schedule may be impacted by the enforced limit on the maximum allowable **Balance**. The **Balance** for a compute allocation cannot exceed the amount of compute resources for a window of 91 days and cannot exceed the amount usable by a 4x burst for the remaining life of the compute allocation. This limit is only relevant for the replenishment schedule nearing the very end of the compute allocation life.

```
Max Allowable Balance [core-hours] = min( WindowMaxBalance, 4xBurstMaxBalance )

where

WindowMaxBalance  [core-hours] = # cores * 24 hours/day * 91 days
4xBurstMaxBalance [core-hours] = # cores * 24 hours/day * # days remaining * 4 burst factor
```

### Compute Allocation Management

The principal contact for a compute allocation is automatically designated as a coordinator for the compute account associated with the compute allocation. A coordinator can add another coordinator with the following command:
```
$ sacctmgr add coordinator account=<compute-account> name=<userid>
```

A coordinator can then add and remove users from the compute account using the following:
```
$ sacctmgr add user account=<compute-account> name=<userid>
$ sacctmgr remove user account=<compute-account> name=<userid>
```


## Storage Allocations

A paid storage allocation provides access to a shareable group location for active file storage. Active group storage is included with a paid compute allocation, but additional storage space can be purchased as well. Active storage is mounted on all compute resources and enables users to read, write, and modify files stored there. For long-term storage of infrequently-used files that is separate from compute resources, archive storage is available for purchase and is accessible via the [Globus](https://www.globus.org/) interface.

[//]: # (add info on umgs)
