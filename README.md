
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
<br>
<br>
<br>

#### 3. My testnet RPC nodes are experiencing increasing disk usage. Are there any pruning options?
__Answer__:

Yes, its expected for now...
We'll release an update that enables pruning in the near future. It's not currently possible


#### 4. There is no derived pool address in latest_validator_info.json file
__Answer__:
In your latest_validator_info.json file, your derived pool address is the value of account_address 

## Issues, Errors & Solutions

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Blocks are frozen - Validator Node</summary>
    <h3>Description</h3>
    <pre>[2024-10-17T01:51:50.607720Z+00:00] WARN moonshot::core: Timeout reached for 
View { epoch_id: EpochId { chain_id: 6, epoch: 195 }, round: 2306 }
[2024-10-17T01:51:55.608661Z+00:00] WARN moonshot::core: Timeout reached for 
View { epoch_id: EpochId { chain_id: 6, epoch: 195 }, round: 2306 }
[2024-10-17T01:52:00.610197Z+00:00] WARN moonshot::core: Timeout reached for 
View { epoch_id: EpochId { chain_id: 6, epoch: 195 }, round: 2306 }</pre>
    <p>Epoch and round are stuck in a particular number</p>
    <h3>Solution</h3>
    You would need to restart the network using a snapshot.<br>
    Following this <a href="https://docs.google.com/document/d/1k0u7k58wFN0RlBmA-2eYrmsJZOGQQQASMINrYgugaVE/edit?tab=t.0#heading=h.vpui32qz16sp" > on-boarding document </a>
    <p>1. Re-run <code>.supra $./onboarding_mainnet.sh</code></p>
    <p>2. Then select <pre> Select Phase V - Restart the network using snapshot</pre>
    </p>


</details>
<hr>
<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Corruption: IO error</summary>

  <h3>Description</h3>
    <p>Database thread 'main' panicked</p>
    <pre>thread 'main' panicked at /home/ubuntu/smr-moonshot/consensus/node/src/bftnode.rs:57:14:
fail to create rocksdb: DBError(Error {
  message: "Corruption: Corruption: IO error: No such file or directory: While open a file for random read: configs/smr_storage/000364.ldb: No such file or directory in file configs/smr_storage/MANIFEST-000369" })
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace</pre>
    <h3>Solution</h3>
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
    <h3>Description</h3>
    <p>rpc::client: Failed to reconnect to server, will try again in 5 seconds</p>
    <h3>Solution</h3>
    <strong>Step 1:</strong> Open port 26000 and 27000<br>
    <strong>Step 2</strong> Restart container (replace container-id with your actual container ID)<br>
    <pre>docker restart container-id </pre>

</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">RPC Node Phase 2 Error</summary>
    <img src="./images/UnexpectedEof.png" alt="Frozen blocks">
    <h3>Description</h3>
    <p>kind: UnexpectedEof, error: Error("EOF while parsing a value", line: 1, column: 0)</p>
    <h3>Solution</h3>
    <pre>docker stop ${Container_name}
    docker remove ${Container_name}</pre>
    Following this <a href="https://docs.google.com/document/d/1pWXxS38HDTU0U0c-QQA0tPWt05jlfI-hd6gSTTCIFmM/edit?tab=t.0#heading=h.lxae1w9pebkd" > on-boarding document </a> repeat Step 1-2
   
</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Grafana not populating data to graphs</summary>
    <img src="./images/grafana-no-data-1.png" alt="grafana-no-data">
    <img src="./images/grafana-no-data-2.png" alt="grafana-no-data">
    <h3>Description</h3>
    <p>Dashboard is not populating correctly</p>
    <h3>Solution</h3>
    Add the full log path with the file name inside the <pre>/etc/promtail/config.yml</pre>
and restart the promtail.service
    It should look like the below
    <code>__path__: "/home/node/supra_configs_mainnet/supra_node_logs/supra.log"</code>
</details>
<hr>
<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Grafana setup IP is not whitelisted?</summary>
  
  <h3>Description</h3>
  <pre>Status 403: Your IP is not whitelisted. Please provide your IPv4 to the Supra Team.</pre>
   <h3>Solution</h3>
  <p>Please provide your IPv4 to the Supra Team on Discord</p>
</details>
<hr>
<details>
  <summary style="font-weight: bold; font-size: 1.2em;">ERROR: API key is missing</summary>
  <h3>Description</h3>
  <p>Error API Key is missing or similar</p>
  <pre>supra@ip-172-31-24-250:~$ chmod +x nodeops-monitoring-telegraf.sh
supra@ip-172-31-24-250:~$ sudo ./nodeops-monitoring-telegraf.sh
[sudo] password for supra:

ERROR: API key is missing.</pre>

<h3>Solution</h3>


<strong>Step 1:</strong> <code>export api_key=AIzaSyD2Byf2_yWYngvHnv6Ib7V6C2EpHY3LL0E</code>

Run the below step according to your distribution<br>
<strong>Step 2:</strong> <pre>sudo -E ./nodeops-monitoring-telegraf-centos.sh or sudo -E ./nodeops-monitoring-telegraf.sh</pre>

<strong>Note:</strong> <p>If you face 403 IP is not whitelisted while running the above steps, then please provide your node IP to  the Supra Team on discord, so that we can provide you access to the Grafana script, after getting whitelisted you can run the above steps again.</p>
</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">RPC node out of sync!</summary>
  <h3>Description</h3>
 <pre>INFO rpc_node::listener: RPC node out of sync!</pre>
 <h3>Solution</h3>
 Following this <a href="https://docs.google.com/document/d/1pWXxS38HDTU0U0c-QQA0tPWt05jlfI-hd6gSTTCIFmM/edit?tab=t.0#heading=h.lxae1w9pebkd" > on-boarding document </a>
<pre>.supra $./rpc_onboarding_mainnet.sh 
Select Phase III - Re-Start RPC node</pre>
</details>

<hr>

<details>
  <summary style="font-weight: bold; font-size: 1.2em;">Subscription execution failed: "Closed(..)</summary>
    <h3>Description</h3>
    <pre>[2024-11-06T08:20:47.531043Z+00:00] ERROR sop2p::behaviour: Subscription execution failed: "Closed(..)"    
[2024-11-06T08:20:47.531922Z+00:00] ERROR sop2p::behaviour: Subscription execution failed: "Closed(..)"    
[2024-11-06T08:20:47.531961Z+00:00] ERROR sop2p::behaviour: Subscription execution failed: "Closed(..)"</pre>
  <h3>Solution</h3>
  <p> Increase System ulimit if not done already</p>
    <pre>
      # Step1:increase shell fd limit
      ulimit -n 65535
      ulimit -n
    </pre>
    <pre>
      # Step2: increase limit for user
      sudo nano /etc/security/limits.conf
      soft nofile 65535
      hard nofile 65535
    </pre>
    <pre>
      # Step3: increase sys limit
      sudo nano /etc/sysctl.conf
      net.core.somaxconn=65535
      sudo sysctl -p
    </pre>
  <p> Restart node from lates snapshot</p>
    <p>Step 1: Download the update_version_round_6_3.sh.</p>
      <pre>wget https://raw.githubusercontent.com/Entropy-Foundation/supra-nodeops-data/refs/heads/master/scripts/update_version_round_6_3.sh</pre>
        
    <p>Step 2: <code>chmod +x update_version_round_6_3.sh</code></p>
    <p>Step 3: <code>./update_version_round_6_3.sh</code></p>
</details>