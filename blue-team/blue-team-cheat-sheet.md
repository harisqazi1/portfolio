# Blue Team Cheat Sheet

## Linux

### Malware Analysis

{% embed url="https://docs.remnux.org/" %}

{% embed url="https://zeltser.com/remnux-malware-analysis-tips/" %}

## Windows

### Malware Analysis

{% embed url="https://github.com/OALabs/BlobRunner" %}

{% embed url="https://github.com/mandiant/flare-vm" %}

{% embed url="https://www.malwarearchaeology.com/cheat-sheets" %}

### Event Logs

{% embed url="https://github.com/WithSecureLabs/chainsaw" %}

{% embed url="https://github.com/omerbenamram/evtx" %}

```bash
#Outputs to script_output.txt | File has XML in it
for file in /home/kali/Downloads/forensics/Windows/System32/Winevt/Logs/*
do
  ../evtx_dump-v0.8.1-x86_64-unknown-linux-musl "$file" >> script_output.txt
done
```

### Visual Basic Scripts

{% embed url="https://github.com/decalage2/ViperMonkey" %}
