## **Sniffing with parameter "-v"**: 
**verbose mode**
```bash 
sudo snort -v

```

## Sniffing with parameter "-i": 
**verbose mode (-v)** and **use the interface**

```bash

sudo snort -v-i eth0


```

## Sniffing with parameter "-d"
**dumping packet data mode**
```bash
 sudo snort -d
  
```

## Sniffing with parameter "-de"
**dump (-d)** and **link-layer header grabbing**

```bash
snort -d -e

```

  
## Sniffing with parameter "-X"
**full packet dump mode**

```bash
sudo snort -X
```

## Logging with parameter "-l"
**packet logger mode**
```bash
sudo snort -dev -l .


```

## Logging with parameter "-K ASCII"
**packet logger mode;**
```bash
sudo snort -dev -K ASCII


sudo snort -dev -K ASCII -l .

```

## Reading generated logs with parameter "-r"
**packet reader mode**
```bash
sudo snort -r

sudo snort -r snort.log.1638459842

```

##   Investigating single PCAP with parameter "-r"

```bash
sudo snort -c /etc/snort/snort.conf -q -r icmp-test.pcap -A console -n 10
```

## Investigating multiple PCAPs with parameter "--pcap-list"
```bash
sudo snort -c /etc/snort/snort.conf -q --pcap-list="icmp-test.pcap http2.pcap" -A console -n 10
```

##  Investigating multiple PCAPs with parameter "--pcap-show"

```bash
sudo snort -c /etc/snort/snort.conf -q --pcap-list="icmp-test.pcap http2.pcap" -A console --pcap-show

```