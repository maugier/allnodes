# allnodes

A simple script to run ssh commands to multiple machines of a cluster, with colorful output.

It is meant to be compatible with my old `forallnodes` perl script, that did pretty much the same jobs, but with threads instead of processes,
a progress monitor mode, and better argument parsing.


## Usage

    allnodes [-vqmn] [-u USER] [-f HOSTFILE | -c CLUSTER] command [args ...]

Runs a command on every node of the cluster, using ssh. It is the sysadmin's resposibility to ensure passwordless ssh to every node is working.

### Node selection

By default, `allnodes` looks for a file called `default` in either `$HOME/.nodes/` or in `/etc/nodes/`, and expects one hostname per line. 

The perl `forallnodes` script used to support PBS to get the list of nodes. This version doesn't yet.

 * `-c, --cluster`: uses an alternate cluster file, instead of the `default` one. The paths searched as the same as above.

 * `-f, --hostfile`: uses the specified node file. Again, it expects one node per line

 * `-u, --user`: change the username for ssh. Use of this option is discouraged in favor of site-specific configuration in `/etc/ssh/config`.


### Local usage

If the `-n` option is present, the command is run locally instead of over ssh. An argument of "{}" will be substituted by the node name in each invocation.


### Output control

By default, `allnodes` display a summary with the return code of the command on every node, like so:

    $ allnodes true
    node1[  0]  node2[  0]  node3[  0]
    node4[  0]  node5[  0]  node6[  0]

If ssh fails to run, it will return a status of 255 (colored magenta). Otherwise, the return code of the command is colored green if the return code
is 0, or red in other cases.

 * `-q, --quiet`: suppresses the summary output.

 * `-m, --monitor`: instead of waiting for completion of all commands, print a complete summary every second until completion. The children that have not
terminated yet are shown with a return code of `...`.

 * `-v, --verbose`: instead of sending the commands stdin and stderr to /dev/null, forward them to the master. The node name, followed by `: `, will be prefixed to each line.



If the `-v` option


