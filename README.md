Disk Utility Tools
=========

This repo contains some scripts for disk utility in large software/hardware RAID arrays.

disk_locate
---------
Locating a disk in a large array is always a problem.

This script is written for a ZFS software RAID system. When a disk dies in ZFS pool ZFS may not automatically indicate the location of the faulted disk. The script takes the GPTID or the serial number of the disk and tries to turn on its indication led. The script uses sas2ircu or sas3ircu to turn on the indication light of the drive. So, I can say that it is LSI dependent.

Even if you have an HBA card in IT mode, if you installed its driver you can install sas2ircu and sas3ircu, and use some of its functionality like 'LOCATE' in our case.supports more than one HBA/RAID card. In order to use it, you need to add the index numbers of the cards in the script. By default it only searches for disks on the card with index 0.

NOTE: disk_locate script depends on disk_serial script.(will change)

disk_serial
---------
This scipt retreives the serial number of disks from their GPTID. It takes the GPTID of the drive, and uses glabel, smartctl (smartmontools) to retreive the serial number of the device. The script takes GPTID because our ZFS system identifies the drives with GPTID. The output of example `zpool status` is given below.

```
# zpool status tank
  pool: tank
 state: ONLINE
  scan: scrub repaired 0 in 1h32m with 0 errors on Thu Dec 25 00:26:15 2014
config:

    NAME                                            STATE     READ WRITE CKSUM
    tank                                            ONLINE       0     0     0
      raidz2-0                                      ONLINE       0     0     0
        gptid/bc3b91fd-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/bcb8079a-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/bd5c02dd-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/ba13d017-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/becc7f6b-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/bf8fc2fd-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
      raidz2-1                                      ONLINE       0     0     0
        gptid/c6640656-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/c318f79e-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/c2cece13-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/c188d4f8-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0
        gptid/2ec2f587-817e-11e4-ae92-0035804f83f0  ONLINE       0     0     0
        gptid/c3f08956-7663-11e4-a311-0035804f83f0  ONLINE       0     0     0

errors: No known data errors
```

Examples
-----------

```
# ./disk_serial gptid/ba13d017-7663-11e4-a311-0025904f83f0
Device Path:    /dev/da15
Serial Number:  W1F2873P
```

```
# ./disk_locate gptid/ba13d017-7663-11e4-a311-0025904f83f0
Serial Number:      W1F2873P
HBA Card Index:     0
Enclosure Number:   2
Slot Number:        15

Running Command: sas3ircu 0 LOCATE 2:15 OFF
Done
```

