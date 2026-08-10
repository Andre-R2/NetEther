# OSPF Router ID

The Router ID (RID) is a unique 32-bit identifier used by OSPF to distinguish one router from another. It is used when establishing OSPF neighbor relationships, participating in DR/BDR elections, and identifying the origin of link-state information.

The Router ID identifies the router within the OSPF process. A router can have multiple OSPF neighbors through different interfaces, but all of those neighbors identify the same router using the same Router ID.

Although the Router ID is normally represented in IPv4 address format for convenience, it is an identifier rather than a routable IP address. It does not need to belong to an interface or be used as a source or destination address for regular network traffic.

## Router ID Selection

OSPF selects the Router ID when the OSPF process starts. On Cisco IOS, the selection follows this order of priority:

- Manually configured Router ID

    A Router ID configured with the `router-id` command has the highest priority.

    ```text
    router ospf 1
     router-id 1.1.1.1
    ```

- If no Router ID is configured manually, OSPF selects the highest IP address among the configured loopback interfaces.

- If no loopback interfaces are available, OSPF selects the highest IP address from an active physical interface.

The Router ID is therefore determined according to the available configuration when the OSPF process initializes.

## Router ID Changes

Once OSPF has selected a Router ID, adding a loopback interface with a higher IP address or changing an interface address does not automatically cause OSPF to select a new Router ID.

The OSPF process must be restarted for the Router ID selection process to occur again.

On Cisco IOS, this can be done with:

```text
clear ip ospf process
```

Restarting the OSPF process causes existing OSPF neighbor relationships to be reset and the Router ID to be selected again according to the configured priority.

## Router ID Scope

The Router ID is local to the OSPF process. It is the identifier OSPF uses to represent the router within the OSPF routing domain.

The same Router ID is used regardless of which interface or neighbor is involved. This allows OSPF to distinguish the router as a single entity even when it has multiple interfaces and multiple neighbor relationships.