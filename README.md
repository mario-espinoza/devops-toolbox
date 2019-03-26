# devops-toolbox

## Bash Scripts

### Scripting Tricks

Pipe and awk

### Networking

1. nslookup

´´´
nslookup
´´´

2. Tricks when no curl 
```
exec 3<> /dev/tcp/$DESTINY_IP/$DESTINY_PORT;echo $?

exec 3<> /dev/tcp/52.11.213.82/22;echo $?
```

3. show Ips by interface
Ex: interface eth0
```
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```
4. To find process that uses a port
a. Good-ole netstat
```
netstat -ltnp | grep -w ':80' 
```
b. LSOF
```
lsof -i :80 
```
c. Using `fuser` and `ps``
```
fuser 80/tcp
ps -p 2053 -o comm=
```
## PM2 CheatSheet
### Fork mode
`pm2 start app.js --name my-api # Name process`

### Cluster mode
```
pm2 start app.js -i 0        # Will start maximum processes with LB depending on available CPUs
pm2 start app.js -i max      # Same as above, but deprecated.
```


### Listing
```
pm2 list               # Display all processes status
pm2 jlist              # Print process list in raw JSON
pm2 prettylist         # Print process list in beautified JSON

pm2 describe 0         # Display all informations about a specific process

pm2 monit              # Monitor all processes
```

### Logs

```
pm2 logs [--raw]       # Display all processes logs in streaming
pm2 flush              # Empty all log files
pm2 reloadLogs         # Reload all logs
```

### Actions

```
pm2 stop all           # Stop all processes
pm2 restart all        # Restart all processes

pm2 reload all         # Will 0s downtime reload (for NETWORKED apps)

pm2 stop 0             # Stop specific process id
pm2 restart 0          # Restart specific process id

pm2 delete 0           # Will remove process from pm2 list
pm2 delete all         # Will remove all processes from pm2 list
```
### Misc
```
pm2 reset <process>    # Reset meta data (restarted time...)
pm2 updatePM2          # Update in memory pm2
pm2 ping               # Ensure pm2 daemon has been launched
pm2 sendSignal SIGUSR2 my-app # Send system signal to script
pm2 start app.js --no-daemon
pm2 start app.js --no-vizion
pm2 start app.js --no-autorestart
```
## Kubernetes

Para listar pods ordenados por reinicios
```
kubectl get pods --all-namespaces --sort-by='.status.containerStatuses[0].restartCount'
```
Para extraer los detalles de deployment por namespace y escribir a un archivo de texto
```
kubectl get ns | awk '{cmd="kubectl describe deployment -n "$1" > "$1".txt"; system(cmd)}'
```

Para descargar todos los logs de un pod /namespace:
```
kgp -n podName |awk '{cmd="kubectl logs -n podName "$1" > "$1".txt"; if(NR>1)system(cmd)}'
```
