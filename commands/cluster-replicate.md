The command reconfigures a node as a slave of the specified master.
If the node receiving the command is an *empty master*, as a side effect
of the command, the node role is changed from master to slave.

Once a node is turned into the slave of another master node, there is no need
to inform the other cluster nodes about the change: heartbeat packets exchanged
between nodes will propagate the new configuration automatically.

A slave will always accept the command, assuming that:

1. The specified node ID exists in its nodes table.
2. The specified node ID does not identify the instance we are sending the command to.
3. The specified node ID is a master.

If the node receiving the command is not already a slave, but is a master,
the command will only succeed, and the node will be converted into a slave,
only if the following additional conditions are met:

1. The node is not serving any hash slots.
2. The node is empty, no keys are stored at all in the key space.

If the command succeeds the new slave will immediately try to contact its master in order to replicate from it.

@return

@simple-string-reply: `OK` if the command was executed successfully, otherwise an error is returned.
