# Netcat

## Transfer file
### Sender
```
nc -w 3 [IP] [PORT] < out.file
```

### Receiver
```
nc -lp [PORT] > out.file
```
