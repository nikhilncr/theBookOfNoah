# books
  - docker in action
    - jeff nickoloff


# background
  - about
    - launched in 2013
    - works with the OS to package, ship and run software
    - a tool for efficiently installing, removing, upgrading, distributing, trusting and managing software
  - use cases
    - easier to install and run software distributed with software
    - managing large scale systems or data centers
    - creating build, test and deployment pipelines
    - for use with networking applications, databases, mail servers, terminal applications, etc.
    - managing software dependencies
      - installed images can reuse existing dependencies
      - dependencies with different versions can coexist
    - improving portability
      - docker runs on all operating systems
      - thus, you can use software deesigned for windows/linux/etc on any other OS that supports docker
      - 

# terminology
  - jail
    - describes a modified runtime environment for a program that prevents that program from accessing protected services
  - container
    - aka jail
    - the goal has been expanded from preventing access to protected resources to isolating a process from all resources except where explicitly allowed
  - virtualization
    - hardware virtualization
      - aka virtual machines
      - provide virtual hardware on which an operating system and other programs can be installed
  - linux namespaces
    - help manage containers at runtime
    - wtf else?
  - cgroup
    - help manage containers at runtime
    - wtf else?
  - user space
    - memory alotted to run user software, e.g. cmdline programs and GUI things



# architecture
  - docker is a commandline program, a background daemon, and a set of remote services that take a logistical approach to solving common software problems
    - installing, running, publishing and removing software
  - stack
    - without docker
      - user space
        - cmd line, software, etc
      - operating system
        - is the interface between all user programs and the hardware that the computer is running on
      - hardware
        - cpu
        - memory
        - IO
          - network interface
          - perisstent storage
          - devices
    - with docker
      - user space
        - docker CLI
          - users interface directly with the CLI
          -the CLI interfaces with the docker daemon
        - docker daemon
          - interfaces with each container space
          - is the parent process to all containers
      - container space [A...Z]
        - each container runs as a child process of the docker daemon
        - the container, and the child process runs in its own memory subspace of the user spoace
        - programs running inside a container can access only their own memory and resources as scoped by the container
  - Container isolation
    - PID namespace
      - process identifiers and capabilities
    - UTS namespace
      - host and domain name
    - MNT namespace
      - file system access and structure
    - IPC namespace
      - process communication over shared memory
    - NET namespace
      - network access and structure
    - USR namespace
      - user names and identifiers
    - chroot()
      - controls the location of the file system root
    - cgroups
      - resource protection

## cmd line

## daemon
  - should always be running
  -
## containers
  - uses existing container engines (installed in linux) to provide consistent containers built according to best practices
    - any software run with docker is run isnide a container
    - software running inside docker containers interface directly with the hosts linux kernel
  - containers started from the same image dont share changes to their file system
## images
  - a bundled snapshot of all the files that should be available to programs running inside a container
  -

## registries and indexes
  - a set of infrastructure components that simplify distributing docker images
  -
# workflows
## simplified
  - create/install images
  - run containers
  -