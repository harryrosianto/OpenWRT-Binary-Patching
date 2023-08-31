# Patching mt76x0-common.ko Module on OpenWRT Devices

This guide explains how to replace and verify a patched `mt76x0-common.ko` kernel module on OpenWRT. The provided steps involve hex editing, copying, and verifying the module for a specific use case.

## Replace patch

Replace occurrences of `122c54f2` with `122c3cf2` in the `mt76x0-common.ko` module and create a patched binary

```
hexdump -ve '1/4 "%.8x\n"' /rom//lib/modules/$(uname -r)/mt76x0-common.ko | \
sed -e 's/^122c54f2/122c3cf2/' | \
sed -re 's/(..)(..)(..)(..)/\\\\x\4\\\\x\3\\\\x\2\\\\x\1/g' | \
xargs -n1 printf > /tmp/mt76x0-common.ko
```

## Copy and Reboot

Copy the patched binary (mt76x0-common.ko) into its appropriate location and reboot.

```
cp /tmp/mt76x0-common.ko /lib/modules/$(uname -r)/mt76x0-common.ko
```

## Verify Size

Verify the size of the patched module and the original module.

```
ls -ld /rom/lib/modules/$(uname -r)/mt76x0-common.ko /tmp/mt76x0-common.ko
```

Sample output:
```
-rw-r--r-- 1 root root 36348 Oct 23 16:16 /rom/lib/modules/5.10.149/mt76x0-common.ko
-rw-r--r-- 1 root root 36348 Feb 22 11:21 /tmp/mt76x0-common.ko
```

## Verify MD5

Verify the MD5 checksum of the patched module and the original module.

```
md5sum /rom/lib/modules/$(uname -r)/mt76x0-common.ko /tmp/mt76x0-common.ko
```

Sample output:
```
e0969068be26377f507baadbd414344e /rom/lib/modules/5.10.149/mt76x0-common.ko
3fc05113be1a1a7e4d405cdbd6197ab1 /tmp/mt76x0-common.ko
```

Please note that the checks are specific to one of the OpenWRT devices. Each user/device might have its own size, kernel version, and MD5 sums for verification.
