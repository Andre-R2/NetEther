# Port Security

Port Security is a Layer 2 security feature that controls which devices can access a switch port by restricting the MAC addresses allowed on that interface.

On a normal access port, when an endpoint is connected, the switch automatically learns its MAC address and associates it with that port. If the device is later disconnected and replaced with another one, the switch simply learns the new MAC address and updates its MAC address table accordingly.

By default, switches do not verify whether the connected device is actually authorized. Any endpoint can be plugged into an access port, and even another switch could be connected, potentially participating in STP, generating broadcasts, or introducing other Layer 2 issues.

Port Security addresses this problem by allowing administrators to define which devices are permitted to use a particular port.

Besides preventing unauthorized devices from accessing the network, Port Security also helps mitigate attacks such as MAC spoofing and CAM table overflow.

## Authorized MAC addresses

Port Security allows administrators to specify which MAC addresses are permitted on a switch port.

For example, suppose port GigabitEthernet0/1 is configured to allow only the MAC address `AAAA.AAAA.AAAA`.

If another device with MAC address `BBBB.BBBB.BBBB` is connected to the same port, the switch detects that the MAC address is unauthorized and applies the configured violation action.

It is also possible to allow multiple MAC addresses on the same interface by defining the maximum number of secure addresses the port can learn.

## Learning secure MAC addresses

There are three ways a switch can learn secure MAC addresses.

### Dynamic

The switch automatically learns the first MAC addresses connected to the port until the configured maximum is reached.

These entries exist only in the running configuration and are lost if the switch is rebooted.

### Sticky

Sticky learning also allows the switch to learn MAC addresses automatically, but unlike dynamic learning, the learned addresses are written into the running configuration.

If the configuration is saved, the secure MAC addresses remain after a reboot.

### Static

The administrator manually configures the allowed MAC addresses.

Only those addresses are permitted to communicate through the port.

## Violation modes

When an unauthorized MAC address appears on a secured port, the switch can perform different actions depending on the configured violation mode.

### Protect

Frames from unauthorized MAC addresses are silently discarded.

The port remains operational, and no notifications or logs are generated.

### Restrict

Frames from unauthorized MAC addresses are dropped.

Unlike Protect mode, the switch increments the violation counter and generates log messages or SNMP traps while keeping the port active.

### Shutdown

This is the default and most secure violation mode.

As soon as an unauthorized MAC address is detected, the switch places the interface into the err-disabled state, effectively shutting down the port until it is manually or automatically recovered.