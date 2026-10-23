```
EnclaveSwitch (config)# monitor session 1 destination interface gigabitEthernet1/0/2 encapsulation replicate  
EnclaveSwitch (config)# end
```

That covers packet capturing within enclaves. Next, we will talk about cross-enclave packet capturing.

# Cross-enclave network packet capturing

To be able to perform network packet capturing or traffic inspecting on communications between enclaves (but not up to Level 3 – Site Operations), the OT-Firewall will be used.

The process is illustrated in the following screenshot:

![](bbox=[0.13, 0.36, 0.87, 0.91])