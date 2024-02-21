## installation
```
helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace
```

>[!tip] Values unnecessary & outdated version

## Example ingress Resource:

[ingress_example](ingress_example.yml)

>[!tip] The selector must target a cluster ip service otherwise you will get 404.
>[example_clusterip_svc](example_clusterip_svc.yml)

## Add letsencrypt

ADD `cert-manager.io/cluster-issuer:` `yourclusterissuername` to the [ingress_example](ingress_example.yml) <--

