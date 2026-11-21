# labs-infra

Laboratórios práticos de infraestrutura Linux, automação, Kubernetes e observabilidade,
montados em 3 VMs Rocky Linux 9 (VirtualBox + Vagrant).

Cada lab segue o roteiro: **montar → validar → quebrar de propósito → recuperar → registrar**.

| Lab | Tema | Tecnologias | Status |
| --- | --- | --- | --- |
| [00](00-ambiente/) | Ambiente | VirtualBox, Vagrant, Rocky Linux 9 | a fazer |
| [01](01-nfs/) | Compartilhamento de arquivos | NFS, autofs, firewalld | a fazer |
| [02](02-glusterfs/) | Armazenamento distribuído | GlusterFS réplica 3, LVM, XFS | a fazer |
| [03](03-ansible/) | Automação e documentação | Ansible roles, ansible-lint | a fazer |
| [04](04-containers/) | Containers a fundo | Docker, multi-stage, cgroups, namespaces | a fazer |
| [05](05-terraform/) | Infraestrutura como código | Terraform, provider Docker | a fazer |
| [06](06-kubernetes-k3s/) | Kubernetes | k3s 3 nós, probes, HPA, Ingress | a fazer |
| [07](07-rancher/) | Gestão de clusters | Rancher | a fazer |
| [08](08-cicd/) | CI/CD | GitHub Actions, GHCR, self-hosted runner | a fazer |
| [09](09-observabilidade/) | Observabilidade e SLO | Prometheus, PromQL, Alertmanager, Grafana | a fazer |
| [10](10-incidente/) | Incidente simulado | Pós-incidente sem culpados | a fazer |

## Ambiente

| VM | IP | Memória |
| --- | --- | --- |
| node1 | 192.168.56.11 | 2 GB |
| node2 | 192.168.56.12 | 2 GB |
| node3 | 192.168.56.13 | 2 GB |

```bash
vagrant up          # cria/liga as VMs
vagrant ssh node1   # acessa uma VM
vagrant halt        # desliga tudo
vagrant destroy -f  # apaga para recomeçar do zero
```
