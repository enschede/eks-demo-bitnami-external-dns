### DNS Controller

De controller leest de annotations van services en ingresses uit. Kijkt naar deze annotation:

    external-dns.alpha.kubernetes.io/hostname: 
      sso.worldofciam.com, sso.worldofciam.eu, sso.worldofciam.nl, sso.iamenschede.eu, sso.marcenschede.nl

Deze domein gaan verwijzen naar de load balancer.

Opmerkingen:
- Werkt records bij in Route53
- Bij down gaan van een service of ingress kan het tot wel 30 seconden duren voordat een record in Route53 verwijderd is. Dit is relevant bij down brengen van het cluster. Zie ook opmering in code hieronder.

---
apiVersion: v1
kind: Namespace
metadata:
  name: bitnami-external-dns

    apiVersion: helm.toolkit.fluxcd.io/v2beta1
    kind: HelmRelease
    metadata:
      name: external-dns
      namespace: bitnami-external-dns
    spec:
      values:
        # Een korte interval zorgt voor een snelle verwijdering van records
        interval: 10s
