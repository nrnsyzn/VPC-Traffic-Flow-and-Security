# VPC Traffic Flow and Security

![Route Tables Overview](https://github.com/user-attachments/assets/60be7294-b166-4976-a469-aa8be26b2dbd)

This project demonstrates the setup of **route tables**, **security groups**, and **network ACLs** within an AWS VPC (Virtual Private Cloud) to manage traffic flow and security boundaries at both subnet and instance levels.

---

## 🛰️ Route Tables

Route tables act like a GPS for your VPC resources. They contain a set of rules — *routes* — that determine how network traffic is directed within and outside of your VPC.

### Key Concepts:

- **Association with Subnets**: Every subnet must be linked to a route table. This tells the subnet where to route its traffic.
- **Internet Access**: To make a subnet *public*, it must be associated with a route table that has a route to an Internet Gateway (IGW).

### Example:

| Destination      | Target |
|------------------|--------|
| `0.0.0.0/0`      | `igw`  |
| `172.31.0.0/16`  | `local`|

- `0.0.0.0/0 → igw` sends all internet-bound traffic to the Internet Gateway.
- `172.31.0.0/16 → local` allows internal traffic within the VPC.

In my setup, the route to the internet used:
- **Destination**: `0.0.0.0/0`
- **Target**: My custom Internet Gateway (`NextWork IG`)

![Route Table Screenshot](https://github.com/user-attachments/assets/9e938fb8-c532-4b24-915d-932b51762a84)

---

## 🔐 Security Groups

Security groups act as virtual firewalls that control traffic *at the instance level*. Each EC2 instance or resource must be associated with one or more security groups.

### Inbound & Outbound Rules:

- **Inbound Rules**: Control the traffic **entering** a resource.
- **Outbound Rules**: Control the traffic **leaving** a resource.

By default:
- All **outbound traffic is allowed**
- **Inbound traffic is denied**, unless explicitly allowed

For example, I configured an inbound rule to allow **HTTP (port 80)** traffic from all sources, enabling public access to a hosted web app.

![Security Group Screenshot](https://github.com/user-attachments/assets/e9d12193-62c6-44b4-8034-c870dc7df138)

---

## 🚦 Network ACLs (NACLs)

Network ACLs act as a subnet-level security filter, inspecting all traffic *entering or exiting* a subnet.

### Characteristics:

- Stateless: Rules must be defined for **both inbound and outbound** traffic.
- Ordered: Rules are evaluated in order (lowest-numbered first).
- Default NACLs: Allow all traffic.
- Custom NACLs: Deny all traffic by default until rules are defined.

NACLs are great for setting **broad security rules** — for instance, blocking all traffic from a suspicious IP range.

---

## 🔍 Security Groups vs. Network ACLs

| Feature                | Security Groups                | Network ACLs                  |
|------------------------|--------------------------------|-------------------------------|
| Scope                  | Instance-level                 | Subnet-level                  |
| Stateful               | ✅ Yes                         | ❌ No (stateless)             |
| Default behavior       | Deny inbound, allow outbound   | Allow all (default NACL)      |
| Rule evaluation        | All rules are evaluated        | Rules evaluated in order      |
| Use case               | Fine-grained access control    | Broad subnet-wide restrictions|

**Best Practice**: Use both in tandem — NACLs for broad subnet protections, and security groups for detailed control at the resource level. This layered security approach minimizes the risk of unauthorized access.

---

## 🧠 Summary

This project showcases the foundational principles of VPC networking and layered security in AWS:

- Route tables for directing traffic.
- Internet gateways for enabling public access.
- Security groups for instance-level control.
- Network ACLs for subnet-wide restrictions.

By combining these elements, you can ensure that your AWS resources are accessible only where intended and remain protected from unauthorized traffic.
