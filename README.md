# Lab 2 Starter Code (Python) — 3 Nodes (A/B/C)

This starter kit implements a minimal **Lamport clock + replicated key–value store** using only the **Python standard library**.

## Files
- `node.py`  — Node server (HTTP JSON), Lamport clock, replication, LWW conflict resolution
- `client.py` — Small CLI client to PUT/GET/STATUS

## Ports / Security Group
Open the node port (e.g. 8000/8001/8002) on each EC2 instance for inbound traffic from peer nodes.

## Run on EC2

### node A
python3 node.py --id A --port 8000 --peers http://<IP-B>:8001,http://<IP-A>:8002

### node B
python3 node.py --id B --port 8001 --peers http://<IP-A>:8000,http://<IP-C>:8002

### node C
python3 node.py --id C --port 8002 --peers http://<IP-A>:8000,http://<IP-B>:8001


## Scenario A:
python3 client.py --node http://<IP-A>:8000 put x 1

python3 client.py --node http://<IP-B>:8001 get x

python3 client.py --node http://<IP-C>:8002 status


## Scenario B:
python3 client.py --node http://<IP-A>:8000 put key1 "1"

python3 client.py --node http://<IP-B>:8001 put key1 "2"


## Scenario C:
Stop the python script on Node C (Ctrl+C)

python3 client.py --node http://<IP-A>:8000 put two_key "C_is_down"

Restart Node C: python3 node.py --id C --port 8002 --peers http://<IP-A>:8000,http://<IP-B>:8001

python3 client.py --node http://<IP-A>:8000 put another_key "replicated_now"

python3 client.py --node http://<IP-C>:8002 status
