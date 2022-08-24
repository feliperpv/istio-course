# Full Cycle Service Mesh with Istio Course (https://plataforma.fullcycle.com.br/)

- [k3d](https://k3d.io/v5.3.0/): k3d is a lightweight wrapper to run k3s (Rancher Lab’s minimal Kubernetes distribution) in docker.
- [kubectl](https://kubernetes.io/docs/tasks/tools/): Kubernetes command-line tool, run commands against Kubernetes clusters
- [istio](https://istio.io/latest/): Service Mesh that helps to manage distributed architectures

# k3d
```
$ k3d cluster create -p "8500:30000@loadbalancer" --agents 2

$ kubectl config use-context k3d-k3s-default
```

# Service Mesh
	- Dedicated infrastructure layer for adding observability, security and reliability to applications/clusters
	- Istio is a open-source service mesh 

# Istio
	- https://istio.io/latest/docs/setup/getting-started/#download
  	- https://istio.io/latest/docs/setup/getting-started/#install
	- Traffic Management
    	- Gateways (ingress e egress)
    	- Load Balancing
    	- Timeout
    	- Retry Policies
    	- Circuit Breaker
    	- Fault Injection
  	- Observability
    	- Metrics
    	- Logs 
  	- Security
    	- Man-in-the-middles
    	- mTLS
    	- AAA (authentication, authorization and audit)
	
	- Istio Addons
    	- Deploy the Kiali dashboard, along with Prometheus, Grafana, and Jaeger.

# Istio Architecture
	- Sidecar Proxy: Envoy
  	- istioD controlls all the (Sidecar) Proxies
    	- Pilot
    	- Citadel
    	- Galley
  	- Data Plane: commmunication between Proxies
  	- Control Plane: Proxies configuration 

# Traffic Management
	- Ingress Gateway
    	- Receives incoming connections
    	- Ports management
	- Virtual Service
    	- Router
    	- Policies of Match, Retries, Fault Injection, Timeout, Subsets
  	- Destination Rule (Subset)
    	- Selector, types of load balancing, locality, circuit breaker
    	- 
# Commands
```
# Install Fortio to test
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.14/samples/httpbin/sample-client/fortio-deploy.yaml
export FORTIO_POD=$(kubectl get pods -l app=fortio -o 'jsonpath={.items[0].metadata.name}')

# Run example
kubectl apply -f deployment.yaml
kubectl label namespace default istio-injection=enabled

kubectl exec "$FORTIO_POD" -c fortio -- fortio load -c 2 -qps 0 -t 10s -loglevel Warning http://nginx-service:8500/

# Install addons
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.14/samples/addons/prometheus.yaml
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.14/samples/addons/kiali.yaml
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.14/samples/addons/jaeger.yaml
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.14/samples/addons/grafana.yaml
$ istioctl dashboard kiali

# Check istio namespace
kubectl get pods -n istio-system
kubectl get svc -n istio-system
```
