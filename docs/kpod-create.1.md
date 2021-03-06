% kpod(1) kpod-create - Create a new container
% Dan Walsh
kpod-create - Create a new container

# SYNOPSIS
**kpod create** [*options* [...]] IMAGE [COMMAND] [ARG...]

# DESCRIPTION

Creates a writeable container layer over the specified image and prepares it for
running the specified command. The container ID is then printed to STDOUT. This
is similar to **kpod run -d** except the container is never started. You can
then use the **kpod start <container_id>** command to start the container at
any point.

The initial status of the container created with **kpod create** is 'created'.

# OPTIONS
**--add-host**=[]
   Add a custom host-to-IP mapping (host:ip)

   Add a line to /etc/hosts. The format is hostname:ip.  The **--add-host**
option can be set multiple times.

**-a**, **--attach**=[]
   Attach to STDIN, STDOUT or STDERR.

   In foreground mode (the default when **-d**
is not specified), **kpod run** can start the process in the container
and attach the console to the process's standard input, output, and standard
error. It can even pretend to be a TTY (this is what most commandline
executables expect) and pass along signals. The **-a** option can be set for
each of stdin, stdout, and stderr.

**--blkio-weight**=*0*
   Block IO weight (relative weight) accepts a weight value between 10 and 1000.

**--blkio-weight-device**=[]
   Block IO weight (relative device weight, format: `DEVICE_NAME:WEIGHT`).

**--cap-add**=[]
   Add Linux capabilities

**--cap-drop**=[]
   Drop Linux capabilities

**--cgroup-parent**=""
   Path to cgroups under which the cgroup for the container will be created. If the path is not absolute, the path is considered to be relative to the cgroups path of the init process. Cgroups will be created if they do not already exist.

**--cidfile**=""
   Write the container ID to the file

**--cpu-count**=*0*
    Limit the number of CPUs available for execution by the container.

    On Windows Server containers, this is approximated as a percentage of total CPU usage.

    On Windows Server containers, the processor resource controls are mutually exclusive, the order of precedence is CPUCount first, then CPUShares, and CPUPercent last.

**--cpu-period**=*0*
    Limit the CPU CFS (Completely Fair Scheduler) period

    Limit the container's CPU usage. This flag tell the kernel to restrict the container's CPU usage to the period you specify.

**--cpu-quota**=*0*
   Limit the CPU CFS (Completely Fair Scheduler) quota

   Limit the container's CPU usage. By default, containers run with the full
CPU resource. This flag tell the kernel to restrict the container's CPU usage
to the quota you specify.

**--cpu-rt-period**=0
   Limit the CPU real-time period in microseconds

   Limit the container's Real Time CPU usage. This flag tell the kernel to restrict the container's Real Time CPU usage to the period you specify.

**--cpu-rt-runtime**=0
   Limit the CPU real-time runtime in microseconds

   Limit the containers Real Time CPU usage. This flag tells the kernel to limit the amount of time in a given CPU period Real Time tasks may consume. Ex:
   Period of 1,000,000us and Runtime of 950,000us means that this container could consume 95% of available CPU and leave the remaining 5% to normal priority tasks.

   The sum of all runtimes across containers cannot exceed the amount allotted to the parent cgroup.

**--cpu-shares**=*0*
   CPU shares (relative weight)

   By default, all containers get the same proportion of CPU cycles. This proportion
can be modified by changing the container's CPU share weighting relative
to the weighting of all other running containers.

To modify the proportion from the default of 1024, use the **--cpu-shares**
flag to set the weighting to 2 or higher.

The proportion will only apply when CPU-intensive processes are running.
When tasks in one container are idle, other containers can use the
left-over CPU time. The actual amount of CPU time will vary depending on
the number of containers running on the system.

For example, consider three containers, one has a cpu-share of 1024 and
two others have a cpu-share setting of 512. When processes in all three
containers attempt to use 100% of CPU, the first container would receive
50% of the total CPU time. If you add a fourth container with a cpu-share
of 1024, the first container only gets 33% of the CPU. The remaining containers
receive 16.5%, 16.5% and 33% of the CPU.

On a multi-core system, the shares of CPU time are distributed over all CPU
cores. Even if a container is limited to less than 100% of CPU time, it can
use 100% of each individual CPU core.

For example, consider a system with more than three cores. If you start one
container **{C0}** with **-c=512** running one process, and another container
**{C1}** with **-c=1024** running two processes, this can result in the following
division of CPU shares:

    PID    container	CPU	CPU share
    100    {C0}		0	100% of CPU0
    101    {C1}		1	100% of CPU1
    102    {C1}		2	100% of CPU2

**--cpus**=0.0
   Number of CPUs. The default is *0.0* which means no limit.

**--cpuset-cpus**=""
   CPUs in which to allow execution (0-3, 0,1)

**--cpuset-mems**=""
   Memory nodes (MEMs) in which to allow execution (0-3, 0,1). Only effective on NUMA systems.

   If you have four memory nodes on your system (0-3), use `--cpuset-mems=0,1`
then processes in your container will only use memory from the first
two memory nodes.

**-d**, **--detach**=*true*|*false*
   Detached mode: run the container in the background and print the new container ID. The default is *false*.

   At any time you can run **kpod ps** in
the other shell to view a list of the running containers. You can reattach to a
detached container with **kpod attach**. If you choose to run a container in
the detached mode, then you cannot use the **-rm** option.

   When attached in the tty mode, you can detach from the container (and leave it
running) using a configurable key sequence. The default sequence is `CTRL-p CTRL-q`.
You configure the key sequence using the **--detach-keys** option or a configuration file.
See **config-json(5)** for documentation on using a configuration file.

**--detach-keys**=""
   Override the key sequence for detaching a container. Format is a single character `[a-Z]` or `ctrl-<value>` where `<value>` is one of: `a-z`, `@`, `^`, `[`, `,` or `_`.

**--device**=[]
   Add a host device to the container (e.g. --device=/dev/sdc:/dev/xvdc:rwm)

**--device-read-bps**=[]
   Limit read rate (bytes per second) from a device (e.g. --device-read-bps=/dev/sda:1mb)

**--device-read-iops**=[]
    Limit read rate (IO per second) from a device (e.g. --device-read-iops=/dev/sda:1000)

**--device-write-bps**=[]
    Limit write rate (bytes per second) to a device (e.g. --device-write-bps=/dev/sda:1mb)

**--device-write-iops**=[]
    Limit write rate (IO per second) to a device (e.g. --device-write-iops=/dev/sda:1000)

**--dns**=[]
   Set custom DNS servers

   This option can be used to override the DNS
configuration passed to the container. Typically this is necessary when the
host DNS configuration is invalid for the container (e.g., 127.0.0.1). When this
is the case the **--dns** flags is necessary for every run.

**--dns-option**=[]
   Set custom DNS options

**--dns-search**=[]
   Set custom DNS search domains (Use --dns-search=. if you don't wish to set the search domain)

**--entrypoint**=""
   Overwrite the default ENTRYPOINT of the image

   This option allows you to overwrite the default entrypoint of the image.
   The ENTRYPOINT of an image is similar to a COMMAND
because it specifies what executable to run when the container starts, but it is
(purposely) more difficult to override. The ENTRYPOINT gives a container its
default nature or behavior, so that when you set an ENTRYPOINT you can run the
container as if it were that binary, complete with default options, and you can
pass in more options via the COMMAND. But, sometimes an operator may want to run
something else inside the container, so you can override the default ENTRYPOINT
at runtime by using a **--entrypoint** and a string to specify the new
ENTRYPOINT.

**-e**, **--env**=[]
   Set environment variables

   This option allows you to specify arbitrary
environment variables that are available for the process that will be launched
inside of the container.

**--env-file**=[]
   Read in a line delimited file of environment variables

**--expose**=[]
   Expose a port, or a range of ports (e.g. --expose=3300-3310) to set up port redirection
   on the host system.

**--group-add**=[]
   Add additional groups to run as

**--hostname**=""
   Container host name

   Sets the container host name that is available inside the container.

**--help**
  Print usage statement

**-i**, **--interactive**=*true*|*false*
   Keep STDIN open even if not attached. The default is *false*.

**--ip**=""
   Sets the container's interface IPv4 address (e.g. 172.23.0.9)

   It can only be used in conjunction with **--network** for user-defined networks

**--ip6**=""
   Sets the container's interface IPv6 address (e.g. 2001:db8::1b99)

   It can only be used in conjunction with **--network** for user-defined networks

**--ipc**=""
   Default is to create a private IPC namespace (POSIX SysV IPC) for the container
			       'container:<name|id>': reuses another container shared memory, semaphores and message queues
			       'host': use the host shared memory,semaphores and message queues inside the container.  Note: the host mode gives the container full access to local shared memory and is therefore considered insecure.

**--kernel-memory**=""
   Kernel memory limit (format: `<number>[<unit>]`, where unit = b, k, m or g)

   Constrains the kernel memory available to a container. If a limit of 0
is specified (not using `--kernel-memory`), the container's kernel memory
is not limited. If you specify a limit, it may be rounded up to a multiple
of the operating system's page size and the value can be very large,
millions of trillions.

**-l**, **--label**=[]
   Add metadata to a container (e.g., --label com.example.key=value)

**--label-file**=[]
   Read in a line delimited file of labels

**--link-local-ip**=[]
   Add one or more link-local IPv4/IPv6 addresses to the container's interface

**--log-driver**="*json-file*|*syslog*|*journald*|*gelf*|*fluentd*|*awslogs*|*splunk*|*etwlogs*|*gcplogs*|*none*"
  Logging driver for the container. Default is defined by daemon `--log-driver` flag.
  **Warning**: the `kpod logs` command works only for the `json-file` and
  `journald` logging drivers.

**--log-opt**=[]
  Logging driver specific options.

**--mac-address**=""
   Container MAC address (e.g. 92:d0:c6:0a:29:33)

   Remember that the MAC address in an Ethernet network must be unique.
The IPv6 link-local address will be based on the device's MAC address
according to RFC4862.

**-m**, **--memory**=""
   Memory limit (format: <number>[<unit>], where unit = b, k, m or g)

   Allows you to constrain the memory available to a container. If the host
supports swap memory, then the **-m** memory setting can be larger than physical
RAM. If a limit of 0 is specified (not using **-m**), the container's memory is
not limited. The actual limit may be rounded up to a multiple of the operating
system's page size (the value would be very large, that's millions of trillions).

**--memory-reservation**=""
   Memory soft limit (format: <number>[<unit>], where unit = b, k, m or g)

   After setting memory reservation, when the system detects memory contention
or low memory, containers are forced to restrict their consumption to their
reservation. So you should always set the value below **--memory**, otherwise the
hard limit will take precedence. By default, memory reservation will be the same
as memory limit.

**--memory-swap**="LIMIT"
   A limit value equal to memory plus swap. Must be used with the  **-m**
(**--memory**) flag. The swap `LIMIT` should always be larger than **-m**
(**--memory**) value.  By default, the swap `LIMIT` will be set to double
the value of --memory.

   The format of `LIMIT` is `<number>[<unit>]`. Unit can be `b` (bytes),
`k` (kilobytes), `m` (megabytes), or `g` (gigabytes). If you don't specify a
unit, `b` is used. Set LIMIT to `-1` to enable unlimited swap.

**--memory-swappiness**=""
   Tune a container's memory swappiness behavior. Accepts an integer between 0 and 100.

**--name**=""
   Assign a name to the container

   The operator can identify a container in three ways:
    UUID long identifier (“f78375b1c487e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778”)
    UUID short identifier (“f78375b1c487”)
    Name (“jonah”)

   kpod generates a UUID for each container, and if a name is not assigned
to the container with **--name** then the daemon will also generate a random
string name. The name is useful when defining links (see **--link**) (or any
other place you need to identify a container). This works for both background
and foreground containers.

**--network**="*bridge*"
   Set the Network mode for the container
			       'bridge': create a network stack on the default bridge
			       'none': no networking
			       'container:<name|id>': reuse another container's network stack
			       'host': use the kpod host network stack.  Note: the host mode gives the container full access to local system services such as D-bus and is therefore considered insecure.
			       '<network-name>|<network-id>': connect to a user-defined network

**--network-alias**=[]
   Add network-scoped alias for the container

**--oom-kill-disable**=*true*|*false*
   Whether to disable OOM Killer for the container or not.

**--oom-score-adj**=""
    Tune the host's OOM preferences for containers (accepts -1000 to 1000)

**--pid**=""
   Set the PID mode for the container
   Default is to create a private PID namespace for the container
			       'container:<name|id>': join another container's PID namespace
			       'host': use the host's PID namespace for the container. Note: the host mode gives the container full access to local PID and is therefore considered insecure.

**--pids-limit**=""
   Tune the container's pids limit. Set `-1` to have unlimited pids for the container.

**--pod**=""
   Run container in an existing pod

**--privileged**=*true*|*false*
   Give extended privileges to this container. The default is *false*.

   By default, kpod containers are
“unprivileged” (=false) and cannot, for example, modify parts of the kernel.
This is because by default a container is not allowed to access any devices.
A “privileged” container is given access to all devices.

   When the operator executes **kpod run --privileged**, kpod enables access
to all devices on the host as well as set turn off most of the security messurs
protecting the host from the container.

**-p**, **--publish**=[]
   Publish a container's port, or range of ports, to the host

   Format: `ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort | containerPort`
Both hostPort and containerPort can be specified as a range of ports.
When specifying ranges for both, the number of container ports in the range must match the number of host ports in the range.
(e.g., `kpod run -p 1234-1236:1222-1224 --name thisWorks -t busybox`
but not `kpod run -p 1230-1236:1230-1240 --name RangeContainerPortsBiggerThanRangeHostPorts -t busybox`)
With ip: `kpod run -p 127.0.0.1:$HOSTPORT:$CONTAINERPORT --name CONTAINER -t someimage`
Use `kpod port` to see the actual mapping: `kpod port CONTAINER $CONTAINERPORT`

**-P**, **--publish-all**=*true*|*false*
   Publish all exposed ports to random ports on the host interfaces. The default is *false*.

   When set to true publish all exposed ports to the host interfaces. The
default is false. If the operator uses -P (or -p) then kpod will make the
exposed port accessible on the host and the ports will be available to any
client that can reach the host. When using -P, kpod will bind any exposed
port to a random port on the host within an *ephemeral port range* defined by
`/proc/sys/net/ipv4/ip_local_port_range`. To find the mapping between the host
ports and the exposed ports, use `kpod port`.

**--read-only**=*true*|*false*
   Mount the container's root filesystem as read only.

   By default a container will have its root filesystem writable allowing processes
to write files anywhere.  By specifying the `--read-only` flag the container will have
its root filesystem mounted as read only prohibiting any writes.

**--rm**=*true*|*false*
   Automatically remove the container when it exits. The default is *false*.
   `--rm` flag can work together with `-d`, and auto-removal will be done on daemon side. Note that it's
incompatible with any restart policy other than `none`.

**--security-opt**=[]
   Security Options


    "label=user:USER"   : Set the label user for the container
    "label=role:ROLE"   : Set the label role for the container
    "label=type:TYPE"   : Set the label type for the container
    "label=level:LEVEL" : Set the label level for the container
    "label=disable"     : Turn off label confinement for the container
    "no-new-privileges" : Disable container processes from gaining additional privileges

    "seccomp=unconfined" : Turn off seccomp confinement for the container
    "seccomp=profile.json :  White listed syscalls seccomp Json file to be used as a seccomp filter

    "apparmor=unconfined" : Turn off apparmor confinement for the container
    "apparmor=your-profile" : Set the apparmor confinement profile for the container

**--shm-size**=""
   Size of `/dev/shm`. The format is `<number><unit>`. `number` must be greater than `0`.
   Unit is optional and can be `b` (bytes), `k` (kilobytes), `m`(megabytes), or `g` (gigabytes).
   If you omit the unit, the system uses bytes. If you omit the size entirely, the system uses `64m`.

**--sig-proxy**=*true*|*false*
   Proxy received signals to the process (non-TTY mode only). SIGCHLD, SIGSTOP, and SIGKILL are not proxied. The default is *true*.

**--stop-signal**=*SIGTERM*
  Signal to stop a container. Default is SIGTERM.

**--stop-timeout**=*10*
  Timeout (in seconds) to stop a container. Default is 10.

**--storage-opt**=[]
   Storage driver options per container

   $ kpod create -it --storage-opt size=120G fedora /bin/bash

   This (size) will allow to set the container rootfs size to 120G at creation time.
   This option is only available for the `devicemapper`, `btrfs`, `overlay2` and `zfs` graph drivers.
   For the `devicemapper`, `btrfs` and `zfs` storage drivers, user cannot pass a size less than the Default BaseFS Size.
   For the `overlay2` storage driver, the size option is only available if the backing fs is `xfs` and mounted with the `pquota` mount option.
   Under these conditions, user can pass any size less then the backing fs size.

**--sysctl**=SYSCTL
  Configure namespaced kernel parameters at runtime

  IPC Namespace - current sysctls allowed:

  kernel.msgmax, kernel.msgmnb, kernel.msgmni, kernel.sem, kernel.shmall, kernel.shmmax, kernel.shmmni, kernel.shm_rmid_forced
  Sysctls beginning with fs.mqueue.*

  Note: if you use the --ipc=host option these sysctls will not be allowed.

  Network Namespace - current sysctls allowed:
      Sysctls beginning with net.*

  Note: if you use the --network=host option these sysctls will not be allowed.

**--tmpfs**=[] Create a tmpfs mount

   Mount a temporary filesystem (`tmpfs`) mount into a container, for example:

   $ kpod run -d --tmpfs /tmp:rw,size=787448k,mode=1777 my_image

   This command mounts a `tmpfs` at `/tmp` within the container.  The supported mount
options are the same as the Linux default `mount` flags. If you do not specify
any options, the systems uses the following options:
`rw,noexec,nosuid,nodev,size=65536k`.

**-t**, **--tty**=*true*|*false*
   Allocate a pseudo-TTY. The default is *false*.

   When set to true kpod will allocate a pseudo-tty and attach to the standard
input of the container. This can be used, for example, to run a throwaway
interactive shell. The default is false.

Note: The **-t** option is incompatible with a redirection of the kpod client
standard input.

**--ulimit**=[]
   Ulimit options

**-u**, **--user**=""
   Sets the username or UID used and optionally the groupname or GID for the specified command.

   The followings examples are all valid:
   --user [user | user:group | uid | uid:gid | user:gid | uid:group ]

   Without this argument the command will be run as root in the container.

**--userns**=""
   Set the usernamespace mode for the container when `userns-remap` option is enabled.
     **host**: use the host usernamespace and enable all privileged options (e.g., `pid=host` or `--privileged`).

**--uts**=*host*
   Set the UTS mode for the container
     **host**: use the host's UTS namespace inside the container.
     Note: the host mode gives the container access to changing the host's hostname and is therefore considered insecure.

**-v**|**--volume**[=*[[HOST-DIR:]CONTAINER-DIR[:OPTIONS]]*]
   Create a bind mount. If you specify, ` -v /HOST-DIR:/CONTAINER-DIR`, kpod
   bind mounts `/HOST-DIR` in the host to `/CONTAINER-DIR` in the kpod
   container. If 'HOST-DIR' is omitted,  kpod automatically creates the new
   volume on the host.  The `OPTIONS` are a comma delimited list and can be:

   * [rw|ro]
   * [z|Z]
   * [`[r]shared`|`[r]slave`|`[r]private`]

The `CONTAINER-DIR` must be an absolute path such as `/src/docs`. The `HOST-DIR`
can be an absolute path or a `name` value. A `name` value must start with an
alphanumeric character, followed by `a-z0-9`, `_` (underscore), `.` (period) or
`-` (hyphen). An absolute path starts with a `/` (forward slash).

If you supply a `HOST-DIR` that is an absolute path,  kpod bind-mounts to the
path you specify. If you supply a `name`, kpod creates a named volume by that
`name`. For example, you can specify either `/foo` or `foo` for a `HOST-DIR`
value. If you supply the `/foo` value, kpod creates a bind-mount. If you
supply the `foo` specification, kpod creates a named volume.

You can specify multiple  **-v** options to mount one or more mounts to a
container. To use these same mounts in other containers, specify the
**--volumes-from** option also.

You can add `:ro` or `:rw` suffix to a volume to mount it  read-only or
read-write mode, respectively. By default, the volumes are mounted read-write.
See examples.

Labeling systems like SELinux require that proper labels are placed on volume
content mounted into a container. Without a label, the security system might
prevent the processes running inside the container from using the content. By
default, kpod does not change the labels set by the OS.

To change a label in the container context, you can add either of two suffixes
`:z` or `:Z` to the volume mount. These suffixes tell kpod to relabel file
objects on the shared volumes. The `z` option tells kpod that two containers
share the volume content. As a result, kpod labels the content with a shared
content label. Shared volume labels allow all containers to read/write content.
The `Z` option tells kpod to label the content with a private unshared label.
Only the current container can use a private volume.

By default bind mounted volumes are `private`. That means any mounts done
inside container will not be visible on host and vice-a-versa. One can change
this behavior by specifying a volume mount propagation property. Making a
volume `shared` mounts done under that volume inside container will be
visible on host and vice-a-versa. Making a volume `slave` enables only one
way mount propagation and that is mounts done on host under that volume
will be visible inside container but not the other way around.

To control mount propagation property of volume one can use `:[r]shared`,
`:[r]slave` or `:[r]private` propagation flag. Propagation property can
be specified only for bind mounted volumes and not for internal volumes or
named volumes. For mount propagation to work source mount point (mount point
where source dir is mounted on) has to have right propagation properties. For
shared volumes, source mount point has to be shared. And for slave volumes,
source mount has to be either shared or slave.

Use `df <source-dir>` to figure out the source mount and then use
`findmnt -o TARGET,PROPAGATION <source-mount-dir>` to figure out propagation
properties of source mount. If `findmnt` utility is not available, then one
can look at mount entry for source mount point in `/proc/self/mountinfo`. Look
at `optional fields` and see if any propagaion properties are specified.
`shared:X` means mount is `shared`, `master:X` means mount is `slave` and if
nothing is there that means mount is `private`.

To change propagation properties of a mount point use `mount` command. For
example, if one wants to bind mount source directory `/foo` one can do
`mount --bind /foo /foo` and `mount --make-private --make-shared /foo`. This
will convert /foo into a `shared` mount point. Alternatively one can directly
change propagation properties of source mount. Say `/` is source mount for
`/foo`, then use `mount --make-shared /` to convert `/` into a `shared` mount.

To disable automatic copying of data from the container path to the volume, use
the `nocopy` flag. The `nocopy` flag can be set on bind mounts and named volumes.

**--volumes-from**=[]
   Mount volumes from the specified container(s)

   Mounts already mounted volumes from a source container onto another
   container. You must supply the source's container-id. To share
   a volume, use the **--volumes-from** option when running
   the target container. You can share volumes even if the source container
   is not running.

   By default, kpod mounts the volumes in the same mode (read-write or
   read-only) as it is mounted in the source container. Optionally, you
   can change this by suffixing the container-id with either the `:ro` or
   `:rw ` keyword.

   If the location of the volume from the source container overlaps with
   data residing on a target container, then the volume hides
   that data on the target.

**-w**, **--workdir**=""
   Working directory inside the container

   The default working directory for running binaries within a container is the root directory (/).
The image developer can set a different default with the WORKDIR instruction. The operator
can override the working directory by using the **-w** option.

# EXAMPLES

# HISTORY
August 2014, updated by Sven Dowideit <SvenDowideit@home.org.au>
September 2014, updated by Sven Dowideit <SvenDowideit@home.org.au>
November 2014, updated by Sven Dowideit <SvenDowideit@home.org.au>
October 2017, converted from Docker documentation to kpod by Dan Walsh for kpod <dwalsh@redhat.com>
