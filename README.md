# elastic-ms-dhcp-logging
Log collection of MS Dhcp logs in Elasticsearch
With big thanks to Guy Bruneau and his blog post at https://isc.sans.edu/diary/27198
His blogpost was a great help but needed a little tweaking to get things working.

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
