# Projeto 2 – Blue Team: Implantação de EDR/HIDS com Wazuh nativo no Ubuntu
Implantando um EDR/HIDS Completo com Wazuh nativo no Ubuntu (sem VMs e com baixo consumo)

## Objetivo
Implantar e configurar um ambiente completo de monitoramento de segurança (EDR/HIDS) utilizando o Wazuh em modo **all-in-one** em um notebook com recursos limitados (2 núcleos, 16 GB RAM), sem uso de máquinas virtuais.

## Requisitos atendidos
- Wazuh Manager + Indexer + Dashboard (all-in-one)
- Wazuh Agent monitorando o próprio host (agent 000)
- File Integrity Monitoring (FIM) em tempo real
- Detecção de rootkits e arquivos SUID/SGID
- Monitoramento de comandos suspeitos
- Geração de alertas em tempo real no dashboard web
- Baixo consumo de recursos (≤ 1.2 GB RAM em idle)

## Estrutura do projeto
/ciberseg/projeto2/
├── README.md ← este arquivo
├── wazuh-install-files/ ← gerado automaticamente
├── screenshots/ ← capturas de tela
├── relatorio-projeto2.tex ← relatório LaTeX
├── relatorio-projeto2.pdf ← relatório compilado
  └── config/
  └── 010-meu-fim-comandos.conf ← configuração customizada

## Evidências geradas
- Alerta de integridade de arquivos (/etc)
- Alerta de criação de arquivo SUID
- Monitoramento de comandos who/w/last/ps
- Dashboard com agente 000 ativo

Autor: Bruno Borges Fagundes
Pós-Graduação em Cibersegurança – Metropolitana
Data: 2025

## Como reproduzir (passo a passo completo)
```bash
# 1. Instalação all-in-one
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a

# 2. Configuração customizada de FIM e comandos
sudo mkdir -p /var/ossec/etc/conf.d
sudo cp config/010-meu-fim-comandos.conf /var/ossec/etc/conf.d/

# 3. Reiniciar serviços
sudo systemctl restart wazuh-manager wazuh-agent

# 4. Simulação de incidentes
sudo touch /etc/arquivo-malicioso.conf
sudo chmod 4755 /tmp/backdoor
who; last; w 
