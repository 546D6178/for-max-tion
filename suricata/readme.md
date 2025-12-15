# Setup the training env

```bash
docker-compose up
```

To access the training environment CLI:


```bash
docker-compose exec suricata bash
```

or
 
```bash
docker exec -it suricata-suricata-1 /bin/bash 
```

Just in case

```bash
docker exec -it suricata-evebox-1 /bin/bash 
```

To access EveBox for visual event display go to http://localhost:5636 with a
browser on your machine.

## Replaying a PCAP

To replay a PCAP first enter the training environment CLI (see above),
then run the following command:

```bash
./replay-pcap.sh ../pcaps/excel-network-obfuscated-powershell2.pcap 
```


## Ending


```bash
docker-compose down
cd tools
rm -f eve.json fast.log stats.log suricata.log 
```


## Debuging


### No rules?

```bash
suricata -c /etc/suricata/suricata.yaml -r ../pcaps/excel-network-obfuscated-powershell2.pcap 
i: suricata: This is Suricata version 8.0.2 RELEASE running in USER mode
W: detect: No rule files match the pattern /var/lib/suricata/rules/suricata.rules
W: detect: 1 rule files specified, but no rules were loaded!
i: mpm-hs: Rule group caching - loaded: 0 newly cached: 0 total cacheable: 0
i: threads: Threads created -> RX: 1 W: 20 FM: 1 FR: 1   Engine started.
i: suricata: Signal Received.  Stopping engine.
i: pcap: read 1 file, 530 packets, 28940 bytes
```

==> `W: detect: 1 rule files specified, but no rules were loaded!`

```bash
sudo suricata-update
```

### No alert?

```bash
./replay-pcap.sh ../pcaps/excel-network-obfuscated-powershell2.pcap 


Alerts:
```

==> verify that :


```bash
$ grep "tcpreplay -i" tools/replay-pcap.sh 
tcpreplay -i eth1 -x 12 ${PCAPFILE} > /dev/null 2>&1

$ grep "suricata -i" docker-compose.yml 
    command: suricata -i eth1
```

&

```bash
docker-compose exec suricata bash
ip a | grep eth1
```