# Data Migration - Swap Hard Drives

## Take down all Kubernetes deployments

```bash
for i in $( kubectl get deployments | awk '{print $1}' | grep -v gluster | grep -v NAME ) ; do kubectl scale --replicas=0 deployment/$i ; done
```

## Stop lsyncd

```bash
sudo service lsyncd stop
```

## Do one last backup

```bash
sudo zfs snapshot -r storage/container_data@latest
zfs list -t snapshot
sudo -i
zfs send storage/container_data@latest | pv | gzip > "/archive/container_data.gz"
```

## Destroy pool

```bash
zpool destroy -f storage
```

## Swap drives

## Get out list of disks

```bash
lsblk | grep disk
```

## Create new pool

```bash
zpool create storage raidz2 sdb sdc sdd sde sdf sdg
```

## Reimport the array to get the disks imported by id

```bash
sudo zpool export storage
sudo zpool import -d /dev/disk/by-id -aN
zpool status
```

## Copy archive.gz file

## Restore dataset

```bash
zpool create storage/${user}
gunzip -c archive.gz | pv | sudo zfs recv storage/container_data
```

## Rsync

```bash
sh -c "rsync -av --progress -e 'ssh -p 22' /mnt/r/backup/* username@host:/storage/backup ; \
rsync -av --progress -e 'ssh -p 22' /mnt/r/personal/* username@host:/storage/personal" |& tee -a rsync.log
```

## Redeploy everything

```bash
find . -maxdepth 1 -type d -name '[^.]*' -exec kubectl apply -f "{}" \;
```
