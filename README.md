

---

## Monitoramento de Servidor com Grafana e Prometheus

### 1. Instalação do Prometheus e Grafana

#### Prometheus
```bash
# Baixar e extrair o Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v*/prometheus-*.*-linux-amd64.tar.gz
tar xvfz prometheus-*.*-linux-amd64.tar.gz
cd prometheus-*.*-linux-amd64

# Iniciar o Prometheus
./prometheus --config.file=prometheus.yml
```

#### Grafana
```bash
# Baixar e instalar o Grafana
wget https://dl.grafana.com/oss/release/grafana_*_amd64.deb
sudo apt-get install -y adduser libfontconfig1
sudo dpkg -i grafana_*_amd64.deb

# Iniciar o Grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

### 2. Configuração do Node Exporter

#### Instalar o Node Exporter no servidor:
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v*/node_exporter-*.*-linux-amd64.tar.gz
tar xvfz node_exporter-*.*-linux-amd64.tar.gz
cd node_exporter-*.*-linux-amd64
./node_exporter
```

### 3. Configuração do Prometheus

#### Editar o arquivo `prometheus.yml`:
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['<endereco_do_servidor>:9100']
```
> Substitua `<endereco_do_servidor>` pelo endereço IP ou hostname do seu servidor.

#### Reiniciar o Prometheus:
```bash
sudo systemctl restart prometheus
```

### 4. Integração do Grafana com Prometheus

#### Adicionar Prometheus como fonte de dados no Grafana:
1. Vá para "Configuration > Data Sources".
2. Clique em "Add data source".
3. Selecione "Prometheus" e configure a URL (por exemplo, `http://localhost:9090`).

#### Criar um painel no Grafana:
1. Vá para "Dashboards > Create Dashboard".
2. Adicione gráficos usando as consultas PromQL abaixo.

### 5. Consultas em PromQL Comuns

#### Uso da CPU
```promql
sum(rate(node_cpu_seconds_total{mode!="idle"}[5m])) by (instance)
```

#### Uso da Memória
```promql
(node_memory_MemTotal_bytes - node_memory_MemFree_bytes - node_memory_Cached_bytes) / node_memory_MemTotal_bytes * 100
```

#### Uso do Disco
```promql
100 - (node_filesystem_free_bytes{fstype!="tmpfs"} / node_filesystem_size_bytes{fstype!="tmpfs"} * 100)
```

### 6. Ferramentas de Benchmarking

#### Gerar carga com `stress`:
```bash
# Teste de CPU
stress --cpu 4 --timeout 60

# Teste de memória
stress --vm 2 --vm-bytes 512M --timeout 60
```

#### Testes com `sysbench`:
```bash
# Teste de CPU
sysbench --test=cpu --cpu-max-prime=20000 run

# Teste de memória
sysbench --test=memory --memory-total-size=1G run
```

#### Parar os testes:
Para parar qualquer um dos testes em execução, basta pressionar `Ctrl + C` no terminal onde o teste está sendo executado.

### 7. Conclusão

Com esta documentação, você pode configurar o monitoramento de servidores usando Prometheus e Grafana, além de testar a performance do servidor com ferramentas de benchmarking.

### 8. Licença

Este projeto é distribuído sob a licença MIT. Sinta-se à vontade para usar e modificar conforme necessário.

---

Espero que essa documentação detalhada seja útil para o seu projeto no GitHub! Se precisar de mais alguma melhoria ou tiver alguma dúvida, estou aqui para ajudar! 😊

