# Lab Exercises for Cluster Architecture, Installation and Configuration

# Exercise 0 - Setup

* [ ] Prepare a cluster (Single node, kubeadm, k3s, etc)
  * Commands Run:
    ```bash
    ❯ k3d --help
    https://k3d.io/
    k3d is a wrapper CLI that helps you to easily create k3s clusters inside docker.
    Nodes of a k3d cluster are docker containers running a k3s image.
    All Nodes of a k3d cluster are part of the same docker network.
    
    Usage:
    k3d [flags]
    k3d [command]

	Available Commands:
	  cluster      Manage cluster(s)
	  completion   Generate completion scripts for [bash, zsh, fish, powershell | psh]
	  config       Work with config file(s)
	  help         Help about any command
	  image        Handle container images.
	  kubeconfig   Manage kubeconfig(s)
	  node         Manage node(s)
	  registry     Manage registry/registries
	  version      Show k3d and default k3s version

	Flags:
	  -h, --help         help for k3d
	      --timestamps   Enable Log timestamps
	      --trace        Enable super verbose output (trace logging)
	      --verbose      Enable verbose output (debug logging)
	      --version      Show k3d and default k3s version

	Use "k3d [command] --help" for more information about a command.
	❯ k3d
	Usage:
	  k3d [flags]
	  k3d [command]

	Available Commands:
	  cluster      Manage cluster(s)
	  completion   Generate completion scripts for [bash, zsh, fish, powershell | psh]
	  config       Work with config file(s)
	  help         Help about any command
	  image        Handle container images.
	  kubeconfig   Manage kubeconfig(s)
	  node         Manage node(s)
	  registry     Manage registry/registries
	  version      Show k3d and default k3s version

	Flags:
	  -h, --help         help for k3d
	      --timestamps   Enable Log timestamps
	      --trace        Enable super verbose output (trace logging)
	      --verbose      Enable verbose output (debug logging)
	      --version      Show k3d and default k3s version

	Use "k3d [command] --help" for more information about a command.
	❯ k3d cluster --help
	Manage cluster(s)

	Usage:
	  k3d cluster [flags]
	  k3d cluster [command]

	Available Commands:
	  create      Create a new cluster
	  delete      Delete cluster(s).
	  edit        [EXPERIMENTAL] Edit cluster(s).
	  list        List cluster(s)
	  start       Start existing k3d cluster(s)
	  stop        Stop existing k3d cluster(s)

	Flags:
	  -h, --help   help for cluster

	Global Flags:
	      --timestamps   Enable Log timestamps
	      --trace        Enable super verbose output (trace logging)
	      --verbose      Enable verbose output (debug logging)

	Use "k3d cluster [command] --help" for more information about a command.
	❯ k3d cluster create --help

	Create a new k3s cluster with containerized nodes (k3s in docker).
	Every cluster will consist of one or more containers:
		- 1 (or more) server node container (k3s)
		- (optionally) 1 loadbalancer container as the entrypoint to the cluster (nginx)
		- (optionally) 1 (or more) agent node containers (k3s)

	Usage:
	  k3d cluster create NAME [flags]

	Flags:
	  -a, --agents int                                                     Specify how many agents you want to create
	      --agents-memory string                                           Memory limit imposed on the agents nodes [From docker]
	      --api-port [HOST:]HOSTPORT                                       Specify the Kubernetes API server port exposed on the LoadBalancer (Format: [HOST:]HOSTPORT)
										- Example: `k3d cluster create --servers 3 --api-port 0.0.0.0:6550`
	  -c, --config string                                                  Path of a config file to use
	  -e, --env KEY[=VALUE][@NODEFILTER[;NODEFILTER...]]                   Add environment variables to nodes (Format: KEY[=VALUE][@NODEFILTER[;NODEFILTER...]]
										- Example: `k3d cluster create --agents 2 -e "HTTP_PROXY=my.proxy.com@server:0" -e "SOME_KEY=SOME_VAL@server:0"`
	      --gpus string                                                    GPU devices to add to the cluster node containers ('all' to pass all GPUs) [From docker]
	  -h, --help                                                           help for create
	      --host-alias ip:host[,host,...]                                  Add ip:host[,host,...] mappings
	      --host-pid-mode                                                  Enable host pid mode of server(s) and agent(s)
	  -i, --image string                                                   Specify k3s image that you want to use for the nodes
	      --k3s-arg ARG@NODEFILTER[;@NODEFILTER]                           Additional args passed to k3s command (Format: ARG@NODEFILTER[;@NODEFILTER])
										- Example: `k3d cluster create --k3s-arg "--disable=traefik@server:0"
	      --k3s-node-label KEY[=VALUE][@NODEFILTER[;NODEFILTER...]]        Add label to k3s node (Format: KEY[=VALUE][@NODEFILTER[;NODEFILTER...]]
										- Example: `k3d cluster create --agents 2 --k3s-node-label "my.label@agent:0,1" --k3s-node-label "other.label=somevalue@server:0"`
	      --kubeconfig-switch-context                                      Directly switch the default kubeconfig's current-context to the new cluster's context (requires --kubeconfig-update-default) (default true)
	      --kubeconfig-update-default                                      Directly update the default kubeconfig with the new cluster's context (default true)
	      --lb-config-override strings                                     Use dotted YAML path syntax to override nginx loadbalancer settings
	      --network string                                                 Join an existing network
	      --no-image-volume                                                Disable the creation of a volume for importing images
	      --no-lb                                                          Disable the creation of a LoadBalancer in front of the server nodes
	      --no-rollback                                                    Disable the automatic rollback actions, if anything goes wrong
	  -p, --port [HOST:][HOSTPORT:]CONTAINERPORT[/PROTOCOL][@NODEFILTER]   Map ports from the node containers (via the serverlb) to the host (Format: [HOST:][HOSTPORT:]CONTAINERPORT[/PROTOCOL][@NODEFILTER])
										- Example: `k3d cluster create --agents 2 -p 8080:80@agent:0 -p 8081@agent:1`
	      --registry-config string                                         Specify path to an extra registries.yaml file
	      --registry-create NAME[:HOST][:HOSTPORT]                         Create a k3d-managed registry and connect it to the cluster (Format: NAME[:HOST][:HOSTPORT]
										- Example: `k3d cluster create --registry-create mycluster-registry:0.0.0.0:5432`
	      --registry-use stringArray                                       Connect to one or more k3d-managed registries running locally
	      --runtime-label KEY[=VALUE][@NODEFILTER[;NODEFILTER...]]         Add label to container runtime (Format: KEY[=VALUE][@NODEFILTER[;NODEFILTER...]]
										- Example: `k3d cluster create --agents 2 --runtime-label "my.label@agent:0,1" --runtime-label "other.label=somevalue@server:0"`
	  -s, --servers int                                                    Specify how many servers you want to create
	      --servers-memory string                                          Memory limit imposed on the server nodes [From docker]
	      --subnet 172.28.0.0/16                                           [Experimental: IPAM] Define a subnet for the newly created container network (Example: 172.28.0.0/16)
	      --timeout duration                                               Rollback changes if cluster couldn't be created in specified duration.
	      --token string                                                   Specify a cluster token. By default, we generate one.
	  -v, --volume [SOURCE:]DEST[@NODEFILTER[;NODEFILTER...]]              Mount volumes into the nodes (Format: [SOURCE:]DEST[@NODEFILTER[;NODEFILTER...]]
										- Example: `k3d cluster create --agents 2 -v /my/path@agent:0,1 -v /tmp/test:/tmp/other@server:0`
	      --wait                                                           Wait for the server(s) to be ready before returning. Use '--timeout DURATION' to not wait forever. (default true)

	Global Flags:
	      --timestamps   Enable Log timestamps
	      --trace        Enable super verbose output (trace logging)
	      --verbose      Enable verbose output (debug logging)
    ```
  * Actual Command Run:
    ```bash
    ❯ k3d cluster create achandrashekaran-test -a 3 --agents-memory 2G
	INFO[0000] Prep: Network
	INFO[0000] Created network 'k3d-achandrashekaran-test'
	INFO[0000] Created image volume k3d-achandrashekaran-test-images
	INFO[0000] Starting new tools node...
	INFO[0002] Creating node 'k3d-achandrashekaran-test-server-0'
	INFO[0003] Pulling image 'ghcr.io/k3d-io/k3d-tools:5.4.6'
	INFO[0003] Pulling image 'docker.io/rancher/k3s:v1.24.4-k3s1'
	INFO[0009] Starting Node 'k3d-achandrashekaran-test-tools'
	INFO[0012] Creating node 'k3d-achandrashekaran-test-agent-0'
	INFO[0013] Creating node 'k3d-achandrashekaran-test-agent-1'
	INFO[0013] Creating node 'k3d-achandrashekaran-test-agent-2'
	INFO[0014] Creating LoadBalancer 'k3d-achandrashekaran-test-serverlb'
	INFO[0015] Pulling image 'ghcr.io/k3d-io/k3d-proxy:5.4.6'
	INFO[0018] Using the k3d-tools node to gather environment information
	INFO[0018] Starting new tools node...
	INFO[0018] Starting Node 'k3d-achandrashekaran-test-tools'
	INFO[0020] Starting cluster 'achandrashekaran-test'
	INFO[0020] Starting servers...
	INFO[0020] Starting Node 'k3d-achandrashekaran-test-server-0'
	INFO[0027] Starting agents...
	INFO[0028] Starting Node 'k3d-achandrashekaran-test-agent-2'
	INFO[0028] Starting Node 'k3d-achandrashekaran-test-agent-1'
	INFO[0028] Starting Node 'k3d-achandrashekaran-test-agent-0'
	INFO[0033] Starting helpers...
	INFO[0033] Starting Node 'k3d-achandrashekaran-test-serverlb'
	INFO[0040] Injecting records for hostAliases (incl. host.k3d.internal) and for 6 network members into CoreDNS configmap...
	INFO[0043] Cluster 'achandrashekaran-test' created successfully!
	INFO[0043] You can now use it like this:
	kubectl cluster-info
    ```
    Cluster Info:
    ```
    ❯ kubectl cluster-info
    Kubernetes master is running at https://0.0.0.0:59473
    CoreDNS is running at https://0.0.0.0:59473/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
    Metrics-server is running at https://0.0.0.0:59473/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy
    ```
    
    

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
* Prepare two vanilla VM's (No Kubernetes components installed) with the kubeadm binary installed (feel free to do this beforehand, I doubt it will be a requirement to add apt sources to get it)
* Open browser tabs to https://kubernetes.io/docs/, https://github.com/kubernetes/ and  https://kubernetes.io/blog/ (these are permitted as per [the current guidelines](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed#certified-kubernetes-administrator-cka-and-certified-kubernetes-application-developer-ckad))

# Exercise 1 - RBAC

A third party application requires access to describe `job` objects that reside in a namespace called `rbac`. Perform the following:

1. Create a namespace called `rbac`
2. Create a service account called `job-inspector` for the `rbac` namespace
3. Create a role that has rules to `get` and `list` job objects
4. Create a rolebinding that binds the service account `job-inspector` to the role created in step 3
5. Prove the `job-inspector` service account can "get" `job` objects but not `deployment` objects

<details><summary>Answer - Imperative</summary>

```shell
kubectl create namespace rbac
kubectl create sa job-inspector -n rbac
kubectl create role job-inspector --verb=get --verb=list --resource=jobs -n rbac
kubectl create rolebinding permit-job-inspector --role=job-inspector --serviceaccount=rbac:job-inspector -n rbac
kubectl --as=system:serviceaccount:rbac:job-inspector auth can-i get job -n rbac 
kubectl --as=system:serviceaccount:rbac:job-inspector auth can-i get deployment -n rbac
```
</details>


<details><summary>Answer - Declarative</summary>

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: rbac
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: job-inspector
  namespace: rbac
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: job-inspector
  namespace: rbac
rules:
  - apiGroups: ["batch"]
    resources: ["jobs"]
    verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: permit-job-inspector
  namespace: rbac
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: job-inspector
subjects:
  - kind: ServiceAccount
    name: job-inspector
    namespace: rbac
```
</details>

# Exercise 2 - Use Kubeadm to install a basic cluster

1. On a node, install kubeadm and stand up the control plane, using `10.244.0.0/16` as the pod network CIDR, and https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml as the CNI
2. On a node, install kubeadm and join it to the cluster as a worker node

<details><summary>Answer</summary>

## Node 1:

Prep kubeadm (as mentioned above, I doubt we will need to do this part in the exam)

```shell
# Install a container runtime, IE https://github.com/containerd/containerd/blob/main/docs/getting-started.md
apt-get update && apt-get install -y apt-transport-https curl
curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```

Turn this node into a master

```shell
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
...
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
...
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
...
Note the join command, ie:
kubeadm join 172.16.10.210:6443 --token 9tjntl.10plpxqy85g8a0ui \
    --discovery-token-ca-cert-hash sha256:381165c9a9f19a123bd0fee36fe36d15e918062dcc94711ff5b286ee1f86b92b 
```
## Node 2

Run the join command taken from the previous step

```shell
kubeadm join 172.16.10.210:6443 --token 9tjntl.10plpxqy85g8a0ui \
    --discovery-token-ca-cert-hash sha256:381165c9a9f19a123bd0fee36fe36d15e918062dcc94711ff5b286ee1f86b92b 
```

Validate by running `kubectl get no` on the master node:

```shell
kubectl get no
NAME      STATUS   ROLES                  AGE     VERSION
ubuntu    Ready    control-plane,master   9m53s   v1.26.0
ubuntu2   Ready    <none>                 50s     v1.26.0
```
</details>

# Exercise 3 - Manage a highly-available Kubernetes cluster

1. Using `etcdctl`, determine the health of the etcd cluster
2. Using `etcdctl`, identify the list of members   
3. On the master node, determine the health of the cluster by probing the API endpoint


<details><summary>Answer</summary>

```shell
etcdctl cluster-health

cluster is healthy
member <id> is healthy
member <id> is healthy
member <id> is healthy

etcdctl member list
<id>: name=etcd1 peerURLs=http://<ip>:2380 clientURLs=<ip>:2379
<id>: name=etcd0 peerURLs=http://<ip>:2380 clientURLs=<ip>:2379
<id>: name=etcd2 peerURLs=http://<ip>:2380 clientURLs=<ip>:2379

curl -k https://localhost:6443/healthz?verbose
[+]ping ok
[+]log ok
[+]etcd ok
[+]poststarthook/start-kube-apiserver-admission-initializer ok
...
```

</details>

#  Exercise 4 - Perform a version upgrade on a Kubernetes cluster using Kubeadm 

1. Using `kubeadm`, upgrade a cluster to the lastest version

<details><summary>Answer</summary>

If held, unhold the kubeadm version

```shell
sudo apt-mark unhold kubeadm
```

Upgrade the `kubeadm` version:

```shell
sudo apt-get install --only-upgrade kubeadm
```

`plan` the upgrade:

```shell
sudo kubeadm upgrade plan

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT       AVAILABLE
kubelet     1 x v1.19.0   v1.20.2

Upgrade to the latest stable version:

COMPONENT                 CURRENT   AVAILABLE
kube-apiserver            v1.19.7   v1.20.2
kube-controller-manager   v1.19.7   v1.20.2
kube-scheduler            v1.19.7   v1.20.2
kube-proxy                v1.19.7   v1.20.2
CoreDNS                   1.7.0     1.7.0
etcd                      3.4.9-1   3.4.13-0
```

Upgrade the cluster

```shell
kubeadm upgrade apply v1.20.2
```

Upgrade Kubelet:

```shell
sudo apt-get install --only-upgrade kubelet
```

</details>

# Exercise 5 - Implement etcd backup and restore

1. Take a backup of etcd
2. Verify the etcd backup has been successful
3. Restore the backup back to the cluster

<details><summary>Answer</summary>

Take a snapshot of etcd:

```shell
ETCDCTL_API=3 etcdctl snapshot save snapshot.db --cacert /etc/kubernetes/pki/etcd/server.crt --cert /etc/kubernetes/pki/etcd/ca.crt --key /etc/kubernetes/pki/etcd/ca.key
```

Verify the snapshot:

```shell
sudo ETCDCTL_API=3 etcdctl --write-out=table snapshot status snapshot.db
```

Perform a restore:

```shell
ETCDCTL_API=3 etcdctl snapshot restore snapshot.db
```

</details>
