# Setup the training env

```
docker-compose up
```

To access the training environment CLI:

```
docker-compose exec suricata bash
docker exec -it suricata-evebox-1 /bin/bash 
docker exec -it suricata-suricata-1 /bin/bash 
```

To access EveBox for visual event display go to http://localhost:5636 with a
browser on your machine.

## Replaying a PCAP

To replay a PCAP first enter the training environment CLI (see above),
then run the following command:

```
./replay-pcap.sh ../pcaps/purplefox-exploit-kit-with-powershell-payloads.pcap
```
