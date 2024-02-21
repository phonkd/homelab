***

**This thing:**
![](Pasted%20image%2020230914213932.png)

## Prerequisites

- [ ] [tls](let's%20encrypt%20setup.md)
- [ ] [ingress-nginx](ingress-nginx.md)

## Setup

### generating credentials:

1. Install `htpasswd` or `apache-tools`
2. Run:
```htpasswd
htpasswd -c ./auth USERNAMENAME
```

### Create k8s secret from htpasswd file

```bash
kubectl create secret generic basic-auth --from-file=auth -n YOUR_NAMESPACE
```
>[!tip]
>Make sure create it in the namespace of your service.
### Configure basic auth on ingress(add this to annotations)

```
# type of authentication
    nginx.ingress.kubernetes.io/auth-type: basic
    # name of the secret that contains the user/password definitions
    nginx.ingress.kubernetes.io/auth-secret: basic-auth
    # message to display with an appropriate context why the authentication is required
    nginx.ingress.kubernetes.io/auth-realm: 'Authentication Required - foo'
```

see [ingress_example](ingress_example.yml)

>[!warning]
>Change the secret name under `    nginx.ingress.kubernetes.io/auth-secret: basic-auth` to the name of your secret.


>[!tip] To change password just regenerate the secret (from-file) and its implemented instantly.