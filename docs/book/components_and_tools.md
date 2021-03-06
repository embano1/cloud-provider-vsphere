## Components and Tools

Before diving into Kubernetes on vSphere, it's important to cover some key components and tools.The following section introduces key components
and tools that are part of any Kubernetes cluster running on vSphere. If you are familiar with Kubernetes and vSphere, you can skip this section.

### VM

A VM is an abstraction of an operating system from the physical machine by creating a "virtual" representation of the physical hardware the OS expects to interact with,
this includes but is not limited to CPU instruction sets, memory, BIOS, PCI buses, etc. A VM is an entirely self-contained entity and shares no components with the host OS.
In the case of vSphere the host OS is ESXi (see below).

### vSphere

vSphere is the product name of the two core components of the VMware Software Defined Datacenter (SDDC) stack, they are vCenter and ESXi. Each is discussed below in detail.

### ESXi

ESXi is the hypervisor, or "host" OS that is used to run VMs on. ESXi provides strong separation between VMs and itself, providing strong security boundaries between the
guest and host operating systems. ESXi can be used as a standalone entity, without vCenter but this is extremely uncommon and feature limited as without a higher level manager (vCenter)
ESXi cannot provide its most valuable features, like High Availability, vMotion, workload balancing and vSAN (a software defined storage stack).

### vCenter

vCenter can be thought of as the management layer for ESXi hosts. Hosts can be arranged into Datacenters, Clusters or resources pools, vCenter is the centralised monitoring and
management control plane for ESXi hosts allow centralised management, integration points for other products in the VMware SDDC stack and third party solutions, like backup, DR
or networking overlay applications, such as NSX. vCenter also provides all of the higher level features of vSphere such as vMotion, vSAN, HA, DRS, Distributed Switches and more.

### govmomi

govmomi is a Go library for interacting with VMware vSphere APIs - ESXi and/or vCenter

### Kubernetes

Kubernetes (K8s) is an open-source system for automating deployment, scaling, and management of containerized applications.

(source: kubernetes.io)

### kube-apiserver

The Kubernetes API server validates and configures data for the api objects which include pods, services, replicationcontrollers, and others.
The API Server services REST operations and provides the frontend to the cluster’s shared state through which all other components interact.

(source: kubernetes.io)

### kube-controller-manager

The Kubernetes controller manager is a daemon that embeds the core control loops shipped with Kubernetes. In applications of robotics and automation,
a control loop is a non-terminating loop that regulates the state of the system. In Kubernetes, a controller is a control loop that watches the
shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state. Examples of
controllers that ship with Kubernetes today are the replication controller, endpoints controller, namespace controller, and serviceaccounts controller.

(source: kubernetes.io)

### kube-scheduler

The Kubernetes scheduler is a policy-rich, topology-aware, workload-specific function that significantly impacts availability, performance, and capacity.
The scheduler needs to take into account individual and collective resource requirements, quality of service requirements, hardware/software/policy
constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, deadlines, and so on. Workload-specific requirements
will be exposed through the API as necessary.

(source: kubernetes.io)

**NOTE**: kube-scheduler will never ask the cloud provider for any information pertaining to scheduling, however, it may depend on information on resources
that were placed by other components like the kubelet.

### kubelet

The kubelet is the primary “node agent” that runs on each node. The kubelet works in terms of a PodSpec. A PodSpec is a YAML or JSON object that
describes a pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and ensures
that the containers described in those PodSpecs are running and healthy. The kubelet doesn’t manage containers which were not created by Kubernetes.

(source: kubernetes.io)

### kube-proxy

The Kubernetes network proxy runs on each node. This reflects services as defined in the Kubernetes API on each node and can do simple TCP, UDP,
and SCTP stream forwarding or round robin TCP, UDP, and SCTP forwarding across a set of backends. Service cluster IPs and ports are currently found
through Docker-links-compatible environment variables specifying ports opened by the service proxy. There is an optional addon that provides cluster DNS
for these cluster IPs. The user must create a service with the apiserver API to configure the proxy.

(source: kubernetes.io)

**NOTE**: kube-proxy will never ask the cloud provider for any information pertaining to network proxy, however, it may depend on information on resources
that were placed by other components like the kube-controller-manager.

### cloud-controller-manager

The Kubernetes cloud controller manager is a daemon that embeds the cloud specific control loops shipped with Kubernetes. Each cloud provider can run
the cloud controller manager as an addon to their cluster. The cloud-controller-manager defines a specification (Go interface) that must be implemented
by every cloud provider. Various expectations and behaviors of the cluster can be tuned based on the implementation set by the cloud provider.
Cloud providers are also free to run custom controllers as part of the cloud-controller-manager.

(source: kubernetes.io)

### etcd

Consistent and highly-available key value store used as Kubernetes’ backing store for all cluster data.

(source: kubernetes.io)
