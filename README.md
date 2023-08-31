# Patching mt76x0-common.ko Module on OpenWRT Devices for EPSON-Doog Project

This guide explains how to replace and verify a patched `mt76x0-common.ko` kernel module on OpenWRT. The provided steps involve hex editing, copying, and verifying the module for a specific use case.

## Replace patch

Replace occurrences of `122c54f2` with `122c3cf2` in the `mt76x0-common.ko` module and create a patched binary

```bash
hexdump -ve '1/4 "%.8x\n"' /rom//lib/modules/$(uname -r)/mt76x0-common.ko | \
sed -e 's/^122c54f2/122c3cf2/' | \
sed -re 's/(..)(..)(..)(..)/\\\\x\4\\\\x\3\\\\x\2\\\\x\1/g' | \
xargs -n1 printf > /tmp/mt76x0-common.ko
