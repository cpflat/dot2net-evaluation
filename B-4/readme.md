# Group B - Assignment 4

## Assignment

Modify network topology by changing [configuration files](./)
including Containerlab input (topo.yaml) and per-device application configuration files
to achieve the target network topology.
Deploy the modified network configuration with Containerlab and then check that the test items works correctly.


|現在のトポロジ  |目標のトポロジ |
|----------------|---------------|
|![](./start.png)|![](./goal.png)|

Note that the additional router r6 is a BGP router and communicates with r2 on eBGP.



## Test items

- Test reachability of r4 to r6 with ping command
- Test reachability of r6 to r4 with ping command

