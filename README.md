

Set Hostname

Edit the hostname file:
Run the following command to open the hostname file in a text editor (replace "new-hostname" with your desired hostname):
 
Update hosts file:
 
Run the following command to apply the changes immediately without reboot 
 
Set Hostname

Master Node:
sudo hostnamectl set-hostname master.example.com
 Worker1 Node:
sudo hostnamectl set-hostname worker-node-1.example.com

 Worker2 Node:
sudo hostnamectl set-hostname worker-node-2.example.com

 

Docker Configuration - Master, Worker1, Worker2

Step 1.     Create a Docker Configuration File:
                  sudo mkdir /etc/docker
This command creates a directory /etc/docker if it doesn't exist. The directory is used to store Docker configuration files.

 
 
sudo mkdir /etc/docker

cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
	"max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
—------------------------------------------------------------
sudo systemctl enable docker
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo swapoff -a

Do the above steps in Master, Worker1 and Worker2 nodes





  This command uses a heredoc (<<EOF) to create a Docker daemon configuration file (/etc/docker/daemon.json). The configuration includes options for the execution, logging, and storage drivers.
Explanation of some key options:
exec-opts: Sets the execution options, specifying the cgroup driver for systemd.
log-driver and log-opts: Define logging options, setting the maximum log file size to 100 megabytes.
storage-driver: Sets the storage driver to overlay2.


Creating a Docker Configuration File:
Creating a configuration file for Docker (/etc/docker/daemon.json) allows you to customize Docker daemon settings. The provided configuration includes options related to the execution environment, logging, and storage driver. These configurations can help optimize Docker for your specific use case.
Enabling Docker Service on Boot:
sudo systemctl enable docker
 ensures that the Docker service starts automatically when the system boots. This is generally a good practice to ensure Docker is available after a system reboot.
Reloading Systemd Daemon:
sudo systemctl daemon-reload 
is used to reload the systemd manager configuration. This is necessary when changes are made to systemd unit files, ensuring that systemd recognizes the updated configurations.
Restarting Docker Service:
sudo systemctl restart docker
 is used to restart the Docker service after applying changes to the Docker daemon configuration. This step is required for the new configuration to take effect.


Master Node initialisation
 sudo kubeadm init 
command is used to initialize a Kubernetes control-plane node (master node). This command is typically run on a machine that you intend to set up as the master node in a Kubernetes cluster. 
 

 
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
  cat ~/.kube/config

    Install Container Network Interface (CNI) 
  kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

 

Verification:

 
Basic commands of Kubernetes
1.kubectl get nodes
   Purpose: This command is used to retrieve information about the nodes in your Kubernetes  cluster  
     - `kubectl`: The command-line tool for interacting with Kubernetes clusters.
     - `get`: The action to retrieve information.
     - `nodes`: The resource type you want to retrieve information about (in this case, nodes).

2.kubectl describe node worker-1:
   Purpose: This command provides detailed information about a specific node in the Kubernetes cluster, in this case, a node named `worker-1`
     - `describe`: The action to provide detailed information.
     - `node worker-1`: Specifies the node named `worker-1`.
   
3. kubectl get pods:
   -Purpose: This command is used to retrieve information about the pods running in the cluster.
     - `pods`: The resource type you want to retrieve information about (in this case, pods)    

4. kubectl describe pod <pod-name>:
   Purpose: This command provides detailed information about a specific pod in the Kubernetes cluster. You replace `<pod-name>` with the actual name of the pod you want information about.
     - `pod <pod-name>`: Specifies the pod for which you want detailed information.
   
1. kubectl get all
   Purpose: This command is used to retrieve information about all resources in the default namespace.
        - `all`: The resource type specified. It fetches information about all resource types (services, pods, deployments, etc.) in the default namespace.

   2. kubectl get all -n kube-system:
   -Purpose: This command is similar to the previous one but fetches information about all resources in the `kube-system` namespace.
        - `-n kube-system`: Specifies the namespace (`kube-system`) to fetch information from.
3. kubectl get ns:
   - Purpose: This command is used to list all namespaces in the Kubernetes cluster.
        - `ns`: The resource type for namespaces.     ```

4. kubectl describe ns kube-system:
   - Purpose: This command provides detailed information about the `kube-system` namespace.
        - `ns kube-system`: Specifies the namespace (`kube-system`) to describe.
1.*kubectl get events
   - purpose: This command is used to display the events in the cluster. Events provide insight into the state changes and activities within the cluster.
        - `events`: The resource type for events.
2. kubectl delete pod <pod-name>:
   - Purpose: This command is used to delete a specific pod in the cluster. Replace `<pod-name>` with the actual name of the pod you want to delete.
   
3. kubectl api-resources:
   - **Purpose**: This command lists all API resource types supported by the Kubernetes cluster.
     - `api-resources`: The action to list available API resource types.
   
4. kubectl explain pod:
   - **Purpose**: This command provides information about the structure of a pod resource.
    - `explain`: The action to display documentation about a resource or field.
     - `pod`: Specifies the resource type to explain.
6.kubectl run nginxpod --image=nginx --port 80
   -*Purpose: This command is used to create a new pod named `nginxpod` with the Nginx container.
   - - `run`: The action to create and run a resource.
     - `nginxpod`: The name of the pod to be created.
     - `--image=nginx`: Specifies the container image (Nginx in this case).
1. kubectl cluster-info:
   - **Purpose**: This command is used to display information about the Kubernetes cluster.
        - `cluster-info`: The action to display information about the cluster.

2. *kubectl cluster-info dump > cluster-dump:
   -Purpose: This command generates a detailed dump of cluster information and saves it to a file named `cluster-dump`.
        - `kubectl`: The command-line tool for interacting with Kubernetes clusters.
     - `cluster-info dump`: The action to generate a detailed dump of cluster information.
     - `> cluster-dump`: Redirects the output to a file named `cluster-dump`.

4. **kubectl describe node worker01 | less*:
   - **Purpose**: This command provides detailed information about the `worker01` node, allowing you to scroll through the information using the `less` pager.
   - **Details**:
     - `kubectl`: The command-line tool for interacting with Kubernetes clusters.
     - `describe`: The action to provide detailed information.
     - `node worker01`: Specifies the resource type (`node`) and the specific node name.
     - `| less`: Pipes the output to the `less` pager for better readability.
  
6. **kubectl get pods -A**:
   - **Purpose**: This command retrieves information about pods across all namespaces.
   - **Details**:
     - `kubectl`: The command-line tool for interacting with Kubernetes clusters.
     - `get`: The action to retrieve information.
     - `pods -A`: Specifies the resource type (`pods`) and the `-A` flag indicates across all namespaces.










