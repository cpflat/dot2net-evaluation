# Preliminary Tutorial for dot2net evaluation

# Environmental Setup

This experiment requires docker environment and root privilege on your computer.
In addition, we use dot2net and containerlab, so install them at first.

[Containerlab](https://containerlab.dev/)

[dot2net](https://github.com/cpflat/dot2net/)


## What are the files in this directory?

There are two main files `ospf.dot` and `ospf.yaml` in this directory.
These files are the input for dot2net.
DOT is a graph description language used in [Graphviz](https://graphviz.org/).
If you have installed graphviz, you can generate a visualized graph from `ospf.dot` with the following command. 

    dot -Tpdf ospf.dot > ospf.pdf

Comparing the `ospf.dot` and the generated visualized graph,
you can see lines 2-4 defines the nodes and lines 6-7 defines the links between the nodes.
The detailed DOT grammar is explained in the [Graphviz documentation](https://graphviz.org/doc/info/lang.html).

You will also see that there is an attribute `class=router` for each node.
It means the node belongs to an ObjectClass `router`.
The ObjectClass is defined in `ospf.yaml`.

`ospf.yaml` defines what kind of configuration file to generate based on the given DOT file.
It includes files definitions to generate, IP address assignment policy, and ObjectClass definitions (with container image and config template blocks).
The files are generated for each nodes respectively, and binded to the container at the given path.
In this case, `daemons` and `vtysh.conf` will be placed as is in this directory, and `frr.conf` will be generated with the config templates.
The embedded parameters are automatically assigned based on the policied given in the `ospf.yaml`.

There are two kinds of ObjectClass definitions: `nodeclass` and `interfaceclass`.
They are corresponding to the nodes and links in the DOT file.
If the nodes and links have no attributes specifying the classes (e.g., links in `ospf.dot`),
the objects will belong to `default` class if defined
(i.e., the links in `ospf.dot` belong to `default` interface class).

Further usage is explained in the [readme.md](https://github.com/cpflat/dot2net/) of dot2net.


## How to use dot2net and containerlab?

Let's generate containerlab topology file from these files.

    dot2net clab -c ./ospf.yaml ./ospf.dot > topo.yaml

This command will generate three directories and a YAML file `topo.yaml`, a containerlab topology file.
(If your dot2net executable is not registered in your PATH, you need to replace `dot2net` to the executable path. e.g., `/path/to/dot2net`)
Each directory contains three files defined in `ospf.yaml`.

The topology file specifies nodes (with container image and file bind mount configurations) and link placement.
The topology file definition is explained at the [Containerlab documentation](https://containerlab.dev/manual/topo-def-file/).

The try deploying the container network with following command.

    sudo containerlab deploy --topo topo.yaml

You can check the containers successfully deployed with `docker ps` command.
You can also attach the login shell of the containers for example with `docker exec -it r1 /bin/sh`.
Please check that the node `r1` is accessible with the node `r3` using ping command.

After testing the network, you can remove the network with following command.

    sudo containerlab destroy --topo topo.yaml


## Change network topology with Containerlab

Traditionally, you can change the network topology by modifying the Containerlab topology file.
Try extending one node `r4` and one link `r3:net1-r4:net0`.
You need to modify topo.yaml and generate directory `r4` including the internal files.
You also need to modify parameters in `r4/frr.conf` considering the IP address assignment.
In addition, do not fotget to add new neighbor network configuration on `r3/frr.conf`

After that, try deploying the extended topology with containerlab and test it with ping.
If it fails, good luck finding out the reason and fix the configuration files.


## Change network topology with dot2net

I believe you can change the network topology easier with dot2net.
You need to modify the `ospf.dot` to add node `r4` (with label `router`) and one link `r3->r4` (or `r4->r3` is also ok).
Note that you do not need to modify `ospf.yaml` in this case.

With the change, try generating `topo.yaml` again.
The generated configuration file will have automatically calculated parameters.

Try deploying the extended topology again.
There should be no failures, but try troubleshooting it if fails.


## What the authors requests you to do in the evaluation?

We will ask you to generate appropriate configuration files for four network topologies with specified platform (Containerlab or dot2net).
You will be randomly assigned to group A or group B.
Use the corresponding directory in the repository for your evaluation.

During the evaluation, please record the working time (minutes) and number of deploy trials (one if no failures) for each topology.
They will be used to compare the ease of changing network topology in dot2net and traditional method (containerlab).

