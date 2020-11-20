# HASS Count
Sensor that counts sensors...

![Example](./example.png?raw=true)

## Installation
- Copy & Paste the YAML :smile:

## Show current status
Home Assistant -> Developer Tools -> Template
```
{%- for d in states | groupby('domain') %}
{{ d[0] }} - {{ states[d[0]] | count  }}
{%- endfor %}
```
