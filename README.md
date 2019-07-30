# CircleCI Resource Classes :muscle:

Welcome to the CircleCI resource class preview docs! This document gives an overview of the `resource_class` and the different resource classes that are currently available.

:warning: **Please Read** :warning: 

**An eligible plan is required to use the `resource_class` feature. If you are currently on a container-based plan, you will need to upgrade to the [CircleCI Performance Plan](https://circleci.com/pricing/usage/).** To upgrade your plan, [log into CircleCI](https://circleci.com/vcs-authorize), and visit the settings for your organization.

Using `resource_class`, it is possible to configure CPU and RAM resources for each job. If `resource_class` is not specified or an invalid class is specified, the default `resource_class: medium` will be used. All of the resource classes are available, but some (marked with &#42;AR) require approval from the CircleCI support team to use. Reach out to CircleCI support to enable these for your account.

**In this document:**

1. [Docker](#docker)
2. [Remote Docker](#remote-docker)
3. [Linux VM](#linux-vm)
4. [GPU (Linux)](#gpu-linux)
5. [macOS](#macos)

<a name="docker"></a>
## Docker
The `docker` key sets Docker containers as the underlying technology to run your job. The Docker image you specify in your configuration is the primary container image in which all steps run.

### Docker (Linux) resource classes
`resource_class` | vCPU | Memory (GB) | Credits/Minute
:--- | :---: | :---: | :---:
`small` | 1 | 2 | 5
`medium` | 2 | 4 | 10
`medium+` | 3 | 6 | 15
`large` | 4 | 8 | 20
`xlarge` | 8 | 16 | 40

### Example Docker job configuration

```yaml
version: 2.1
jobs:
  myDockerJob:
    docker:
      - image: foo/bar:0.0.1
    resource_class: large # Uses the "large" Docker executor
    steps:
      - run: echo "This runs in a large Docker container"
```

<<a name="remote-docker"></a>
## Remote Docker
The Remote Docker resource class attaches to builds running in a Docker container so that you can execute Docker commands such as `docker build`. Note that you only need to use remote Docker if the job is running in the Docker executor. Machine and macOS executors can run Docker commands natively.

### Remote Docker resource class
`resource_class` | vCPU | Memory (GB) | Credits/Minute
:--- | :---: | :---: | :---:
`medium` | 2 | 7.5 | 10

### Remote Docker job configuration
```yaml
version: 2.1
jobs:
  myDockerJob:
    docker:
      - image: foo/bar:0.0.1
    resource_class: large # Uses the "large" Docker executor
    steps:
      # ... steps for building/testing app...

      - setup_remote_docker # attaches remote docker resource class
```

<a name="linux-vm"></a>
## Linux VM
The `machine` executor runs your job in a dedicated, ephemeral virtual machine. Using the `machine` executor gives your application full access to OS resources.

### Linux VM resource classes
`resource_class` | vCPU | Memory (GB) | Credits/Minute
:--- | :---: | :---: | :---:
`medium` | 2 | 7.5 | 10
`medium+` | 3 | 6 | 15
`large` (&#42;AR) | 4 | 15 | 20

(&#42;AR) Approval required to access. Reach out to your Customer Success Manager or to [CircleCI Support](https://support.circleci.com/hc/en-us/requests/new) to request access.

### Example Linux machine job configuration
```yaml
version: 2.1
jobs:
  myLinuxVMJob:
    machine:
      image: circleci/classic:latest
    resource_class: medium+ # Uses the "medium+" machine executor
    steps:
      - run: echo "This runs in a large Linux VM"
```

<a name="gpu-linux"></a>
## GPU (Linux)
The `gpu` resource class for the `machine` executor runs your job in a virtual machine equipped with GPU. GPU resources are powerful machines designed to run applications that require highly complex calculations. Virtual reality, deep learning, and artificial intelligence are common use cases for GPU.

### GPU resource classes
`resource_class` | vCPU | Memory (GiB) | GPUs | GPU Memory (GiB) | Credits/Minute
:--- | :---: | :---: | :---: | :---: | :---:
`gpu.small` (&#42;AR) | 16 | 122 | 1 | 8 | 80
`gpu.medium` (&#42;AR) | 32 | 244 | 2 | 16 | 160
`gpu.large` (&#42;AR) | 64 | 488 | 4 | 32 | 320

(&#42;AR) Approval required to access. Reach out to your Customer Success Manager or to [CircleCI Support](https://support.circleci.com/hc/en-us/requests/new) to request access.

### Example GPU (Linux) job configuration
```yaml
version: 2.1
jobs:
  myGPUJob:
    machine:
      image: circleci/classic:latest
    resource_class: gpu.medium # Uses the "gpu.medium" machine executor
    steps:
      - run: echo "This runs in a medium GPU enabled Linux VM"
```

<a name="macos"></a>
## macOS
The macOS executor runs your job in a macOS environment on a VM. You can also specify which version of Xcode should be used.

### macOS resource classes
`resource_class` | vCPU | Memory (GB) | Credits/Minute
:--- | :---: | :---: | :---:
`medium` | 4 | 8 | 50
`large` (&#42;AR) | 8 | 16 | 100

(&#42;AR) Approval required to access. Reach out to your Customer Success Manager or to [CircleCI Support](https://support.circleci.com/hc/en-us/requests/new) to request access.

### Example macOS job configuration
```yaml
version: 2.1
jobs:
  myMacOSJob:
    macos:
      xcode: "9.0" # uses Xcode version
    resource_class: large # Uses the "large" macOS executor
    steps:
      - run: echo "This runs in a large macOS container"
```
