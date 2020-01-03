Experimentation with multiple instances using [Mech](https://github.com/mechboxes/mech) which allows you to do command line interactions with local VMware instances.

Note: Current `mech` version does not support multiple instances in the same Mechfile, but if you put
a Mechfile in a separate directory, you can spin up multiple VMs quickly and in *parallel*.

To get started:

    virtualenv -p python3 venv
    source venv/bin/activate
    pip install mech

Run any of these commands:

    ./up
    ./down
    ./destroy

Common commands:

    mech global-status
    mech ls
    
    
Note: The reason for the bash in the scripts like `up` is because I wanted to `time` how long it would take to spin up the instances in parallel, like this:

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


Created the `Mechfile` in each instances directory by running a `mech init` command like this:

    mech init bento/ubuntu-18.04

