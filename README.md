# envoy-canary-sample

## Deploy sample app
```bash
kubectl create ns order

kubectl apply -f order-app.yaml
kubectl apply -f order-app-canary.yaml
```

## Envoy Gateway
### Deploy Envoy Gateway

```bash
helm install eg oci://docker.io/envoyproxy/gateway-helm --version v0.5.0 -n envoy-gateway-system --create-namespace

kubectl apply -f envoy-gw.yaml
```

### Create A record

```bash
export YOUR_IP_ADDRESS=<YOUR_IP_ADDRESS>
export DNS_ZONE=kkuchima-com

gcloud dns record-sets transaction start --zone=${DNS_ZONE}

gcloud dns record-sets transaction add --zone=${DNS_ZONE} \
--name="order.kkuchima.com." --type A --ttl=300 "${YOUR_IP_ADDRESS}"

gcloud dns record-sets transaction execute --zone=${DNS_ZONE}

gcloud dns record-sets list --zone=${DNS_ZONE}
```

## Envoy
