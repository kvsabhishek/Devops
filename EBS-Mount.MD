* When we attach EBS volume to EC2, we cannot use it directly. We need to create filesystem for that attached EBS volume.

* To list all block storages. Attached EBS will list here.
```
lsblk
```

* Get information about all of the devices attached to the instance.
```
sudo lsblk -f
```

* To check if there is already file system created for this volume or not.(/dev/xvdf volume name we attached)
```
sudo file -s /dev/xvdf
```
If this command shows output like this that means no filesytem exists /dev/xvdf: data

* To create file system
```
sudo mkfs -t xfs /dev/xvdf
```

* To mount the volume to /data directory
```
sudo mount /dev/xvdf /data
```

* If we have partitions to that volume then use this to mount specific partition 
```
sudo mount /dev/xvdf1 /data
```