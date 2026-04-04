---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
---

# Cloud concepts

Cloud service providers, such as Microsoft Azure and Amazon Web Services, offer companies and individuals convenient availability of various software, platform or infrastructure services on-demand for a fee, depending on the level of usage of the service (pay-as-you-go basis), saving expenses to individuals and opex for businesses

With cloud computing, companies can leverage resources like applications, server storage and cloud computing power, so that they don't have to maintain their own hardware infrastructure

Applications running in the cloud, as well as data, are distributed across many servers in a different regions or countries, ensuring high redundancy and disaster recovery

Cloud providers have an extensive set of security policies and monitoring practices, which in general makes them more secure than many traditional data centers.

Clouds provide unlimited resources compared to a traditional data center. Therefore, it is easy to scale up and down your cloud instances and services to increase their performance.

which streamlines the overall business maintenance and provides great flexibility and scalability

### Cloud deployment models

Cloud deployment models are based on the ownership model of the cloud infrastructure:

![AWS or Private Cloud or both, what's your strategy? - Cisco Blogs](<../.gitbook/assets/AWS or Private Cloud or both, what's your strategy - Cisco Blogs>)

**Private cloud (also called Virtual Private Cloud VPC)** cloud infrastructure is completely dedicated to a particular organization; there is no public access. Usually, it is located on the organization's premises, although it may be outsourced. This allows the organization to have full control over the infrastructure, helping ensure security, privacy, and customization according to their specific needs and requirements.

**Public cloud:** A third-party provider offers cloud infrastructure that is accessible to the general public. This means that individuals or organizations can access and use the services and resources provided by the cloud provider. Typically, there may be a fee associated with using public cloud services, which can vary based on the usage and specific offerings of the provider.

**Hybrid cloud:** The cloud infrastructure is a mix of at least two (usually private and public) cloud models. In this deployment model, the public cloud is often used to supplement the resources of the private cloud. This allows organizations to leverage the scalability and cost-effectiveness of the public cloud while maintaining control over sensitive data and applications in the private cloud.

**Multi-cloud:** Multi-cloud generally refers to the consumption of cloud services from two or more public cloud providers. It also often describes specific architectures where an app uses the same service model across multiple cloud providers, in some cases including on-premises data centers and colocation facilities.

### Cloud service models

#### Infrastructure-as-a-Service (IaaS)

Instead of purchasing all necessary equipment, clients can purchase IaaS, which offers virtualized cloud-based solution, including all equipment such as storage servers, computing power or networking hardware on a pay-as-you-go basis

It provides the clients control over physical hardware and software resources by allowing them to modify storage, CPU and RAM as well as configuring network resources within the cloud platform.

**Terraform** is an Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to define, provision, and manage infrastructure using a declarative configuration language called HCL (HashiCorp Configuration Language)

Infrastructure as Code – Define infrastructure in code form, making it repeatable and version-controlled.

Cloud-Agnostic – Works with AWS, Azure, Google Cloud, VMware, Kubernetes, and even on-prem infrastructure.

Declarative – You describe the desired state, and Terraform figures out how to achieve it.

State Management – Keeps track of the infrastructure state in a Terraform state file (terraform.tfstate).

Modular & Scalable – Supports modules for reusable configurations, making large-scale deployments easier.

**Terraform workflow**

Write Configuration – Define infrastructure using HCL (.tf files).

Initialize (terraform init) – Prepares Terraform by downloading necessary provider plugins.

Plan (terraform plan) – Shows a preview of changes Terraform will apply.

Apply (terraform apply) – Deploys the infrastructure.

Destroy (terraform destroy) – Removes all managed resources.

Unlike Ansible, Puppet, or Chef, which focus on configuring and maintaining routers,switches,servers, Terraform is designed primarily for provisioning infrastructure, making it a better choice for managing cloud environments.

![](<../.gitbook/assets/unknown (3).png>)

#### Platform-as-a-Service (PaaS)

**Platform-as-a-Service (PaaS)** offers customers to directly access prepared platform to develop, deploy, and manage applications without the complexity of building and maintaining the underlying infrastructure

**DaaS (Desktop as a service)** delivers cloud-based virtual desktop infrastructure (VDI) to end-users, allowing them to access working virtualized desktop with their personal device over the internet. The most popular provider is Citrix

![](<../.gitbook/assets/unknown (4).png>)

#### Software-as-a-Service (SaaS)

**Software-as-a-Service (SaaS)** cloud-based software or application is offered on a per-client or per-group subscription basis

The consumer is only provided with the application’s user interface, and applications are not installed on the user’s device

One common example of SaaS applications is Google Workplace and Microsoft Office 365 applications, in which users can access the applications like emails and Google Docs using a web browser.

![](<../.gitbook/assets/unknown (5).png>)

#### Network-as-a-Service (NaaS)

**Network-as-a-Service (NaaS)** works by applying a service-based model only to network equipment. The service provider owns, installs, and operates the hardware used by their customers, and the customer pays a monthly subscription fee for access.

NaaS can also be deployed as a cloud-based service, where routing switching and entire network infrastructure such as routers, switches and firewalls can be deployed as a virtual machines in a cloud, so that enterprise locations can have just one harware that is capable to access the internet, where their core routing and switching politics are enforced instead of having to maintain their own core site with all necessarry hardware

### How IoT devices are controlled remotely (NAT + cloud)

1\. Connecting the vacuum cleaner to the mains for the first time

After turning it on and connecting to Wi-Fi, the vacuum cleaner gets a private IP address (e.g. 192.168.1.x).

The vacuum cleaner establishes an outbound connection to the manufacturer's server (cloud), e.g. via MQTT or WebSocket.

This creates a so-called persistent connection (a long-term open socket) to the Internet.

Thanks to NAT, the router allows outgoing connections, and because the connection remains open, the server can respond back through this "channel".

2\. Mobile applications (e.g. from a mobile phone via LTE or other Wi-Fi)

The app does not communicate directly with the vacuum cleaner, because it has a private IP.

It communicates with the manufacturer's cloud, typically via HTTPS REST API.

For example, the app sends a request: "Start cleaning at 18:00".

3\. The cloud as an intermediary

The cloud server processes the request and sends it to the vacuum cleaner via an open connection.

This way, the vacuum cleaner will receive the command even if you're completely away from your home network.

If the connection drops (e.g. after restarting the router), the vacuum cleaner will re-establish it.

#### Technologies used

**MQTT (Message Queue Telemetry Transport):** a lightweight protocol common in IoT devices, ideal for push notifications.

**WebSockets:** A persistent bidirectional TCP connection.

**NAT traversal over an outbound connection:** since the connection starts from the inside, NAT allows it.

**TLS/SSL encryption:** communication security.
