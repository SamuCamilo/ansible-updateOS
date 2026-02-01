# Changelog

Este projeto segue, de forma leve, o versionamento semântico (SemVer):
- MAJOR: mudanças incompatíveis
- MINOR: novas features compatíveis
- PATCH: correções compatíveis

## [Unreleased]
### Planejado
- Suporte a sistemas Debian-like (Debian 10/11/12 e Ubuntu LTS):
  - Atualização via `apt`
  - Checagem de reboot necessário (ex.: `/var/run/reboot-required`)
  - Exclusões/holds de pacotes equivalentes ao `exclude` (pin/hold)
- Suporte a outras distros derivadas (ex.: Rocky Linux) com validação explícita
- Melhorias de detecção de painel:
  - Identificação mais robusta por serviços/paths
  - Possibilidade de múltiplos painéis (ou “unknown” com regras seguras)
- Relatório pós-execução:
  - Lista de pacotes atualizados por host
  - Registro em arquivo (ex.: `logs/report-YYYY-MM-DD.md`)
- Modo “security-only” com comportamento consistente por distro
- Ajustes finos por versão:
  - Regras específicas para EL7 vs EL8/9 (ex.: repositórios, dnf/yum, plugins)
- Integração com CI:
  - `ansible-lint` e checagens básicas via GitHub Actions

## [0.1.0] - 2026-02-01
### Adicionado
- Estrutura inicial do projeto Ansible pronta para GitHub:
  - `ansible.cfg`, `.gitignore`, `README.md`, inventário e role
- Inventário em YAML com grupos:
  - `cpanel` e `directadmin`
- Variáveis por grupo:
  - `group_vars/cpanel.yml` com padrões de exclusão do cPanel (ex.: `cpanel*`, `ea-*`)
  - `group_vars/directadmin.yml` com padrões de exclusão do DirectAdmin
  - `group_vars/all.yml` com defaults globais (ex.: `os_update_exclude_extra`)
- Role `os_update`:
  - Atualização de pacotes via `yum` (EL7) e `dnf` (EL8/EL9)
  - Suporte a `exclude` para não atualizar pacotes gerenciados pelo painel
  - Opções: `update_cache`, `skip_broken`, `security_only` (quando suportado)
- Checagem de reboot necessário sem reiniciar:
  - Instala `yum-utils`/`dnf-utils`
  - Executa `needs-restarting -r`
  - Loga aviso em destaque `!!! REBOOT NECESSÁRIO !!!`
- Log central no controlador:
  - `logs/ansible.log`

