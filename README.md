# Azure & Networking Concepts 🌐

## Overview
This guide provides an in-depth look at **Azure networking concepts** and configurations, covering **virtual networks (VNets), subnets, security groups, VPNs, load balancers, and DNS services**. It includes manual setup steps and automation via PowerShell and Azure CLI.

---

## 📌 Prerequisites
Before diving into Azure networking, ensure you have:
- An **Azure account** with appropriate permissions.
- **Azure CLI or PowerShell installed**.
- Basic knowledge of **networking (IP addresses, subnets, routing, and security groups).**

---

## 🔹 1. Setting Up a Virtual Network (VNet)
Azure **VNets (Virtual Networks)** allow resources to securely communicate with each other and the internet.

### **Manually via Azure Portal:**
1. Navigate to **Azure Portal** → **Virtual Networks** → **Create VNet**.
2. Enter:
   - **Name:** `MyVNet`
   - **Address Space:** `10.0.0.0/16`
   - **Subscription & Resource Group:** Choose appropriately.
3. Click **Create**.

### **Automating via Azure CLI:**
```bash
az network vnet create \
  --name MyVNet \
  --resource-group MyResourceGroup \
  --location eastus \
  --address-prefix 10.0.0.0/16
```

### **Automating via PowerShell:**
```powershell
New-AzVirtualNetwork -ResourceGroupName "MyResourceGroup" -Location "EastUS" -Name "MyVNet" -AddressPrefix "10.0.0.0/16"
```

---

## 🔹 2. Configuring Subnets
Subnets segment a VNet into logical sections for better security and performance.

### **Manually via Azure Portal:**
1. Navigate to your **Virtual Network**.
2. Under **Subnets**, click **Add Subnet**.
3. Enter:
   - **Name:** `AppSubnet`
   - **Subnet Address Range:** `10.0.1.0/24`
   - **Network Security Group (NSG):** Assign an existing NSG or create a new one.
4. Save changes.

### **Automating via Azure CLI:**
```bash
az network vnet subnet create \
  --resource-group MyResourceGroup \
  --vnet-name MyVNet \
  --name AppSubnet \
  --address-prefix 10.0.1.0/24
```

### **Automating via PowerShell:**
```powershell
Add-AzVirtualNetworkSubnetConfig -Name "AppSubnet" -AddressPrefix "10.0.1.0/24" -VirtualNetwork $vnet
```

---

## 🔹 3. Configuring Network Security Groups (NSGs)
NSGs control **inbound and outbound traffic** for Azure resources.

### **Manually via Azure Portal:**
1. Go to **Azure Portal** → **Network Security Groups**.
2. Click **Create NSG** → **Enter Name & Resource Group** → **Create**.
3. Add Inbound Rules:
   - Allow **HTTP (80)**, **HTTPS (443)**.
   - Allow **RDP (3389)** (for Windows) or **SSH (22)** (for Linux).
   - Deny all other inbound traffic for security.

### **Automating via Azure CLI:**
```bash
az network nsg create --resource-group MyResourceGroup --name MyNSG
az network nsg rule create \
  --resource-group MyResourceGroup \
  --nsg-name MyNSG \
  --name Allow_HTTP \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80 \
  --access Allow
```

### **Automating via PowerShell:**
```powershell
New-AzNetworkSecurityRuleConfig -Name "Allow-HTTP" -Protocol "Tcp" -Direction "Inbound" -Priority 100 -SourceAddressPrefix "*" -SourcePortRange "*" -DestinationAddressPrefix "*" -DestinationPortRange 80 -Access "Allow"
```

---

## 🔹 4. Setting Up a VPN Gateway
VPN Gateways enable **secure connectivity** between an on-premises network and Azure.

### **Manually via Azure Portal:**
1. Navigate to **Virtual Network Gateway** → **Create Gateway**.
2. Choose **VPN type (Route-based)**.
3. Set the **Public IP address**.
4. Click **Create**.

### **Automating via Azure CLI:**
```bash
az network vnet-gateway create \
  --resource-group MyResourceGroup \
  --name MyVPNGateway \
  --vnet MyVNet \
  --public-ip-address MyPublicIP \
  --gateway-type Vpn \
  --vpn-type RouteBased
```

### **Automating via PowerShell:**
```powershell
New-AzVirtualNetworkGateway -ResourceGroupName "MyResourceGroup" -Location "EastUS" -Name "MyVPNGateway" -GatewayType "Vpn" -VpnType "RouteBased"
```

---

## 🔹 5. Configuring an Azure Load Balancer
Azure Load Balancers distribute traffic across multiple virtual machines.

### **Automating via Azure CLI:**
```bash
az network lb create \
  --resource-group MyResourceGroup \
  --name MyLoadBalancer \
  --sku Standard \
  --public-ip-address MyPublicIP \
  --frontend-ip-name MyFrontEnd \
  --backend-pool-name MyBackEndPool
```

### **Automating via PowerShell:**
```powershell
New-AzLoadBalancer -ResourceGroupName "MyResourceGroup" -Name "MyLoadBalancer" -Sku "Standard"
```

---

## 🔹 6. Configuring Azure DNS
Azure DNS provides **domain name resolution** for Azure resources.

### **Automating via Azure CLI:**
```bash
az network dns zone create --resource-group MyResourceGroup --name mydomain.com
```

### **Automating via PowerShell:**
```powershell
New-AzDnsZone -ResourceGroupName "MyResourceGroup" -Name "mydomain.com"
```

---

## ✅ Final Checks
- **Verify Virtual Network & Subnets**: `az network vnet list -o table`
- **Check NSG Rules**: `az network nsg rule list --nsg-name MyNSG -o table`
- **Confirm Load Balancer Health**: `az network lb show --name MyLoadBalancer -o table`

---

## 🎯 Conclusion
This guide covered essential **Azure networking concepts**, including **VNets, subnets, NSGs, VPN gateways, load balancers, and DNS services**. By automating setup with **Azure CLI and PowerShell**, you can efficiently manage network resources at scale.

📌 **Next Steps:**
- Set up **Azure Bastion** for secure remote access.
- Configure **ExpressRoute** for high-performance private connections.
- Implement **Azure Firewall & DDoS Protection** for security enhancements.

For further learning, explore [Microsoft Learn: Azure Networking](https://learn.microsoft.com/en-us/azure/networking/) 📖.

---

### **Author:** [Josue Corral](https://github.com/JosueCorralPro)  
**GitHub Repository:** [Azure Networking Setup](https://github.com/JosueCorralPro)
