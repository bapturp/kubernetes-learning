# Network Namespaces

Network namespaces allow to isolate interfaces.
It's used eavily in container to isolate container from their host.

## Namespaces

```sh
# Create network namespace
ip netns add red

# List namesapces
ip netns


# show interfaces in a specific namespace
ip netns exec red ip link
ip -n red link # same as above
```

# Connect 2 namespaces together

To connect two namespaces together we use a _virtual cable_ (_virtual ethernet pair_) or a _pipe_.


```sh
# Create a virtual cable
ip link add veth-red type veth peer name veth-bleu

# Attach the virtual cable to namespaces
ip link set veth-red netns red
ip link set veth-blue netns blue

# Assign ip to virtual interfaces on each end of the cable
ip -n red addr add 192.168.15.1 dev veth-red
ip -n blue addr add 192.168.15.2 dev veth-blue

# Brink up interfaces
ip -n red link set veth-red up
ip -n blue link set veth-blue up

# ping each interfaces to test
ip netns exec red ping 192.168.15.2

# show arp table
ip netns exec blue arp
```

## Connect multiple namespaces together with Linux Bridge

```sh
# Create a bridge (it's just an interface on the host perspective)
ip link add v-net-0 type bridge

# Bring it up
ip link set dev v-net-0 up

## Connect namespaces to the switch ##
# create a link
ip link add veth-red type veth peer name veth-red-br

# attach link to namespace
ip link set veth-red netns red

# attach link to switch
ip link set veth-red-br master v-net-0
```

## Connect v-switch to the outside world

To connect the v-switch and namespaces to the outside world, first we need to assign an ip to the bridge.
This allow now the host to communicate with namespace through the swtich.
```sh
# Set a address to the bridge
ip addr add 192.168.15.5/24 dev v-net-0
```

To allow namespaces to reach the outside world, we need to create a NAT with iptables.
```sh
# Rule that NAT any packet that come from ip range of our namespaces.
iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE
```

To reach a service inside a namespace we use the port forwarding with iptables
```sh
# Rule that forward any ingress packet on port 80 to the destination inside a namespace
iptables -t nat -A PREROUTING --dport 80 --to-destination 192.168.15.2:80 -j DNAT
```
