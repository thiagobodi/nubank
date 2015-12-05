# nubank
Nubank Network Test

This is a test network for a challenge proposed by NUBANK.
I used to solve this challenge 3 solutions:
- Vagrant (mandatory for the challenge);
- VirtualBox;
- CentOS 6.5.

I put all tests directly inside each virtual machine (using a strategy suggested by Vagrant). This way, to run the tests you can connect at each node and run test.sh.

Each virtual machine is called by a letter and "node" (ex. enode).
Following a simple diagram of network

         guest ->    Enode (192.168.10.2) (internet gateway) GUEST
        /
Anode --|   
        |
        |
        |
        |             Bnode (192.168.11.102)
        \ intranet ->    Cnode (192.168.11.2) (internet gateway) INTRANET
                      Dnode (192.168.11.103)


All virtual machines starts with 2 interfaces: 1 nat and 1 hostonly. To facilitate the work, I used a routing/filter (iptables) solution.
For the first test (Anode in GUEST network), use the Vagrantfile as downloaded. Run Vagrantfile and execute in each node: /tmp/test.sh (the results will appear in /tmp/test.out)

For the second test  (Anode in INTRANET network), comment the TEST 1 block (in Vagrantfile) and uncomment TEST 2 block, rerun Vagrant and run test.sh again (the results will appear in /tmp/test.out)

Unfortunately, the Vagrant need more configurations to work with both networks and that's a working in progress in community (https://github.com/mitchellh/vagrant/issues/5524).
