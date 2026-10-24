As outlined earlier, this resource will automatically create a printer port for this
printer if one does not exist. In its current form, the underlying printer provider
expects that the printer port has the name `IP_#{ipv4_address}`—so if you have a
custom-named port, it will not be used.

# ## Examples of managing printers

Create a new printer that is named `HP LaserJet, Bldg. A Floor 20` addressed by
the `10.0.0.5` IP. This will use the default settings when constructing a printer port,
so the new port will be named `IP_10.0.0.0.15` and will use port `9001` and the RAW
protocol with no SNMP as follows:

```
```
windows_printer 'HP LaserJet, Bldg. A Floor 20' do
 driver_name 'HP LaserJet 1320'
 ipv4_address '10.0.0.5'
end
```
```

Create a more finely grained printer and port configuration with the following code:

```
```
windows_printer_port '10.0.0.10' do
 port_name 'IP_10.0.0.10'
 port_number 9001
 port_description 'Building B print server'
 snmp_enabled true
end
```
```

The following code would create our port, `IP_10.0.0.10`, which we could then bind
a printer to:

```
```
windows_printer 'HP LaserJet 6L - Copy Room' do
 driver_name 'HP LaserJet 6L'
 ipv4_address '10.0.0.10'
 location 'Building B, Floor 13, Copy Room'
 comment 'Haunted floor, look out for ghosts.'
end
```
```

Now, when the provider constructs the printer, it will use the `IP_10.0.0.10` port,
which we built with custom settings earlier.