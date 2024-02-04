# The Hunt for LOLbins

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt=""><figcaption></figcaption></figure>

I read the background and instructions (from the challenge page at [https://hacktoria.com/contracts/last-flight/](https://hacktoria.com/contracts/last-flight/)) and got the following notes out of it:

* The attacker is known to use malicious USBs
* I might have to reverse some code
* My job is to look through Event Logs and locate something suspicious
* The answer format is: `W0rd{s0me-very-0bscur3-gu1d-5tr1ng}`.&#x20;

I started off by downloading the material. It was an .evtx file. I use Linux, so I had to find a way to read the file. I ended up finding [https://github.com/omerbenamram/evtx](https://github.com/omerbenamram/evtx). I was now able to read the file:

<figure><img src="../../.gitbook/assets/image (2) (1) (2).png" alt=""><figcaption></figcaption></figure>

I think my first step should be locating any USB activity on the system. I can then try to drill down on the location where the malicious event is occurring. I was doing research and stumbled upon [https://www.csoonline.com/article/3561889/the-most-important-windows-10-security-event-log-ids-to-monitor.html](https://www.csoonline.com/article/3561889/the-most-important-windows-10-security-event-log-ids-to-monitor.html). Here they mention, looking for the following IDs: 1936, 1937, 1938. I searched for 1936 (`./evtx_dump-v0.8.1-x86_64-unknown-linux-gnu WinEvents.evtx | grep "1936" -A 30 -B 15`) and ended up getting the following:

```xml
Record 1605
<?xml version="1.0" encoding="utf-8"?>
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="Service Control Manager" Guid="{555908d1-a6d7-4695-8e1e-26931d2012f4}" EventSourceName="Service Control Manager">
    </Provider>
    <EventID Qualifiers="16384">7036</EventID>
    <Version>0</Version>
    <Level>4</Level>
    <Task>0</Task>
    <Opcode>0</Opcode>
    <Keywords>0x8080000000000000</Keywords>
    <TimeCreated SystemTime="2023-04-21T09:47:49.577215Z">
    </TimeCreated>
    <EventRecordID>1936</EventRecordID>
    <Correlation>
    </Correlation>
    <Execution ProcessID="448" ThreadID="508">
    </Execution>
    <Channel>System</Channel>
    <Computer>PEGASUS.NEFARIOUS.CORP</Computer>
    <Security>
    </Security>
  </System>
  <EventData>
    <Data Name="param1">System Events Broker</Data>
    <Data Name="param2">running</Data>
    <Binary>530079007300740065006D004500760065006E0074007300420072006F006B00650072002F0034000000</Binary>
  </EventData>
</Event>
```

At the bottom, I noticed a Binary tag with a hexadecimal string. I entered the string online, and the output was the following: `SystemEventsBroker/4`. I think I might be onto something here. My plan now is to look for more binary strings. Searching just for the "Binary" tag ended with me getting 1525 results. I want to write a bash command that will decode all of this line by line for me. I ended up using `/evtx_dump-v0.8.1-x86_64-unknown-linux-gnu WinEvents.evtx | grep "Binary" | sed -e 's///g' -e 's/</Binary>//g' | xxd -r -p`. The output was a chunk of data:

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (2) (2) (1).png" alt=""><figcaption></figcaption></figure>

This is human-readable, so I need to drill down on what I am actually looking for. I did not see anything too usable from that. I went back to the same article I mentioned previously and ran the command`/evtx_dump-v0.8.1-x86_64-unknown-linux-gnu WinEvents.evtx | grep "1102" -A 20 -B 20`. I then saw something interesting in the output:

```xml
Record 1806
--
  </System>
  <EventData>
    <Data Name="param1">C:\Windows\Explorer.EXE (PEGASUS)</Data>
    <Data Name="param2">PEGASUS</Data>
    <Data Name="param3">Other (Unplanned)</Data>
    <Data Name="param4">0x5000000</Data>
    <Data Name="param5">power off</Data>
    <Data Name="param6"></Data>
    <Data Name="param7">NEFARIOUS\Administrator</Data>
  </EventData>

```

I notice that Windows Explorer is being used here. By looking around, I ended up finding a host that might be the victim: `WIN-BPIG0DE7217`. At this point, I was just searching around to see what I can find, with no strategy, if I am being honest. I did see winlogon being used 2 times:

```xml
<Data Name="param1">C:\Windows\system32\winlogon.exe (WIN-BPIG0DE7217)</Data>
<Data Name="param1">C:\Windows\system32\winlogon.exe (WIN-6NISDFG4M0U)</Data>
```

These could be the hosts that were "hit". I ended up reading the writeup at [https://flagthecapture.blogspot.com/2023/05/ctf-writeup-hacktoria-contract-hunt-for.html](https://flagthecapture.blogspot.com/2023/05/ctf-writeup-hacktoria-contract-hunt-for.html) and learned I overlooked a "Provider" named "BKyz". I then ran `./evtx_dump-v0.8.1-x86_64-unknown-linux-gnu WinEvents.evtx | grep -i "BKyz" -A 20` and saw the following:

```xml
   <Provider Name="BKyz">
    </Provider>
    <EventID Qualifiers="0">1337</EventID>
    <Level>4</Level>
    <Task>0</Task>
    <Keywords>0x80000000000000</Keywords>
    <TimeCreated SystemTime="2023-04-21T09:42:06.000000Z">
    </TimeCreated>
    <EventRecordID>1756</EventRecordID>
    <Channel>System</Channel>
    <Computer>PEGASUS.NEFARIOUS.CORP</Computer>
    <Security>
    </Security>
  </System>
  <EventData>
    <Data>%COMSPEC% /b /c start /b /min powershell -nop -w hidden -encodedcommand JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAE0AZQBtAG8AcgB5AFMAdAByAGUAYQBtACgALABbAEMAbwBuAHYAZQByAHQAXQA6ADoARgByAG8AbQBCAGEAcwBlADYANABTAHQAcgBpAG4AZwAoACIASAA0AHMASQBBAE0AbABQAFEAbQBRAEEALwAzADEAVAB3AFcAcQBGAE0AQgBDADgAQgAvAEkAUABDADgAOQBpAHkANgBOAGcAYgBGAEwAcgA0AFUASAAvAFEAMABTAEMAVwBtAHUAUgBDAE4ARwBlADIAdgA2ADcAcwBaAGIAaQBMAEsAaQAzAGMAUwBhAHoAawAzAEcAOQBVAEQAMAAyAGIAVABYAE4AMQBzADkAUwBTAEIARgBaADcAKwBsAEcAcgAvAGMAUABnAEsAUwBnADgAQgBUADEAdQAvAFYAbABsAHUANgBSAEEAWgBUAG4AZQA2AFMAUwBEAEsAQgA2ADMAawBQADkAZwBxAFQARwBvAHcAWgA4ADAAUwBoADkATwBpAFoATgBjAGgAegBQAHEATwBNAEEAcQBGAFEASgAyAEQAQwBvAEkAWgB5AEcAUwArAE4AOABOAGcATwBTAG8AZwB1AGYAagA4AEYAWgBHAGkARABSAGgANwBWAHgAMQBnADAAMABmAHUAbwBDAHYAUwBHAEgAdAAyAGQAZABZAEUANwA4ADQATwB5AEcATQBJAEwARgBoAHIAMQBCAFQAcQBVAGgAOQA3AGEAcwBiADQAUAB0AHcAcgBaAEcANwBuAE0AWQAvAHIAZgAzAG0AKwA3AG8AYQB6AHUAdwBDAGEANQBCAFUAZABIAGoAeAA5AGcANwBpAG0ATQBwAGYAbABaAHAATQBjADIAKwBkADEAMwA1AEsAMQBsAGYALwBQADAAWAByAFcAcwBXAHIAUgBlAFAAKwBpAEkARABBAEEAQQA9ACIAKQApADsASQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABJAE8ALgBTAHQAcgBlAGEAbQBSAGUAYQBkAGUAcgAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABJAE8ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ALgBHAHoAaQBwAFMAdAByAGUAYQBtACgAJABzACwAWwBJAE8ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ATQBvAGQAZQBdADoAOgBEAGUAYwBvAG0AcAByAGUAcwBzACkAKQApAC4AUgBlAGEAZABUAG8ARQBuAGQAKAApADsA</Data>
    <Binary>0A14</Binary>
  </EventData>
</Event>

```

The event data includes a PowerShell command. I decoded it using the command line:

```bash
#Command:
echo "JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAE0AZQBtAG8AcgB5AFMAdAByAGUAYQBtACgALABbAEMAbwBuAHYAZQByAHQAXQA6ADoARgByAG8AbQBCAGEAcwBlADYANABTAHQAcgBpAG4AZwAoACIASAA0AHMASQBBAE0AbABQAFEAbQBRAEEALwAzADEAVAB3AFcAcQBGAE0AQgBDADgAQgAvAEkAUABDADgAOQBpAHkANgBOAGcAYgBGAEwAcgA0AFUASAAvAFEAMABTAEMAVwBtAHUAUgBDAE4ARwBlADIAdgA2ADcAcwBaAGIAaQBMAEsAaQAzAGMAUwBhAHoAawAzAEcAOQBVAEQAMAAyAGIAVABYAE4AMQBzADkAUwBTAEIARgBaADcAKwBsAEcAcgAvAGMAUABnAEsAUwBnADgAQgBUADEAdQAvAFYAbABsAHUANgBSAEEAWgBUAG4AZQA2AFMAUwBEAEsAQgA2ADMAawBQADkAZwBxAFQARwBvAHcAWgA4ADAAUwBoADkATwBpAFoATgBjAGgAegBQAHEATwBNAEEAcQBGAFEASgAyAEQAQwBvAEkAWgB5AEcAUwArAE4AOABOAGcATwBTAG8AZwB1AGYAagA4AEYAWgBHAGkARABSAGgANwBWAHgAMQBnADAAMABmAHUAbwBDAHYAUwBHAEgAdAAyAGQAZABZAEUANwA4ADQATwB5AEcATQBJAEwARgBoAHIAMQBCAFQAcQBVAGgAOQA3AGEAcwBiADQAUAB0AHcAcgBaAEcANwBuAE0AWQAvAHIAZgAzAG0AKwA3AG8AYQB6AHUAdwBDAGEANQBCAFUAZABIAGoAeAA5AGcANwBpAG0ATQBwAGYAbABaAHAATQBjADIAKwBkADEAMwA1AEsAMQBsAGYALwBQADAAWAByAFcAcwBXAHIAUgBlAFAAKwBpAEkARABBAEEAQQA9ACIAKQApADsASQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABJAE8ALgBTAHQAcgBlAGEAbQBSAGUAYQBkAGUAcgAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABJAE8ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ALgBHAHoAaQBwAFMAdAByAGUAYQBtACgAJABzACwAWwBJAE8ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ATQBvAGQAZQBdADoAOgBEAGUAYwBvAG0AcAByAGUAcwBzACkAKQApAC4AUgBlAGEAZABUAG8ARQBuAGQAKAApADsA" | base64 -d
#Output:
$s=New-Object IO.MemoryStream(,[Convert]::FromBase64String("H4sIAMlPQmQA/31TwWqFMBC8B/IPC89iy6NgbFLr4UH/Q0SCWmuRCNGe2v67sZbiLKi3cSazk3G9UD02bTXN1s9SSBFZ7+lGr/cPgKSg8BT1u/Vllu6RAZTne6SSDKB63kP9gqTGowZ80Sh9OiZNchzPqOMAqFQJ2DCoIZyGS+N8NgOSogufj8FZGiDRh7Vx1g00fuoCvSGHt2ddYE784OyGMILFhr1BTqUh97asb4PtwrZG7nMY/rf3m+7oazuwCa5BUdHjx9g7imMpflZpMc2+d135K1lf/P0XrWsWrReP+iIDAAA="));IEX (New-Object IO.StreamReader(New-Object IO.Compression.GzipStream($s,[IO.Compression.CompressionMode]::Decompress))).ReadToEnd();
```

There seems to be a base64 string encoded in this command. I used the command line to base64 decode this one as well...but the result turned out to be not human-readable. I then assumed it was a command you can run in PowerShell and see the output. I used [https://tio.run/#powershell](https://tio.run/#powershell) and got the flag:

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (3).png" alt=""><figcaption></figcaption></figure>

The flag was `H4ckt0ria{a24304dd-1209-4f2f-a926-a3a1140f3989}`.&#x20;

<figure><img src="../../.gitbook/assets/Contract_Card-The_Hunt_for_LOLBins.cleaned.png" alt=""><figcaption></figcaption></figure>
