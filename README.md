# qemu-if-hook
Connect VMs via tc mirred

# qemu-ifup-wire
The script will try to pair devices using tc mirror feature.
The variable TOPOLOGY should be set to filename in YAML.
The format of YAML data is:

links:
  - wire: [<ep>,<ep>]
  - fork: [<ep>,<ep>,<ep>]
  ...
  
Endpoint <ep> is either just device name or tuple <vmname>:<devname>.
The <vmname> part is not needed by this script and simply removed at input.
It can be used as informational.

Use the script as hook in libvirt XML:

    <interface type='ethernet'>
      <script path='/etc/libvirt/hooks/qemu-ifup-wire'/>
      <target dev='w1a'/>
      <model type='virtio'/>
    </interface>

# graph-gen
The script generates input file for graphviz.
The script read data in YAML as input stream.
The format of YAML data is the same as above.
The <vmname> part considered as node name. Links without <vmname> at least in one <ep> will be ignored.
