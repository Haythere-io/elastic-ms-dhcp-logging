# elastic-ms-dhcp-logging
Log collection of MS Dhcp logs in Elasticsearch
With big thanks to Guy Bruneau and his blog post at https://isc.sans.edu/diary/27198
His blogpost was a great help but needed a little tweaking to get things working. All data here is from his blogpost. I modified the template, index initialisation, grok pattern and logstash pipeline filter.

## Create the ilm policy.

Start by adding the ilm policy with the following command in the Kibana Dev tools.

```
PUT  _ilm/policy/microsoft.dhcp
{
 <insert content from ms-dhcp-ilm-policy.yml here>
}
```

## Create the index template

Add the template with the following command:
```
PUT _template/microsoft.dhcp
{
 <insert content of ms-dhcp-template.yml here>
}
```

## Initialise the index

Initialising the index helps with proper alias setup.
Initialise the index with the following command
```
PUT microsoft.dhcp-<yyyy.mm.dd>-000001
{
 <insert content of ms-dhcp-index-initialise.yml here>
}
```

## Create MAC Vendor translation data

The logstash pipeline contains a filter which is used to compare the host MAC address OUI against a local list (in this configuration it is: oui.yml). Get OUI list from the web and convert it into a yml list saved in the /opt directory.

```
sudo cd /opt
sudo wget http://standards-oui.ieee.org/oui/oui.txt
sudo cat /opt/oui.txt | grep 'base 16' |sed -e 's/\([[:xdigit:]]\{6\}\).*(base 16)\t\t\(.*\)\r/"\1": \2/gi' | tr -d '@' > /opt/oui.yml
sudo rm /opt/oui.txt
```

## Create the Logstash pipeline config
Place this logstash [config file](logstash-ms-dhcp-pipeline.yml)  in de /etc/logstash/conf.d folder or create a logstash ingest pipeline in Kibana. (depending on your license and config method)
