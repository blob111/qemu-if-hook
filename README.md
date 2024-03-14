# qemu-if-hook
Connect VMs via tc mirror

## qemu-ifup-wire
The script will try to pair devices using tc mirror feature.
The variable TOPOLOGY should be set to filename in YAML.
Format of YAML data is:

```
links:
  - wire: [<ep>,<ep>]
  - fork: [<ep>,<ep>,<ep>]
  - bridge: [<ep>, <brname>]
  ...
```

Endpoint `<ep>` is either just device name or tuple `<vmname>:<devname>`.
The `<vmname>` part is not needed by this script and simply removed at input.
Link of type wire is point-to-point link which connects two endpoints.
Link of type fork has root endpoint (first one) and two leaves. Packets egressing the root endpoint appear at ingress of both leaves.
Link of type bridge connects the endpoint to the bridge `<brname>`.

Use the script as hook in libvirt XML:

```
    <interface type='ethernet'>
      <script path='/etc/libvirt/hooks/qemu-ifup-wire'/>
      <target dev='w1a'/>
      <model type='virtio'/>
    </interface>
```

## graph-gen
The script generates input file for graphviz.
It read data in YAML as input stream.
Format of YAML data is the same as above.
The `<vmname>` part considered as node name. Links without `<vmname>` at least in one endpoint will be ignored.
