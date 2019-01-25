# devops-toolbox

## Bash Scripts

### Scripting Tricks

Pipe and awk

### Networking

nslookup

Tricks when no curl 
```
exec 3<> /dev/tcp/$DESTINY_IP/$DESTINY_PORT;echo $?

exec 3<> /dev/tcp/52.11.213.82/22;echo $?
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
