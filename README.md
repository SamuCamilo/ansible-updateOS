# Ansible - Atualização segura de OS (cPanel/DirectAdmin) sem tocar no que o painel gerencia

Este projeto atualiza pacotes do sistema em servidores:
- CentOS 7/8/9 (obs: CentOS Linux 8 está EOL; use com cuidado)
- AlmaLinux 8/9
- CloudLinux 7/8/9

**Objetivo:** aplicar atualizações de OS com **listas de exclusão** para evitar mexer em pacotes tipicamente gerenciados pelo **cPanel** (ex.: `cpanel*`, `ea-*`) ou **DirectAdmin** (`directadmin*`), e **NÃO reiniciar** o servidor automaticamente.

Quando um reboot for necessário, o playbook **não reinicia**: ele registra um aviso em destaque:
`!!! REBOOT NECESSÁRIO !!!`

## Pré-requisitos (máquina de controle)
- Ansible 2.14+ (ideal 2.15+)
- Acesso SSH (chave) aos servidores
- Usuário com sudo (become)

## Como usar (rápido)
1) Edite o inventário em `inventory/production/hosts.yml` com seus hosts.
2) Ajuste exclusões em:
   - `inventory/production/group_vars/cpanel.yml`
   - `inventory/production/group_vars/directadmin.yml`
3) Execute:
```bash
ansible-playbook -i inventory/production/hosts.yml playbooks/update_os.yml
```

## Exemplos úteis
Atualizar só servidores cPanel:
```bash
ansible-playbook playbooks/update_os.yml -l cpanel
```

Atualizar um host específico:
```bash
ansible-playbook playbooks/update_os.yml -l srv01
```

Rodar em modo "check" (simulação):
```bash
ansible-playbook playbooks/update_os.yml --check --diff
```

## Observações importantes
- O playbook tenta **detectar** o painel por caminhos padrão:
  - cPanel: `/usr/local/cpanel`
  - DirectAdmin: `/usr/local/directadmin`
  Se preferir, você pode **forçar** com `panel_type: cpanel|directadmin` em host_vars/group_vars.
- O reboot necessário é detectado usando `needs-restarting -r` (instalando `yum-utils`/`dnf-utils` quando preciso).
- Não faz mudanças persistentes em `yum.conf/dnf.conf`. A exclusão é aplicada **somente na execução** (via parâmetro `exclude` do módulo).

## Estrutura
- `playbooks/update_os.yml` - playbook principal
- `roles/os_update` - role responsável por atualizar e checar reboot
- `inventory/production` - inventário + vars

## Licença
MIT (se quiser, adicione um arquivo LICENSE com o texto padrão).
