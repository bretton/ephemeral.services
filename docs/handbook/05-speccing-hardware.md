# Specification for Hardware

At a minimum, each node should have the bitcoin & LN server files on a redundant array of disks. Losing in-channel funds because of a stale channel state database can be mitigated by ensuring there are always two readable copies of the files.

## Raid 1

Raid 1 writes a copy of every file to two devices. It is slow to write, and fast to read.

The OS runs on a raid1 array

Or the user directory is a raid1 mounted drive

Raid 1 will sustain the failure of one disk in the array, with all the data still present on the remaining disk.

A hot spare, preferably two, will speed up re-establishing Raid 1 redundancy in the event of a disk failure.

## Raid 10

Raid 10 is using two raid 1 arrays, but alternating which array gets a copy of the data. It is fast to write, and fast to read.

The OS runs on a raid10 array. This is rare.

Or the user directory is a raid10 mounted drive. This is more common and raid1/raid10 partition for /home or similar would make sense.

Raid 10 will sustain the failure in one disk in either array, with the data still present on the remaining disks.

2-4 hot spares will speed up re-establishing Raid 10 redundancy in the event of disk failure.

### BTRFS

Using BTRFS we can mount 4 SSDs in a raid10 config, and use this as the /home partition. You will also want a 5th SSD of matching size to the array for read-only snapshots. If you don't have this, recovery from failure is fraught with difficulty. Always have an extra disk for snapsnots and do them regularly.

## VM on a HA cluster

Another option is to run a high availability hypervisor cluster, and run your node as a VM on the cluster.

Depending on vendor solution, live backups of the virtual image, and uninterruptible operation even if one or two servers in the cluster fail, can mitigate concerns of losing channel state files and funds.

## CPUs

More CPUs is better, 4 CPU cores or more should cope with OS, bitcoin, LN server, web/api 

# Testing Redundancy

All the redundant planning in the world is useless without regular, repeat testing of disk failures in a simulation run.

Every X weeks, schedule a disk removal, watch the rebuild from hot spare, and ensure everything works normally.

This ensures regular disk rotation in the array, with hot spares coming into usage and existing disks rotating out into hot spares.

Regularly rebuilding the array as a normal part of operations ensures the redundancy is continually tested.

## Sample procedure

1. Schedule tests for the year ahead, such as the first friday of every month.
2. Announce tests via calendar, a day before, an hour before, and at the time of the test.
3. Pull out a hard drive from the active array
4. Watch the automatic hotspare attach and array rebuild (perform manually if necessary).
5. Run tests to ensure services are live, data is valid.
6. Announce results. If successful, end of procedure.
7. In the event of failures, identify the cause and solutions.
8. Announce results. If successful, end of procedure.
