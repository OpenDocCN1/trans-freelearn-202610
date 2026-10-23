```
4092=8765=4a9f76793842 -smbios\ntype=1,manufacturer=Fedora\nProject,product=OpenStack Nova,version=13.1.0-\n1.el7,serial=8184f926-0263-4305-8d8d-\nd50c1472697e,uuid=b803b3ce-1570-4092-8765-\n4a9f76793842,family=Virtual Machine -no-user-\nconfig -nodefaults -chardev\nsocket,id=charmonitor,path=/var/lib/libvirt/\nqemu/domain-instance-00000001/monitor.sock,\nserver,nowait -mon\nchardev=charmonitor,id=monitor,mode=control -rtc\nbase=utc -no-shutdown -boot strict=on -device\npiix3-usb-uhci,id=usb,bus=pci.0,addr=0x1,0x2 -\ndrive file=/var/lib/nova/instances/b803b3ce-1570-\n4092-8765-4a9f76793842/disk,if=none,id=drive-\nvirtio-disk0,format=qcow2,cache=none -device\nvirtio-blk-pci,scsi=off,bus=pci.0,addr=0x4,\ndrive=drive-virtio-disk0,id=virtio-disk0,\nbootindex=1 -netdev tap,fd=26,id=hostnet0 -device\nvirtio-net-pci,netdev=hostnet0,id=net0,\nmac=fa:16:3e:43:48:ad,bus=pci.0,addr=0x3 -chardev\nfile,id=charserial0,path=/var/lib/\nnova/instances/b803b3ce-1570-4092-8765-\n4a9f76793842/console.log -device isa-\nserial,chardev=charserial0,id=serial0 -chardev\npty,id=charserial1 -device isa-\nserial,chardev=charserial1,\nid=serial1 -device usb-tablet,id=input0 -vnc\n0.0.0.0:0 -k en-us -vga cirrus -device\nvirtio-balloon-pci,id=balloon0,bus=pci.0,addr=0x5\n-msg timestamp=on
```

As you can see, the first line will give the context of qemu-kvm (instance process). The `instance-00000001` instance has an MCS context of `system_u:system_r:svirt_tcg_t:s0:c55,c122`.

+   5. Now, looking at the files associated with this instance in `/var/lib/nova/instances/b803b3ce-1570-4092-8765-4a9f76793842/` confirms that sVirt has properly labeled them same as the instance processes (`s0:c55,c122`):

```
# ls -alZ /var/lib/nova/instances/b803b3ce-1570-\n4092-8765-4a9f76793842/\n\ndrwxr-xr-x. nova nova\nsystem_u:object_r:nova_var_lib_t:s0 .
```