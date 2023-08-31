# EPSON-Network-Binary-Patching

## Replace patch with
```
hexdump -ve '1/4 "%.8x\n"' /rom//lib/modules/$(uname -r)/mt76x0-common.ko|sed -e 's/^122c54f2/122c3cf2/' |sed -re 's/(..)(..)(..)(..)/\\\\x\4\\\\x\3\\\\x\2\\\\x\1/g'|  xargs -n1 printf >  /tmp/mt76x0-common.ko
```
`no output`
