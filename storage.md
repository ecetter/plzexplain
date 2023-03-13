
# File Storage

Both Roar and Roar Collab offer several file storage options for users, each with their own quotas and data retention policies. The multiple options are available for users to optimize their workflows.


## Storage Information

| Storage | Location | Space Quota | Files Quota | Backup Policy |
| ---- | ---- | ---- | ---- | ---- |
| Home | /storage/home | 16 GB | 500,000 files | Daily snapshot |
| Work | /storage/work | 128 GB | 1 million files | Daily snapshot |
| Scratch | /gpfs/scratch (Roar)<br>/scratch (RC) | None | 1 million files | No Backup, Files purged after 30 days |
| Group | /gpfs/group (Roar)<br>/storage/group (RC) | Allocation-dependent | Allocation-dependent | Daily snapshot |


## Check Usage

To check storage usage against the storage quotas, run the following command on either Roar or Roar Collab:
```
$ check_aci_storage_quota
```

On Roar Collab, the following command works as well:
```
$ check_storage_quotas
```

