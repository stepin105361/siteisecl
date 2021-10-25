# Common Cluster Deployment Guide 

Common Cluster deployment is for deploying both Foundational Security and Secure Key Caching components in a common Kubernetes cluster. The worker nodes may be separate with `TXT/BTG/SUEFI` enabled on a specific worker node and `SGX` on a separate worker node or all Intel Security hardware features enabled on a single worker node. 

This guide goes over the deployment steps to enable Common Cluster for Foundational Security and Secure Key Caching

## Pre-requisites

### Binary Deployment

Binary deployment would enable all components of Foundational Security and Secure Key Caching to be deployed as binary systemd services in a VM and Host for Agent systemd services. Refer the following links for the pre-requisites and requirements

All Hardware, OS , Network, RPM's requirements are given the table below

| Use case                         | Details                                                      |
| -------------------------------- | ------------------------------------------------------------ |
| Foundational & Workload Security | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Foundational%20&%20Workload%20Security%20-%20Containerization.md#hardware--os-requirements) |
| Secure Key Caching               | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Secure%20Key%20Caching%20-%20Containerization.md#hardware--os-requirements) |

All Build steps and pre-requisites are given in the table below

| Use case                         | Details                                                      |
| -------------------------------- | ------------------------------------------------------------ |
| Foundational & Workload Security | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Foundational%20&%20Workload%20Security%20-%20Containerization.md#hardware--os-requirements) |
| Secure Key Caching               | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Secure%20Key%20Caching%20-%20Containerization.md#hardware--os-requirements) |

### Containerized Deployment

Containerized deployment would enable all components of Foundational Security and Secure Key Caching to be deployed as containers in a Kubernetes cluster. Refer the following links for the pre-requisites and requirements

All Hardware, OS , Network, RPM's requirements are given the table below

| Use case                         | Details                                                      |
| -------------------------------- | ------------------------------------------------------------ |
| Foundational & Workload Security | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Foundational%20&%20Workload%20Security%20-%20Containerization.md#hardware--os-requirements) |
| Secure Key Caching               | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Secure%20Key%20Caching%20-%20Containerization.md#hardware--os-requirements) |

All Build steps and pre-requisites are given in the table below

| Use case                         | Details                                                      |
| -------------------------------- | ------------------------------------------------------------ |
| Foundational & Workload Security | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Foundational%20&%20Workload%20Security%20-%20Containerization.md#hardware--os-requirements) |
| Secure Key Caching               | [Quick Start Guide](https://github.com/intel-secl/docs/blob/master/quick-start-guides/Quick%20Start%20Guide%20-%20Intel%C2%AE%20Security%20Libraries%20-%20Secure%20Key%20Caching%20-%20Containerization.md#hardware--os-requirements) |
