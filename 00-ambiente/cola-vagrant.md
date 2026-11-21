# Cola de comandos do Vagrant

Todos os comandos são rodados no PowerShell, **dentro da pasta onde está o `Vagrantfile`**:

```powershell
cd <pasta-do-projeto>\labs-infra
```

Sem o nome da VM, o comando vale para **todas** (node1, node2 e node3).
Com o nome, vale só para ela. Exemplo: `vagrant halt node3`.

## Ligar, desligar e ver o estado

| Comando | O que faz |
| --- | --- |
| `vagrant status` | Mostra se cada VM está ligada (`running`), desligada (`poweroff`) ou ainda não existe (`not created`). |
| `vagrant up` | Cria as VMs que não existem e liga as que estão desligadas. Na primeira vez, baixa a imagem e roda o provisionamento. |
| `vagrant up node2` | Mesma coisa, só para o node2. |
| `vagrant halt` | Desliga as VMs de forma limpa, como um `shutdown`. Use no fim do dia. |
| `vagrant halt node3` | Desliga só o node3 (ex.: no lab 2, para simular a queda de um nó). |
| `vagrant reload node3` | Desliga e liga de novo, **relendo o `Vagrantfile`**. Use depois de mudar memória, CPU, rede ou disco. |
| `vagrant suspend` | Pausa as VMs e guarda a memória em disco. Volta mais rápido, mas ocupa espaço. |
| `vagrant resume` | Retoma as VMs pausadas com `suspend`. |

## Acessar as VMs

| Comando | O que faz |
| --- | --- |
| `vagrant ssh node1` | Entra no terminal do node1 com o usuário `vagrant`. Para virar root: `sudo -i`. Para sair: `exit`. |
| `vagrant ssh node1 -c "hostname -I"` | Roda um comando no node1 sem entrar nele e mostra o resultado. |
| `vagrant ssh-config node1` | Mostra porta, usuário e caminho da chave SSH. Serve para acessar pelo MobaXterm ou pelo WSL. |

## Configurar e recriar

| Comando | O que faz |
| --- | --- |
| `vagrant provision` | Roda de novo os blocos `provision` do `Vagrantfile` nas VMs já ligadas, sem recriar. |
| `vagrant reload --provision` | Reinicia e roda o provisionamento em seguida. |
| `vagrant destroy -f` | **Apaga** todas as VMs e os discos delas. O `-f` não pede confirmação. A pasta e o `Vagrantfile` continuam. |
| `vagrant destroy -f node2` | Apaga só o node2. Depois, `vagrant up node2` recria ele limpo. |
| `vagrant validate` | Confere se o `Vagrantfile` está escrito corretamente, sem criar nada. Rode depois de editar. |

## Imagens (boxes)

| Comando | O que faz |
| --- | --- |
| `vagrant box list` | Lista as imagens baixadas no computador (ex.: `bento/rockylinux-9`). |
| `vagrant box outdated` | Avisa se a imagem usada tem versão mais nova. |
| `vagrant box update` | Baixa a versão mais nova. Só vale para VMs criadas depois disso. |
| `vagrant box prune` | Apaga versões antigas das imagens para liberar espaço em disco. |

## Snapshots (ponto de restauração)

Útil antes de um teste arriscado: se quebrar demais, você volta em segundos.

| Comando | O que faz |
| --- | --- |
| `vagrant snapshot save node1 antes-gluster` | Cria um ponto de restauração do node1 com o nome `antes-gluster`. |
| `vagrant snapshot list` | Lista os snapshots de cada VM. |
| `vagrant snapshot restore node1 antes-gluster` | Volta o node1 para o estado do snapshot. |
| `vagrant snapshot delete node1 antes-gluster` | Apaga o snapshot. |

## Diagnóstico

| Comando | O que faz |
| --- | --- |
| `vagrant global-status` | Lista todas as VMs do Vagrant no computador, de qualquer pasta, com um ID. |
| `vagrant global-status --prune` | Limpa da lista as VMs que já não existem. |
| `vagrant up --debug` | Liga com log detalhado. Use quando o `up` falhar e você precisar ver onde travou. |
| `vagrant --version` | Mostra a versão instalada do Vagrant. |

## Rotina do dia nos labs

```powershell
vagrant up                      # começar o dia
vagrant status                  # conferir se as 3 estão running
vagrant ssh node1               # trabalhar
vagrant snapshot save node1 ok  # antes de quebrar algo de propósito
vagrant halt                    # fim do dia
```

## Problemas comuns

| Sintoma | O que tentar |
| --- | --- |
| `up` trava em "Waiting for machine to boot" | Virtualização (VT-x) desligada na BIOS, ou conflito com o Hyper-V/WSL2. Rode `vagrant up --debug` e veja o erro. |
| Mudei a memória no `Vagrantfile` e nada mudou | Faltou o `vagrant reload <vm>`. |
| VM sumiu do VirtualBox mas o Vagrant acha que existe | `vagrant global-status --prune` e depois `vagrant up`. |
| Erro de rede "host-only adapter" | Abra o VirtualBox > Arquivo > Ferramentas > Gerenciador de Rede e confira se existe a rede 192.168.56.1. |
| Quero recomeçar do zero | `vagrant destroy -f` e `vagrant up`. |
