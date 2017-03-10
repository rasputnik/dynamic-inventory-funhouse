h2. playground for dynamic inventories.

Not very useful, just exists for me to get my head
around dynamic inventories (and how they work with
local group_vars/ and host_vars flat files).

I'm using the serf inventory _(slightly tweaked)_
because it has the fewest moving parts.

there are 2 inventories and 2 playbooks.

h3. first, build some vms

    ansible-playbook -i vagrant/ bootstrap.yml

will build 3 vms with a serf agent on each, and
tag them with the contents of 'vagrant/group_vars/'.

The most important tag is the 'group=foo' one, since that
will determine which group the vm finds itself in when we 
cut over to a Serf dynamic inventory.

h3. next, start your own serf agent

On your control host, run up a serf agent and connect
it to the cluster. The hardest part is finding the right
interface. On my Mac its :

    serf agent -discover=mycluster -join web1 -iface vboxnet27

h3. cut over to the dynamic inventory.


    ansible-playbook -i serf site.yml


Try switching tags on the hosts to see the hostvars switch
in realtime.

TIP: 'serf members' will dump all active members and their vars.
