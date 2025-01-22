
### Postgresql
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install my-postgresql bitnami/postgresql
```
create dump
```bash
kubectl create configmap database-dump --from-file=dump.sql
```

```yaml
helm upgrade --install my-postgresql bitnami/postgresql \
  --set primary.extraVolumes[0].name=database-dump \
  --set primary.extraVolumes[0].configMap.name=database-dump \
  --set primary.extraVolumeMounts[0].name=database-dump \
  --set primary.extraVolumeMounts[0].mountPath=/docker-entrypoint-initdb.d
```

```bash
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default my-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)
```

```bash
 kubectl run my-postgresql-client --rm --tty -i --restart='Never' --namespace default \
  --image docker.io/bitnami/postgresql:17.2.0-debian-12-r7 --env="PGPASSWORD=$POSTGRES_PASSWORD" \
  --command -- psql --host my-postgresql -U postgres -d postgres -p 5432
```

```sql
CREATE DATABASE users;
CREATE USER kirill WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE users TO kirill;
ALTER USER kirill WITH SUPERUSER;
```

```bash
kubectl apply -f postgres-cf.yaml  
kubectl apply -f postgres-sc.yaml
```

### Auth-service

```bash
docker build -t auth_service_app-k8s .
```

```bash
kind load docker-image auth_service_app-k8s:latest
```

```bash
kubectl apply -f auth-deployment.yaml
```


