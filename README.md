# Awesome-Container-Runtimes

## Top Container Runtimes Ecosystem



**Curated List of SaaS Products & Open-Source GitHub Projects**  

*Focused on Container Execution, Isolation & OCI Standards*  

**Last updated: October 2026**



This repository tracks notable **SaaS platforms** and **open-source projects** for **Container Runtimes**. These tools enable organizations to execute containers on hosts and clusters—from the standard `runc` to sandboxed runtimes that provide hardware-level isolation for untrusted workloads, and from the Docker daemon to daemonless alternatives that improve security posture.



**Examples** include containerd, CRI-O, Podman, Docker Engine, runc, Kata Containers, gVisor, Firecracker, Youki, and crun (the category leaders).



**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, container isolation, and open runtime infrastructure—ideal for platform engineers, security teams, and organizations seeking transparent, vendor-independent container execution.



Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.



## Table of Contents

- [SaaS/Hosted Platforms](#saas-hosted-platforms)

- [Open-Source GitHub Projects](#open-source-github-projects)

- [How to Contribute](#how-to-contribute)

- [Disclaimer](#disclaimer)



## SaaS/Hosted Platforms



- **Managed Kubernetes Runtime Defaults** (GKE, EKS, AKS)  

  All major managed Kubernetes services ship **containerd as the default runtime** . GKE additionally offers **gVisor as a first-class sandbox option** via GKE Sandbox . These platforms abstract runtime choice but allow RuntimeClass configuration for sandboxed workloads.



- **Docker Desktop**  

  **Desktop container environment** for Mac, Windows, and Linux. Features **Docker Engine**, **Docker Compose**, and **Kubernetes integration**. **Note**: Commercial licensing required for larger organizations ($9-$24/user/month) .



## Open-Source GitHub Projects



- **[containerd](https://github.com/containerd/containerd)**  

  **An open and reliable container runtime** (Apache-2.0) . **The industry-standard high-level runtime**—**CNCF Graduated project** donated by Docker in 2017 . **The default runtime in GKE, EKS, AKS, and most on-premises Kubernetes distributions** . **Architecture**: **shim API** provides stable interfaces with each container getting its own shim process—**if containerd daemon restarts, running containers continue uninterrupted** . **Features**: **image management**, **snapshots for layer storage**, **plugin system**, **NRI (Node Resource Interface) 2.0** for dynamic GPU/CPU pinning, **Sandbox API** for Kata/gVisor integration, and **runwasi shim** for WebAssembly modules . **v2.0 GA** (September 2024) adds **Image Transfer Service** for simplified pull/push retries and progress monitoring . **Latest v2.3.5** (September 2026) includes security updates (CVE-2026-53495) and hardening for stripping sensitive authentication data during image distribution . **The reference implementation for Kubernetes container runtime**.



- **[CRI-O](https://github.com/cri-o/cri-o)**  

  **OCI-based implementation of Kubernetes Container Runtime Interface** (Apache-2.0) . **The Kubernetes-only lightweight runtime**—**designed by Red Hat specifically for Kubernetes CRI, nothing else** . **Positioned differently from containerd**: containerd targets both Docker and Kubernetes; **CRI-O focuses exclusively on Kubernetes** . **Release cadence**: **1:1 matching with Kubernetes minor versions**—K8s 1.31 → CRI-O 1.31 . **Resource efficiency**: **~50 MB memory overhead per node** vs. containerd's ~100 MB, **50-100ms container start time** vs. containerd's 100-200ms . **The default runtime for OpenShift 4.16+** . **2026 additions**: **stream isolation** for logs/exec (separates from OOM cascade), **userns auto-mode** for rootless UID mapping, and **WebAssembly runtime class support** . **Latest v1.36.5** (September 2026) is a patch release addressing bug fixes and regressions . **The reference implementation for Kubernetes-only minimal runtime**.



- **[Podman](https://github.com/containers/podman)**  

  **A tool for managing OCI containers and pods** (Apache-2.0) . **The daemonless, rootless Docker alternative**—**`podman run` uses fork-exec; no daemon means no single point of failure** . **Key differentiators**: **rootless by default** (user namespaces + slirp4netns + fuse-overlayfs), **Pod concept** for grouping containers with shared IP/volumes, **Docker CLI compatibility** (`alias docker=podman` works for most workflows), and **`podman generate kube`** to export running pods as Kubernetes YAML . **Security advantage**: **eliminates the privileged Docker socket** (`/var/run/docker.sock`)—anything with access can spawn containers or escalate privileges . **Podman's rootless model limits compromise to user permissions, not root-level system access** . **Performance**: **15-20% lower memory per container** than Docker, **0.7-1.1s container startup** vs. Docker's 1.0-1.6s . **Ecosystem**: **Buildah** (building), **Skopeo** (image inspection/copy)—the Red Hat trio . **Latest v6.1.1** (September 2026) addresses CVE-2026-17106 (tar archive path traversal) and fixes rootlessport IPv4/IPv6 bind behavior . **The reference implementation for daemonless rootless containers**.



- **[Docker Engine (Moby)](https://github.com/moby/moby)**  

  **The container engine that started it all** (Apache-2.0) . **The developer-favorite runtime**—**largest ecosystem (Docker Hub, Compose, extensions), industry-standard CLI, and polished desktop GUI** . **Architecture**: **5-layer call chain** (`docker run → dockerd → containerd → containerd-shim → runc`) vs. containerd's 4 layers . **Performance trade-off**: **~200 MB memory overhead per node** and **200-400ms container start time**—the extra dockerd layer adds latency . **Security concern**: **root-owned daemon and privileged socket widen attack surface**—compromised CI/CD jobs are a common vector . **When to use**: **development environments**, **Docker Compose workflows**, **Windows/macOS native experience** . **When to avoid**: **Kubernetes production nodes**—dockershim was removed in Kubernetes 1.24 . **The reference implementation for developer container workflows**.



- **[runc](https://github.com/opencontainers/runc)**  

  **CLI tool for spawning and running containers according to the OCI specification** (Apache-2.0) . **The universal low-level runtime**—**the default OCI runtime for Docker, containerd, CRI-O, and Podman** . **Written in Go**, implements the **OCI Runtime Specification**. **The reference implementation for OCI container execution**.



- **[crun](https://github.com/containers/crun)**  

  **A fast and lightweight OCI runtime written in C** (GPL-2.0) . **The performance-focused runc alternative**—**written in C for better performance than Go-based runc** . **The default OCI runtime for CRI-O 1.31 and Podman 5** . **Wasm support**: **crun 1.20+ can launch WasmEdge/Wasmtime directly via WebAssembly handler** . **The reference implementation for C-based OCI runtime**.



- **[Youki](https://github.com/containers/youki)**  

  **A container runtime written in Rust** (Apache-2.0) . **The memory-safe runc alternative**—**gaining traction as a lower-overhead drop-in replacement for runc, particularly in latency-sensitive environments** . **Performance**: **~290 ms cold start** for a Node.js image vs. containerd's 380 ms and CRI-O's 395 ms—**meaningful for FaaS-style workloads launching thousands of containers per minute** . **The emerging standard for Rust-based OCI runtime**.



- **[Kata Containers](https://github.com/kata-containers/kata-containers)**  

  **Open source container runtime with VM-level isolation** (Apache-2.0) . **The hardware-enforced isolation runtime**—**runs each container or pod inside a lightweight VM with its own kernel** . **Isolation**: **hypervisor boundary (KVM)**—same model that separates AWS EC2 instances . **Kernel exploit inside a Kata container compromises only that VM; host kernel is untouched** . **Performance**: **~150-300ms cold start**, **<10 MiB memory overhead + guest kernel**, **near-native CPU/IO performance** . **VMM flexibility**: supports **QEMU**, **Cloud Hypervisor**, and **Firecracker** as backends . **Confidential Containers**: **Kata 3.x ships with Intel TDX and AMD SEV-SNP support**—hardware-attested memory encryption where host operator cannot inspect container memory . **Kubernetes integration**: **native via CRI RuntimeClass**—install Kata, create RuntimeClass, specify in pod spec . **Operational complexity**: **low**—Kata handles VMM provisioning, guest kernel, networking, and lifecycle . **Best for**: **production Kubernetes workloads requiring VM-level isolation**, **multi-tenant databases**, **PCI-DSS/HIPAA/FedRAMP compliance** where checklist literally says "VM-level isolation" . **The reference implementation for VM-isolated containers**.



- **[gVisor](https://github.com/google/gvisor)**  

  **Application Kernel for Containers** (Apache-2.0) . **The user-space kernel sandbox**—**implements a user-space kernel (Sentry) in Go that intercepts container syscalls before they reach the host kernel** . **How it works**: container process makes a syscall → hits Sentry (not host kernel) → Sentry handles it in user space → limited "gofer" calls to restricted host process for actual I/O . **Security model**: **even if attacker achieves code execution and triggers kernel exploit payload, payload hits Sentry—a much smaller, auditable codebase than full Linux kernel** . **Performance**: **milliseconds boot** (no VM), **10-30% I/O overhead** (syscall tax), **minimal CPU-bound impact** . **Systrap mode** is now default—nearly eliminates early performance cliff . **Limitations**: **does not implement every Linux syscall**—io_uring not supported, certain ioctl operations fail, no GPU direct access . **Kubernetes**: **RuntimeClass** (`runtimeClassName: gvisor`) . **GKE offers gVisor as first-class "GKE Sandbox"** . **On EKS**: requires DaemonSet to install runsc + containerd config update—**systrap platform works on standard EC2** (no bare metal needed unlike Kata) . **Best for**: **CI/CD runners executing arbitrary user pipelines** (GitHub Actions self-hosted, GitLab runners), **ML inference APIs where users submit model weights/custom code**, **SaaS plugin execution**, **cloud IDE backends** . **The reference implementation for syscall-interception sandboxing**.



- **[Firecracker](https://github.com/firecracker-microvm/firecracker)**  

  **Secure and fast microVMs for serverless computing** (Apache-2.0) . **AWS's microVM manager**—**built for Lambda and Fargate, used in billions of invocations per day** . **Architecture**: **stripped to absolute minimum**—no BIOS, no PCI bus, no USB, no legacy devices. Just **virtio network, virtio block, serial console** . **Codebase**: **~50,000 lines of Rust** vs. QEMU's ~2 million lines of C . **Performance**: **~100-200ms boot to userspace**, **<5 MiB memory overhead per microVM**, **up to 150 microVMs per second per host** . **Security**: **jailer process with seccomp, cgroup, namespace restrictions**—cannot access host filesystem, no hardware passthrough . **Device emulation attack surface**: **~30 surfaces vs. QEMU's hundreds** . **Operational challenge**: **Firecracker doesn't include orchestration**—requires building kernel image management, root filesystem, networking, jailer security, and VM lifecycle . **Most teams use Firecracker through Kata Containers rather than directly** . **Used by**: **AWS Lambda/Fargate**, **Fly.io Machines** (Firecracker fork core primitive) . **Best for**: **custom serverless platforms**, **high-density multi-tenant workloads** . **The reference implementation for serverless microVMs**.



- **[Mirantis Container Runtime](https://docs.mirantis.com/mcr/)**  

  **Enterprise container runtime** (commercial) . **The enterprise-hardened Docker Engine**—**incorporates all features and fixes from Moby versions 26 through 29** . **Key updates in MCR 29**: **containerd image store now production-ready and default** (multi-platform images, Wasm support, pluggable snapshotters), **IPv6 improvements** (ip6tables enabled by default, automatic ULA prefix assignment), **overlay network address usage statistics**, and **Swarm service memory control** . **Minimum API version increased to 1.40** (MCR 19.03) . **Platform support**: RHEL 8.10/9.6, Rocky 8.10, SLES 15 SP7, Ubuntu 22.04/24.04, Windows Server 2019/2022 Core . **The enterprise-hardened Docker alternative**.



- **[Apple Containerization](https://github.com/apple/container)**  

  **A tool for creating and running Linux containers using lightweight virtual machines on a Mac** (Swift) . **Apple's native container solution**—**runs containers directly on macOS without Docker Desktop's VM overhead** . **Architecture**: **Container CLI** → **Containerization** (container creation) → **Virtualization.framework** (hypervisor API) → **Hypervisor.framework** (low-level) . **Similar to WSL2**—launches lightweight VM per container to run Linux kernel on non-Linux host . **OCI-compliant**—Kali Linux images work . **Availability**: **public release for macOS Tahoe 26 and Sequoia 15** . **Limitations**: **known network issues on Sequoia 15** (containers may not get IP addresses) . **Note**: **Docker Desktop is aware but focused on Docker VMM backend**—Apple Container as Docker Desktop backend is not currently planned . **The reference implementation for native macOS containers**.



### Additional Strong Open-Source Options



- **[Lima](https://github.com/lima-vm/lima)** — Linux VMs on macOS with automatic file sharing and port forwarding (Apache-2.0) . **The most popular macOS VM manager** for container workloads .

- **[Colima](https://github.com/abiosoft/colima)** — Container runtimes on macOS with minimal setup (MIT) . **The Lima-based Docker/containerd runtime** for macOS .

- **[nerdctl](https://github.com/containerd/nerdctl)** — Docker-compatible CLI for containerd (Apache-2.0) . **The missing CLI for containerd**—provides Docker-like commands for standalone containerd .

- **[crictl](https://github.com/kubernetes-sigs/cri-tools)** — CLI for CRI-compatible container runtimes (Apache-2.0) . **The standard CRI debugging tool** .

- **[WasmEdge](https://github.com/WasmEdge/WasmEdge)** — Lightweight, high-performance, and extensible WebAssembly runtime (Apache-2.0) . **The CNCF WebAssembly runtime** for edge and container integration .

- **[containerd-shim-wasmtime](https://github.com/containerd/runwasi)** — WebAssembly runtime for containerd (Apache-2.0) . **Runs Wasm modules as containers** via containerd's runwasi shim .

- Community **n8n** workflows for container runtime monitoring and security alerting.



**Frameworks for building custom systems**: Combine **containerd** or **CRI-O** for high-level Kubernetes runtime with **runc** or **crun** for low-level execution, **Kata Containers** for VM-isolated workloads, **gVisor** for syscall-intercepted sandboxes, and **Podman** for daemonless rootless development—creating a comprehensive container runtime stack.



## How to Contribute



1. Fork the repo.

2. Add/edit entries in `README.md` (follow existing format).

3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.

4. Submit PR with a short explanation.



Star the repo if you find it useful!



## Disclaimer



- This is a **community-curated** list — not exhaustive and not an endorsement.

- Container runtimes handling production workloads must follow security best practices, implement appropriate isolation, and comply with applicable regulations.

- **Open-source container runtimes are mature and production-ready.** containerd is the default in GKE, EKS, AKS, and most on-prem distributions . CRI-O is the Red Hat-aligned minimal alternative . Podman provides daemonless rootless containers . **These are genuine alternatives to Docker Engine**.

- **The containerd vs. CRI-O choice depends on your ecosystem.** containerd for **broad ecosystem support, Docker compatibility, and embedded use cases** . CRI-O for **pure Kubernetes environments, Red Hat/OpenShift, and minimal attack surface** . **Both are production-grade**—containerd has **~100 MB memory overhead** vs. CRI-O's **~50 MB** .

- **The isolation spectrum matters for security.** **runc/crun** share the host kernel—a kernel CVE is a container escape risk . **gVisor** intercepts syscalls in user space—reduces kernel attack surface but breaks some workloads (io_uring, certain ioctls) . **Kata Containers** runs each container in a VM with its own kernel—**hardware-enforced boundary** where a guest kernel exploit does not reach the host . **Choose based on your threat model**: first-party internal services rarely justify VM-level isolation; multi-tenant or user-supplied code workloads demand it .

- **gVisor vs. Kata Containers is a complexity vs. security trade-off.** **Kata provides stronger security** (hypervisor escape is harder than Sentry bug) but **requires nested virtualization** on most clouds . **gVisor provides meaningful isolation improvement with lower operational complexity**—**systrap platform works on standard EC2** (no bare metal needed unlike Kata) . **gVisor's limitation**: does not implement every Linux syscall—**pip/apt install may fail for packages using unsupported syscalls** .

- **Firecracker is the serverless foundation.** **~125ms cold start**, **<5 MiB memory per microVM**, **150 microVMs/second per host** . **But it includes no orchestration**—**most teams use it through Kata Containers rather than directly** . **Used by AWS Lambda, Fargate, and Fly.io Machines** .

- Self-hosted open-source solutions require proper kernel compatibility (eBPF for some security tools, KVM for Kata/Firecracker), runtime configuration, and ongoing maintenance. **Container isolation is your security boundary**—choose runtime based on actual threat model, not feature lists.



---



**Made for platform engineers, security teams, and organizations building transparent container runtime solutions.**  

Let's make Container Runtimes more open, data-driven, and secure.
