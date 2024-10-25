
<img src="./images/supra-oracles.jpg" alt="Supra Oracles logo">

# NodeOps-Support-FAQ

This document is created to support node operators, with their issue, and solutions they can administer before reaching out for support.


## FAQ
#### 1. Should the onboarding script be run directly on the machine that will act as the staking provider?

__Answer__:
You should run all the steps of Round onboarding for Public Testnet on the machine that will act as a stacking provider.

#### 2. What are the hardware requirements?

__Answer__:

Below are the hardware requirements: 
- Cores: 16 to 32
- RAM: 64G
- CPU: Intel(R) Xeon(R) Platinum CPU @ 2.8GHz speed or higher
- Architecture: x86/64
- Disk Type: SSD
- Minimum Disk Size: 2TB
- Network Bandwidth: 1Gbps
<hr>
<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Blocks are frozen</summary>
    <img src="./images/Frozen-blocks.png" alt="Frozen blocks">
    <h4>Description</h4>
    <p>Epoch and round are stuck in a particular number</p>
    <h4>Solution</h4>
</details>
<hr>
<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Corruption: IO error</summary>
  <img src="./images/io-error.png" alt="Frozen blocks">
  <h4>Description</h4>
    <p>Database thread 'main' panicked</p>
    <h4>Solution</h4>
    1. <code>docker ps -a</code><br>
    2. <code>docker stop supra_${ip_address}</code><br>
    3. <code>sudo rm -rf ./supra_configs/ledger_storage ./supra_configs/smr_storage/* ./supra_configs/supra_node_logs </code><br>
    4. <code>./supra_configs/latest_snapshot.zip ././supra_configs/snapshot </code><br>
    5.<code> wget -O ./supra_configs/latest_snapshot.zip https://testnet-snapshot.supra.com/snapshots/latest_snapshot.zip </code><br>
    6. <code>unzip ./supra_configs/latest_snapshot.zip -d ./supra_configs/ </code><br>
    7. <code>cp ./supra_configs/snapshot/snapshot_*/* ./supra_configs/smr_storage/ </code><br>
    8. <code>docker start supra_${ip_address} </code><br>
    9. <code>docker exec -it supra_$ip_address /supra/supra node smr run </code>
</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">RPC Error on startup</summary>
    <img src="./images/rpc-error-on-startup.png" alt="Frozen blocks">
    <h4>Description</h4>
    <p>rpc::client: Failed to reconnect to server, will try again in 5 seconds</p>
    <h4>Solution</h4>
    <strong>Note:</strong> Open port 26000 and 27000<br>
    <strong>Step 1:</strong><br>
     <code>sudo rm -rf ./supra_configs/rpc_archive ./supra_configs/rpc_ledger ./supra_configs/snapshot ./supra_configs/rpc_store/* ./supra_configs/rpc_node_logs ./supra_configs/latest_snapshot.zip</code><br>
    <strong>Step 2:</strong><br>
    <code>wget -O ./supra_configs/latest_snapshot.zip https://testnet-snapshot.supra.com/snapshots/latest_snapshot.zip</code><br>
    <strong>Step 3:</strong><br>
    <code>unzip ./supra_configs/latest_snapshot.zip -d ./supra_configs/</code><br>
    <strong>Step 4: </strong><br>
    <code>cp -r ./supra_configs/snapshot/snapshot_*/* ./supra_configs/rpc_store/</code><br>
    <strong>Step 5:</strong><br>
    <code>docker exec -itd supra_rpc_{your_rpc_ip} /supra/rpc_node </code>

</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">RPC logs | ERROR ntex_files</summary>
    <img src="./images/ntex_files.png" alt="Frozen blocks">
    <h4>Description</h4>
    <p>ERROR ntex_files: Specified path is not a directory: "html_guide/"</p>
    <h4>Solution</h4>
</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">RPC Node Phase 2 Error</summary>
    <img src="./images/UnexpectedEof.png" alt="Frozen blocks">
    <h4>Description</h4>
    <p>kind: UnexpectedEof, error: Error("EOF while parsing a value", line: 1, column: 0)</p>
    <h4>Solution</h4>
    <pre>docker stop ${Container_name}
    docker remove ${Container_name}</pre>
    then repeat Step 1~3
</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Grafana not populating data to graphs</summary>
    <img src="./images/grafana-no-data-1.png" alt="grafana-no-data">
    <img src="./images/grafana-no-data-2.png" alt="grafana-no-data">
    <h4>Description</h4>
    <p>Dashboard is not populating correctly</p>
    <h4>Solution</h4>
    Add the full log path with the file name inside the <pre>/etc/promtail/config.yml</pre>
and restart the promtail.service
    It should look like the below
    <code>__path__: "/home/node/supra_configs_mainnet/supra_node_logs/supra.log"</code>
</details>