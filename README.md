# Terraform K0S Hetzner Installer

## :dart: About ##

Terraform project for generating a ready to go secured based cloud infrastructure through Hetzner Cloud provider, with a ready-to-install [K0S](https://k0sproject.io/), a zero-friction Kubernetes distribution. The cluster will be composed of 4 servers :

1. A main control pane server
2. 2 worker nodes
3. 1 data node for DB specific tasks

Feel free to add some additional nodes inside `servers.tf` file.

## :white_check_mark: Requirements ##

Before starting :checkered_flag:, you need to have a Hetzner cloud account as well as the `terraform` client. On Windows, this is a simple `scoop install terraform`.

Before continue, **DO NOT** reuse any existing project as we'll use terraform ! Create a new empty hcloud empty project with a valid Read/Write API token key.

## :checkered_flag: Starting ##

```bash
# Clone this project
$ git clone https://github.com/adr1enbe4udou1n/terraform-hetzner-kube-sample

# Prepare variables, cf bellow for list
cp terraform.tfvars.example terraform.tfvars

# Install
terraform apply
```

## Variables reference

| Name                       | Purpose                                                                                                                                                                                                |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| hcloud_token               | The token to access the Hetzner Cloud API (must have write access)                                                                                                                                     |
| cluster_name               | Used for server names prefix, ${cluster_name}-controller-01, ${cluster_name}-worker-01, etc.                                                                                                           |
| cluster_user               | The default non root user for ssh connection                                                                                                                                                           |
| cluster_fqdn               | User for main cluster FQDN access through Kubernetes API endpoint. This domain must be created on your own registrar and point towards the main controller IP                                          |
| server_location            | At Nuremberg by default                                                                                                                                                                                |
| volume_size                | The size of volume that will be mounted to data node, 10Gb by default                                                                                                                                  |
| my_public_ssh_name         | Name of default Hetzner ssh key                                                                                                                                                                        |
| my_public_ssh_key          | Your public SSH key for remote access to your nodes                                                                                                                                                    |
| my_ip_addresses            | IP addresses that will be whitelisted for SSH and Kubernetes API connection through Hetzner firewall. IP format must have full format with proper mask, e.i. x.x.x.x/32. Leave default for free access |
| controller_private_ssh_key | The private key of main controller server, required for k0sctl install                                                                                                                                 |
| controller_public_ssh_key  | The public key of main controller server                                                                                                                                                               |

Use the command `ssh-keygen -t ed25519 -f "cluster-key" -qN ""` for quick generation and put both content of private `cluster-key` and public `cluster-key.pub` keys into above respective `controller_private_ssh_key` and `controller_public_ssh_key` variables.

You can legitimately think that the private SSH key through TF variable is unsecure, and you'll be right. But this is only intended for internal controller-to-worker access through private network, and cannot be used outside. All internal servers behind the main control pane node will be entirely blocked by the Hetzner firewall. If we need access to this internal nodes, we'll simply use the Jump SSH feature, and use controller node as bastion.

## Usage

TODO SSH Config
TODO Kube Cluster install

## :memo: License ##

This project is under license from MIT. For more details, see the [LICENSE](https://adr1enbe4udou1n.mit-license.org/) file.

Made with :heart: by <a href="https://github.com/adr1enbe4udou1n" target="_blank">Adrien Beaudouin</a>
