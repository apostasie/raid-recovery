# RAID recovery

Notes on recovering data on (failing) RAID array

## Basics

Do NOT try to fix things by working off the physical drives.

Clone the drives with dd, then mess up with the clones.

```bash
dd if=/dev/disk5s1 of=diskAs1
dd if=/dev/disk5s2 of=diskAs2
dd if=/dev/disk5s3 of=diskAs3
```

Clones can then be mounted as loopback on linux:

```bash
losetup --read-only /dev/loop0 diskAs1
losetup --read-only /dev/loop1 diskAs2
losetup --read-only /dev/loop2 diskAs3
```

## MDADM

```bash
apt-get install mdadm

mdadm --examine /dev/loop2
```

Note: RAID5 can have one drive fail.

```bash
mdadm --assemble /dev/md/foo /dev/loop2  /dev/loop5 /dev/loop8 /dev/loop11

mkdir mountpoint
mount /dev/md/foo mountpoint
```

## Readings

https://ahelpme.com/linux/recovering-md-array-and-mdadm-cannot-get-array-info-for-dev-md0/

https://serverfault.com/questions/529881/how-do-i-mount-a-raid-disk-in-linux
