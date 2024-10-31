# Configuração do Node Exporter para Coleta de Métricas no Prometheus

Este documento descreve como instalar e configurar o Node Exporter em um servidor para que as métricas possam ser coletadas pelo Prometheus.

## Passo 1: Instalar o Node Exporter

1. **Baixe a versão mais recente do Node Exporter:**
   wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
   (pode ser encontrado aqui: https://prometheus.io/download/#node_exporter)
   
2. **Estraia o arquivo baixado:**
   tar xvfz node_exporter-1.8.2.linux-amd64.tar.gz

3. **Mover para um diretório de uso geral:**
   mv node_exporter-1.8.2.linux-amd64/node_exporter /usr/local/bin/

## Passo 2: Configurar o node_exporter como um serviço

1. **Crie um arquivo de serviço para o Node Exporter:**
   nano /etc/systemd/system/node_exporter.service

2. **Adiciona as configurações ao arquivo:**
   [Unit]
   Description=Node Exporter
   Wants=network-online.target
   After=network-online.target

   [Service]
   User=node_exporter #Verifique o usuário desejado
   ExecStart=/usr/local/bin/node_exporter

   [Install]
   WantedBy=default.target

3. **Crie o usuário e ajuste as permissões:**
   useradd -rs /bin/false node_exporter

4. **Inicie o serviço, habilite-o para iniciar automaticamente e verifique o status:**
   sudo systemctl daemon-reload
   sudo systemctl start node_exporter
   sudo systemctl enable node_exporter
   sudo systemctl status node_exporter

## Passo 3: Adicione as configurações no servidor do prometheus

1. **No arquivo prometheus.yml, adicione a configuração do seu job na sessão scrape_configs:**
   - job_name: "node_exporter"
      static_configs:
        - targets: ["IP_DO_SERVIDOR:9100"]
          
2. Reinicie o serviço
   sudo systemctl restart prometheus



Esse arquivo `README.md` fornece todas as informações necessárias para configurar o Node Exporter em um servidor e integrá-lo ao Prometheus.

