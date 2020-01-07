# Multiple Mech Example
Experimentation with multiple instances using [Mech](https://github.com/mechboxes/mech) which allows you to do command line interactions with local VMware instances.

Note: Current `mech` version does not support multiple instances in the same Mechfile, but if you put
a Mechfile in a separate directory, you can spin up multiple VMs quickly and in *parallel*.

# To get started:

    virtualenv -p python3 venv
    source venv/bin/activate
    pip install mech

Run any of these commands:

    ./up
    ./down
    ./destroy

# Common commands:

    mech global-status
    mech ls


# How to add a new Mechfile
Created the `Mechfile` in each instances directory by running a `mech init` command like this:

    mech init bento/ubuntu-18.04 --name ubuntu18

# Comparisons:

Comparing `vagrant up` vs. `mech up` using the `alpine311` instance:

    $ cd /tmp
    $ vagrant init mrlesmithjr/alpine311
    $ time vagrant up
    Bringing machine 'default' up with 'vmware_desktop' provider...
    ==> default: Cloning VMware VM: 'mrlesmithjr/alpine311'. This can take some time...
    ==> default: Checking if box 'mrlesmithjr/alpine311' version '1578437753' is up to date...
    ==> default: Verifying vmnet devices are healthy...
    ==> default: Preparing network adapters...
    ==> default: Starting the VMware VM...
    ==> default: Waiting for the VM to receive an address...
    ==> default: Forwarding ports...
    default: -- 22 => 2222
    ==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
    ==> default: Machine booted and ready!
    ==> default: Configuring network adapters within the VM...

    real	0m35.189s
    user	0m4.648s
    sys	0m0.802s


    # starting in this repo's main directory
    $ cd instances/alpine311
    $ time mech up
    Loading metadata for box 'mrlesmithjr/alpine311' (1578437753)
    Extracting box 'mrlesmithjr/alpine311'...
    Added network interface to vmx file
    Bringing machine up...
    Getting IP address...
    Sharing current folder...
    VM started on 192.168.2.163

    real	0m16.944s
    user	0m1.639s
    sys	0m0.312s

So, it takes half the time to spin up using `mech` vs. `vagrant`.


Note: The reason for the bash in the scripts like `up` is because I wanted to `time` how long it would take to spin up the instances in parallel, like this:

Comparison on spinning up mulitple instances:

    time ./up
    <snip>
    Loading metadata for box 'bento/centos-7' (201912.05.0)
    Loading metadata for box 'bento/centos-8' (201912.04.0)
    Loading metadata for box 'bento/ubuntu-18.04' (201912.04.0)
    Extracting box 'bento/centos-8'...
    Extracting box 'bento/centos-7'...
    Extracting box 'bento/ubuntu-18.04'...
    Added network interface to vmx file
    Bringing machine up...
    Added network interface to vmx file
    Bringing machine up...
    Added network interface to vmx file
    Bringing machine up...
    Getting IP address...
    Getting IP address...
    Getting IP address...
    Sharing current folder...
    VM started on 192.168.2.216
    Sharing current folder...
    VM started on 192.168.2.206
    Sharing current folder...
    VM started on 192.168.2.207

    real	0m57.409s
    user	0m11.863s
    sys	0m2.375s

Which is faster than not sending the `mech up` commands to the background:

    real	2m29.214s
    user	0m11.259s
    sys	0m2.116s

For a comparison with `vagrant` using the VMware Desktop plugin and using the `Vagrantfile` in this directory and running `time vagrant up`:

    real	1m55.257s
    user	0m15.807s
    sys	0m3.205s

