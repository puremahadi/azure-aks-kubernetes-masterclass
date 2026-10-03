# Create AKS Cluster

## Step-01: Introduction
- Create Azure AKS Cluster
- Connect to Azure AKS Cluster using Azure Cloud Shell
- Explore Azure AKS Cluster Resources using kubectl cli and Azure Portal
- Install Azure CLI, kubectl CLI on local desktop and connect to Azure AKS Cluster using Azure CLI from local desktop
- Deploy Sample Application on AKS Cluster and test
- Clean-up Kubernetes resources deployed as part of this demo

## Step-02: Create AKS Cluster
- Create Kubernetes Cluster
### Basics
- **Subscription:** StackSimplify-Paid-Subscription
- **Resource Group:** Creat New: aks-rg1
- **Cluster preset configuration:** Dev/Test
- **Kubernetes Cluster Name:** aksdemo1  
- **Region:** (US) East US
- **Fleet Manager:** NONE (LEAVE TO DEFAULT)
- **Availability zones:** NONE (LEAVE TO DEFAULT)
- **AKS Pricing Tier:** Free
- **Kubernetes Version:** Select what ever is latest stable version
- **Automatic upgrade:** Enabled with patch (recommended)
- **Node security channel type:** Node Image (LEAVE TO DEFAULT)
  - **Security channel scheduler:** Every week on Sunday (recommended)
- **Authentication and Authorization:** 	Local accounts with Kubernetes RBAC    
### Node Pools
- In Nodepools, **Update node pool**
  - **Node pool name:** agentpool (LEAVE TO DEFAULT)
  - **Mode:** system (LEAVE TO DEFAULT)
  - **OS SKU:** Ubuntu Linux  (LEAVE TO DEFAULT)
  - **Availability zones:** ZONES 1,2,3 (LEAVE TO DEFAULT)
  - **Node size:** Standard D2ls v6 (2 vcpus, 4 GiB memory)
  - **Scale method:** Autoscale
  - **Minimum node count:** 2
  - **Maximum node count:** 5
  - REST ALL LEAVE TO DEFAULTS
  - Click on **UPDATE**
- REST ALL LEAVE TO DEFAULTS
### Networking
- **Private access**
  - Enable private cluster: UNCHECKED (LEAVE TO DEFAULTS)
- **Public access**
  - Set authorized IP ranges: UNCHECKED (LEAVE TO DEFAULTS)
- **Container networking:** 
  - Network configuration: Azure CNI Overlay
- **Bring your own Azure virtual network:** CHECKED  
  - Review all the auto-populated details 
  - Virtual Network
  - Cluster Subnet
  - Kubernetes Service address range
  - Kubernetes DNS Service IP Address
  - DNS Name prefix
- **Network Policy:** None (LEAVE TO DEFAULTS)
- **Load balancer:** Standard
### Integrations
  - **Azure Container Registry:** None
  - All leave to defaults
### Monitoring
  - All leave to defaults
### Security
  - All leave to defaults  
### Advanced
  - All leave to defaults  
### Tags
  - All leave to defaults 
### Review + Create
  - Click on **Create**


## Step-03: Cloud Shell - Configure kubectl to connect to AKS Cluster
- Go to https://shell.azure.com
```t
# Template
az aks get-credentials --resource-group <Resource-Group-Name> --name <Cluster-Name>

# Replace Resource Group & Cluster Name
az aks get-credentials --resource-group aks-rg1 --name aksdemo1

# Get kubectl client version only (shows client version only (no server required))
kubectl version --client=true

# Get kubectl version (Displays both client CLI and k8s server versions)
kubectl version 

# List Kubernetes Worker Nodes
kubectl get nodes 
kubectl get nodes -o wide
```

## Step-04: Explore Cluster Control Plane and Workload inside that
```t
# List Namespaces
kubectl get namespaces
kubectl get ns

# List Pods from all namespaces
kubectl get pods --all-namespaces

# List all k8s objects from Cluster Control plane
kubectl get all --all-namespaces
```

## Step-05: Explore the AKS cluster on Azure Management Console
- Explore the following features on high-level
  - Overview
  - Kubernetes Resources
  - Settings
  - Monitoring
  - Automation



## Step-06: Local Desktop - Install Azure CLI and Azure AKS CLI
```t
# Install Azure CLI (MAC)
brew update && brew install azure-cli

# Verify AZ CLI version
az --version

# Install Azure AKS CLI
sudo az aks install-cli

# Get kubectl client version only (shows client version only (no server required))
kubectl version --client=true

# Get kubectl version (Displays both client CLI and k8s server versions)
kubectl version

# Login to Azure
az login

# Configure Cluster Creds (kube config)
az aks get-credentials --resource-group aks-rg1 --name aksdemo1

# List AKS Nodes
kubectl get nodes 
kubectl get nodes -o wide
```
- **Reference Documentation Links**
- https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest
- https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest

## Step-07: Deploy Sample Application and Test
- Don't worry about what is present in these two files for now. 
- By the time we complete **Kubernetes Fundamentals** sections, you will be an expert in writing Kubernetes manifest in YAML.
- For now just focus on result. 
```t
# Deploy Application
kubectl apply -f kube-manifests/

# Verify Pods
kubectl get pods

# Verify Deployment
kubectl get deployment

# Verify Service (Make a note of external ip)
kubectl get service

# Access Application
http://<External-IP-from-get-service-output>

# Review the Kubernetes Resources in Azure Mgmt Console
Go to Kubernetes Resources
1. Namespaces
2. Workloads
3. Services and Ingress
```

## Step-07: Clean-Up
```t
# Delete Applications
kubectl delete -f kube-manifests/
```

## References
- https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-macos?view=azure-cli-latest

## Why Managed Identity when creating Cluster?
- https://docs.microsoft.com/en-us/azure/aks/use-managed-identity

Step-03 & 06: Azure AKS ক্লাস্টারের সাথে যুক্ত হওয়া ও ভেরিফিকেশন
• az aks get-credentials --resource-group aks-rg1 --name aksdemo1
	• কেন ব্যবহার করবেন: এটি আপনার লোকাল কম্পিউটার বা ক্লাউড শেলের সাথে Azure AKS ক্লাস্টারের সংযোগ তৈরি করে। এটি ব্যাকএন্ডে একটি গোপন ফাইল (.kube/config) ডাউনলোড করে, যা ছাড়া আপনি ক্লাস্টারে কোনো কাজ করতে পারবেন না।
	• কখন দিবেন: ক্লাস্টার তৈরি করার পর একদম শুরুতে মাত্র একবার অথবা অন্য কোনো কম্পিউটার থেকে ক্লাস্টার নিয়ন্ত্রণ করতে চাইলে এই কমান্ড দিতে হবে।
• kubectl version --client=true
	• কেন ব্যবহার করবেন: এটি শুধুমাত্র আপনার কম্পিউটারে বা ক্লাউড শেলে ইনস্টল করা kubectl টুলের নিজস্ব ভার্সন কত, তা দেখায় (ক্লাস্টারের সাথে কানেক্ট না থাকলেও এটি কাজ করবে)।
	• কখন দিবেন: আপনার লোকাল কম্পিউটারে টুলটি সঠিকভাবে ইনস্টল হয়েছে কিনা তা নিশ্চিত করতে।
• kubectl version
	• কেন ব্যবহার করবেন: এটি একই সাথে আপনার কম্পিউটারের kubectl ভার্সন এবং দূরবর্তী মূল Kubernetes ক্লাস্টার (Server)-এর ভার্সন দুটিই প্রদর্শন করে।
	• কখন দিবেন: আপনার লোকাল টুলটি ক্লাস্টারের সাথে সফলভাবে যোগাযোগ করতে পারছে কিনা (Connection Test) তা চেক করার জন্য।
• kubectl get nodes
	• কেন ব্যবহার করবেন: আপনার ক্লাস্টারে বর্তমানে কয়টি ভার্চুয়াল মেশিন বা ওয়ার্কার নোড (Worker Nodes) সচল আছে এবং সেগুলোর স্ট্যাটাস (Ready/NotReady) কী, তার একটি সংক্ষিপ্ত তালিকা দেখতে।
	• কখন দিবেন: ক্লাস্টারের স্বাস্থ্য পরীক্ষা করতে এবং নোডগুলো কাজ করছে কিনা তা দেখতে।
• kubectl get nodes -o wide
	• কেন ব্যবহার করবেন: এটি নোডগুলোর আরও বিস্তারিত তথ্য দেখায়, যেমন: নোডের ইন্টারনাল ও এক্সটারনাল আইপি (IP Address), অপারেটিং সিস্টেমের নাম এবং কার্নেল ভার্সন।
	• কখন দিবেন: যখন কোনো নেটওয়ার্কিংয়ের কাজ বা নোডের আইপি অ্যাড্রেস জানার প্রয়োজন হবে।
Step-04: ক্লাস্টারের ভেতরের অংশ বা ব্যাকএন্ড এক্সপ্লোর করা
• kubectl get namespaces অথবা kubectl get ns
	• কেন ব্যবহার করবেন: একটি কুবারনেটিস ক্লাস্টারকে ভার্চুয়ালি কয়েকটা ভাগে ভাগ করা যায়, যাকে Namespace বলে। এই কমান্ড দিয়ে ক্লাস্টারের ভেতরের সমস্ত বিভাগের তালিকা দেখা যায়।
	• কখন দিবেন: আপনার প্রজেক্ট বা অ্যাপ্লিকেশনটি কোন ডিপার্টমেন্ট বা বিভাগে আছে তা চেক করতে।
• kubectl get pods --all-namespaces
	• কেন ব্যবহার করবেন: কুবারনেটিসে অ্যাপ্লিকেশনের সবচেয়ে ছোট একক হলো Pod। এই কমান্ডের মাধ্যমে ক্লাস্টারের সব Namespace-এর ভেতরে যতগুলো Pod সচল বা অচল আছে, সবগুলোর তালিকা একসাথে দেখা যায়।
	• কখন দিবেন: আপনার নিজের অ্যাপের পাশাপাশি কুবারনেটিসের নিজস্ব সিস্টেম ও সিকিউরিটি পডগুলো ঠিকঠাক চলছে কিনা তা নিশ্চিত হতে।
• kubectl get all --all-namespaces
	• কেন ব্যবহার করবেন: এটি ক্লাস্টারের একটি মহা-তালিকা বা মাস্টার লিস্ট। সব Namespace-এর ভেতরে থাকা সমস্ত Pods, Services, Deployments, ReplicaSets ইত্যাদি এক স্ক্রিনে দেখায়।
	• কখন দিবেন: ক্লাস্টারের সামগ্রিক বর্তমান অবস্থার একটি পূর্ণাঙ্গ চিত্র একসাথে দেখতে চাইলে।
Step-06 (লোকাল ডেস্কটপ অংশ): টুল ইনস্টলেশন ও অ্যাপ্লিকেশন ডেপ্লয়মেন্ট
• brew update && brew install azure-cli
	• কেন ব্যবহার করবেন: ম্যাক (Mac) কম্পিউটারে Azure-এর অফিশিয়াল কমান্ড লাইন টুল (Azure CLI) ইনস্টল করার জন্য।
	• কখন দিবেন: আপনার লোকাল ম্যাক ল্যাপটপ থেকে Azure-এর সাথে কাজ শুরু করার একদম প্রথমে।
• az --version
	• কেন ব্যবহার করবেন: Azure CLI সফলভাবে ইনস্টল হয়েছে কিনা এবং এর ভার্সন কত তা চেক করতে।
	• কখন দিবেন: ইনস্টলেশন সম্পন্ন হওয়ার পর ভেরিফাই করতে।
• sudo az aks install-cli
	• কেন ব্যবহার করবেন: Azure CLI-এর মাধ্যমে কুবারনেটিস ম্যানেজ করার মূল টুল kubectl এবং kubelogin আপনার ল্যাপটপে ইনস্টল করার জন্য।
	• কখন দিবেন: লোকাল ল্যাপটপ থেকে সরাসরি কুবারনেটিস কমান্ড চালানোর ক্ষমতা পাওয়ার জন্য।
• az login
	• কেন ব্যবহার করবেন: আপনার ল্যাপটপ থেকে ব্রাউজার ওপেন করে আপনার Azure অ্যাকাউন্টে সাইন-ইন বা লগইন করার জন্য। (এটির পরেই মূলত আপনার সেই আগের 53003 এররটি এসেছিল, কারণ আপনার ল্যাপটপটি রেজিস্টার্ড ছিল না)।
	• কখন দিবেন: ক্লাস্টারের ক্রেডেনশিয়াল ডাউনলোড করার ঠিক আগে, যাতে Azure বুঝতে পারে আপনিই আসল মালিক।
• kubectl apply -f kube-manifests/
	• কেন ব্যবহার করবেন: kube-manifests/ ফোল্ডারের ভেতরে থাকা সমস্ত YAML কনফিগারেশন ফাইল রিড করে আপনার অ্যাপ্লিকেশনটি কুবারনেটিস ক্লাস্টারে লাইভ বা রান করানোর জন্য।
	• কখন দিবেন: যখন আপনি আপনার কোড বা অ্যাপ্লিকেশন ক্লাস্টারে ডেপ্লয় (Deploy) করতে চান।
• kubectl get pods
	• কেন ব্যবহার করবেন: আপনার অ্যাপ্লিকেশনের পডগুলো সফলভাবে চালু (Running) হয়েছে নাকি কোনো এরর (Error/CrashLoopBackOff) দেখাচ্ছে, তা দেখতে।
	• কখন দিবেন: অ্যাপ ডেপ্লয় করার ঠিক ১-২ মিনিট পর, স্ট্যাটাস চেক করার জন্য।
• kubectl get deployment
	• কেন ব্যবহার করবেন: আপনার অ্যাপ্লিকেশনটির ডেপ্লয়মেন্ট অবজেক্টের অবস্থা দেখতে, অর্থাৎ আপনি কয়টি কপি বা রেপ্লিকা চেয়েছিলেন আর বর্তমানে কয়টি লাইভ আছে তা চেক করতে।
	• কখন দিবেন: অ্যাপ্লিকেশন সঠিকভাবে স্কেল (Scale) হয়েছে কিনা তা দেখতে।
• kubectl get service
	• কেন ব্যবহার করবেন: আপনার অ্যাপ্লিকেশনের নেটওয়ার্ক সার্ভিস বা লোড ব্যালেন্সারের তথ্য দেখতে। এখান থেকেই মূলত External-IP পাওয়া যায়, যা দিয়ে বাইরের পৃথিবী থেকে আপনার অ্যাপটি দেখা যাবে।
	• কখন দিবেন: অ্যাপ ডেপ্লয় হওয়ার পর সেটির ইউজার ইন্টারফেস বা ওয়েবসাইট ব্রাউজারে ভিজিট করার জন্য আইপি খুঁজতে।
• kubectl describe nodes "node-name"
	• কেন ব্যবহার করবেন: কোনো নির্দিষ্ট নোড বা ভার্চুয়াল মেশিনের একদম নাড়িভুঁড়ি বা বিস্তারিত বিবরণ দেখতে (যেমন: কতটুকু CPU/RAM ফাঁকা আছে, কী কী এরর ইভেন্ট ঘটেছে)।
	• কখন দিবেন: কোনো নোড যদি হঠাৎ ডাউন বা NotReady হয়ে যায়, তখন ট্রাবলশুট করতে।
• kubectl describe pods "pods-name"
	• কেন ব্যবহার করবেন: কোনো নির্দিষ্ট Pod কেন চালু হচ্ছে না, কেন এরর দিচ্ছে বা ব্যাকএন্ডে কী সমস্যা হচ্ছে, তার লাইভ লগ এবং ইভেন্ট হিস্ট্রি দেখতে।
	• কখন দিবেন: যখন কোনো Pod-এর স্ট্যাটাস Error বা Pending দেখাবে, তখন মূল সমস্যাটি ধরার জন্য।
• kubectl port-forward service/myapp1-loadbalancer 8080:80 --address 0.0.0.0
	• কেন ব্যবহার করবেন: যদি Azure আপনাকে কোনো এক্সটারনাল আইপি না দেয় বা ক্লাউড নেটওয়ার্ক ব্লকের কারণে আপনি অ্যাপটি ব্রাউজারে দেখতে না পান, তখন কুবারনেটিসের ভেতরের পোর্টটিকে সরাসরি আপনার ল্যাপটপের পোর্টের (localhost:8080) সাথে টানেলিং বা ফরোয়ার্ড করে দেয়। --address 0.0.0.0 দেওয়ার কারণে ল্যাপটপের উবুন্টু ভিএম থেকেও এটি অ্যাক্সেস করা যায়।
	• কখন দিবেন: টেস্ট করার সময় বা এক্সটারনাল আইপি কাজ না করলে সাময়িকভাবে ব্রাউজারে অ্যাপটি লোড করার জন্য।
Step-07: Clean-Up (সবকিছু মুছে ফেলা)
• kubectl delete -f kube-manifests/
	• কেন ব্যবহার করবেন: আপনার ডেপ্লয় করা অ্যাপ্লিকেশন, সার্ভিস এবং পডগুলো ক্লাস্টার থেকে সম্পূর্ণভাবে মুছে ফেলার জন্য।
	• কখন দিবেন: আপনার প্র্যাকটিস বা টেস্টিং শেষ হয়ে গেলে, যাতে অপ্রয়োজনীয় পড চালু থেকে আপনার Azure-এর ফ্রি ক্রেডিট বা টাকা কেটে না নেয়।
