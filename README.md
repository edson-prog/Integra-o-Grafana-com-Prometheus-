# Monitoramento-Grafana-com-Prometheus
Prometheus
Este guia prático cobre a instalação, configuração e uso do Prometheus, Grafana e Node Exporter para monitorar servidores de forma eficiente.

1. Instalação do Prometheus e Grafana
Prometheus
bash
Copiar código
# Baixar e extrair o Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v*/prometheus-*.*-linux-amd64.tar.gz
tar xvfz prometheus-*.*-linux-amd64.tar.gz
cd prometheus-*.*-linux-amd64
 Verifique a ultima atualização so Prometheus e do Node_exporter
# Iniciar o Prometheus
./prometheus --config.file=prometheus.yml
Grafana
bash
Copiar código
# Baixar e instalar o Grafana
wget https://dl.grafana.com/oss/release/grafana_*_amd64.deb
sudo apt-get install -y adduser libfontconfig1
sudo dpkg -i grafana_*_amd64.deb

# Iniciar o Grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
2. Configuração do Node Exporter
Instalar o Node Exporter no servidor:

bash
Copiar código
wget https://github.com/prometheus/node_exporter/releases/download/v*/node_exporter-*.*-linux-amd64.tar.gz
tar xvfz node_exporter-*.*-linux-amd64.tar.gz
cd node_exporter-*.*-linux-amd64
./node_exporter
3. Configuração do Prometheus
Editar o arquivo prometheus.yml:

yaml
Copiar código
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['<endereco_do_servidor>:9100']
Reiniciar o Prometheus:

bash
Copiar código
sudo systemctl restart prometheus
4. Integração do Grafana com Prometheus
Adicionar o Prometheus como fonte de dados no Grafana:

Vá para Configuration > Data Sources.
Clique em Add data source.
Selecione Prometheus e configure a URL (http://localhost:9090).
Criar um painel no Grafana:

Vá para Dashboards > Create Dashboard.
Adicione gráficos usando as consultas abaixo.
5. Consultas em PromQL Comuns
Uso da CPU
promql
Copiar código
sum(rate(node_cpu_seconds_total{mode!="idle"}[5m])) by (instance)
Uso da Memória
promql
Copiar código
(node_memory_MemTotal_bytes - node_memory_MemFree_bytes - node_memory_Cached_bytes) / node_memory_MemTotal_bytes * 100
Uso do Disco
promql
Copiar código
100 - (node_filesystem_free_bytes{fstype!="tmpfs"} / node_filesystem_size_bytes{fstype!="tmpfs"} * 100)
6. Ferramentas de Benchmarking
Gerar carga com stress:
bash
Copiar código
# Teste de CPU
stress --cpu 4 --timeout 60

# Teste de memória
stress --vm 2 --vm-bytes 512M --timeout 60
Testes com sysbench:
bash
Copiar código
# Teste de CPU
sysbench --test=cpu --cpu-max-prime=20000 run

# Teste de memória
sysbench --test=memory --memory-total-size=1G run
Conclusão
Com esta documentação, você pode configurar o monitoramento de servidores usando Prometheus e Grafana, além de testar a performance do servidor com ferramentas de benchmarking.
