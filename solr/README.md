# Solr Helm Chart

This helm chart installs a solr and required zookeeper cluster into a running
kubernetes cluster.

## Dependencies

Solr has a dependency on the zookeeper incubator helm chart, to install the
dependency first ensure that you have the incubator repo added to helm:

`helm repo add incubator https://storage.googleapis.com/kubernetes-charts-incubator`

then run:

`helm dependency update`

to fetch the required zookeeper chart.

Solr can then be installed with:

`helm install .`

## Configuration Options

TODO: Document


## TLS Configuration

Generate SSL certificate for the installation:

`cfssl genkey ssl_config.json | cfssljson -bare server`

base64 Encode the CSR and apply into kubernetes as a CSR


```
cat <<EOF | ikubectl apply -f -
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: ${MY_CSR_NAME}
spec:
  groups:
  - system:authenticated
  request: $(cat server.csr | base64 | tr -d '\n')
EOF

```
Approve the CSR

`kubectl certificate approve ${MY_CSR_NAME}`


`kubectl get csr ${MY_CSR_NAME}`
