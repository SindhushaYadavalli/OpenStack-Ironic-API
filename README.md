Step-1:

clone the repo using command:
git clone https://github.com/openstack-dev/devstack.git devstack

Step-2:

create local.conf in devstack/local.conf and copy the contents of the file "local.conf" from the repository.

Step-3:

Now your devstack is ready for setup, simply run
./stack.sh

Step-4:

After devstack finishes running, you should see something similar to:
Horizon is now available at http://10.0.0.1/
Keystone is serving at http://10.0.0.1:5000/v2.0/
Examples on using novaclient command line is in exercise.sh
The default users are: admin and demo
The password: secrete
This is your host ip: 10.0.0.1

Step-5:

Created fake-nodes can be observed using command:
ironic node-list

Step-6:

nova boot --nic net-id”=<net-id> --flavor baremetal –image <image-id> --key_name test2
my-first

Step-7:

Now ironic nodes will go through the following stages:
Deploying -> wait-callback ->active.

Step-8:

And now we can see the instance is Running with command:

nova list

Step-9:

ssh to created instance from external network using “Floating IP”
(i)check available floating-ip using command:
nova flating-ip-list
(ii)create floating-ip:
nova floating-ip-create
(iii)Assign created floating-ip to instance
nova add floating-ip my-first 172.24.4.3

Now with that floating-ip can ping from external network
and Can ssh to that system using:
ssh -i test.pem cirros@172.24.4.3

Step-10:

	If in case want to create ironic node on other system, we need to create ironic node by ssh to
required system. Which involves following steps:

1.Chassis Create
ironic chassis-create

2.node-create

ironic node-create --chassis_uuid 98834cf1-6ed3-4fd2-98c8-f1c98184bd54 --driver pxe_ssh -i
pxe_deploy_kernel=1a229813-bb82-4a0c-8057-47f7ed559a4f -i pxe_deploy_ramdisk=c866ad86-
9846-41b5-9766-4089788b5950 -i ssh_virt_type=virsh -i ssh_address=10.1.98.197 -i ssh_port=22
-i ssh_username="sindhusha" -p cpus=1 -p memory_mb=1024 -p local_gb=20 -p
cpu_arch=x86_64

3.port-create

ironic port-create --address 56:84:7a:fe:97:99 --node_uuid 0bdabd94-7667-4b19-8de4-
27fe5cf0d516

4.Instance info updation

ironic node-update 72ba92fb-1afa-496e-ae95-15ba3f3a3c8f add
instance_info/image_source=54514ec1-a95a-4f12-ab7b-d6d9abf15f33
ironic node-update 72ba92fb-1afa-496e-ae95-15ba3f3a3c8f add instance_info/root_gb=1

