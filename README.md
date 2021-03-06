# simple-kubernetes-deploy
A group of very simple Ansible playbooks to deploy the most basic Kubernetes Cluster

The main inspiration for these playbooks comes from this [Digital Ocean article](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-20-04)

Ultimately, the desire is to simplify the deployment of a basic Kubernetes cluster as much as possible without getting into any of the nasty details. The Kubernetes community have done fantastic work in the past few years to simplify kubeadm and other tools so if you want to pay them back, please consider becoming a [K8s contributor](https://github.com/kubernetes/community/tree/master/contributors/guide)

## Getting Started

First make sure you align with the requirements set out below, then run the Ansible playbooks and finally test if the deployment was successful.

### Requirements

There are three requirements to get started, make sure:
1. You have 4 nodes (Check out the [Tested On Section](#tested-on) to see tested OS')
    - 1 Node for Ansible Controller
    - 1 Node for Control Plane
    - 2 Nodes for Worker
2. You have Ansible installed on the Ansible Controller
    **Ubuntu 20.04**:
    ```bash
    sudo apt install ansible
    ```
3. Your Ansible Controller has root ssh key access to the Control Plane and Worker Nodes
    **Ubuntu 20.04**:
    Create the SSH Pub/Priv Keys:
    ```bash
    # On Ansible Controller:
    ssh-keygen -t ed25519
    ```
    Upload the ssh public key to your Github account. Then import them as root on your other nodes:
    ```bash
    # On Control Plane and Worker Nodes:
    sudo su # Enter user password at prompt
    
    ssh-import-id-gh <github-username>
    ```

### hosts file

Add details about all the nodes you will deploy K8s on in the `hosts` file

### Run the Ansible Playbook

To deploy the Playbook, simply run the following command:
```bash
ansible-playbook -i hosts k8s-deploy.yml
```

Assuming no errors came up, congratulations, you've deployed a Kubernetes Cluster!! :tada: :tada:

## Verify Cluster

To make sure the Kubernetes cluster deployment was a success, log into the Control Plane node with the `ubuntu` username:
```bash
ssh ubuntu@control_plane_ip
```
Then check if all the nodes are connected to the K8s cluster:
```bash
kubectl get nodes
```
If the following prints, you're in the clear and ready to deploy workloads:
```
control1   Ready    control-plane,master   74m   v1.22.4
worker1    Ready    <none>                 72m   v1.22.4
worker2    Ready    <none>                 72m   v1.22.4
```

## Contributing

If you like to add or change some of the code in this repo feel free to open a pull request and I will take look as soon as possible.

## Tested On

| Operating System | Virtual Machine | Bare Metal | CPU Architecture | Kubernetes Version |
|------------------|-----------------|------------|------------------|--------------------|
| Ubuntu 20.04     | Yes             | No         | x86              | 1.22               |
| Ubuntu 22.04     | Yes             | No         | x86              | 1.24


## To-Do

- Test on Raspberry Pi
- Test on RHEL Distro
- Add Group Vars configuration file
- Test on Ubuntu Bare Metal

## License

MIT
