# tmp

{
  "schemaVersion": 2,
  "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
  "config": {
    "mediaType": "application/vnd.docker.container.image.v1+json",
    "size": 233,
    "digest": "sha256:1db355b3b368170ad1f033a148c1d75956f9c30255513f9303d43cdb9e5a56ba"
  },
  "layers": [
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 1086,
      "digest": "sha256:b9b5067ea14ba719106dae9119bf53d63b2647b4b267c89ae4ec1f55b853ea11"
    }
  ],
  "annotations": {
    "org.opencontainers.image.created": "2022-12-21T11:03:18Z",
    "org.opencontainers.image.revision": "6.3.0/e2e85a960447a56a1fa45747d2275abd28c13870",
    "org.opencontainers.image.source": "https://github.com/stefanprodan/podinfo"
  }
}

apiVersion: notification.toolkit.fluxcd.io/v1beta2
kind: Alert
metadata:
  name: example-alert
  namespace: example
spec:
  summary: "blah blah"
  providerRef:
    name: slack
  eventSeverity: info
  eventSources:
    - kind: GitRepository
      name: '*'
    - kind: Kustomization
      name: '*'

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- alert.yaml

transformers:
- |-
  apiVersion: builtin
  kind: PrefixSuffixTransformer
  metadata:
    name: alert-summary-prefixer
  prefix: vtenant8.example.com
  fieldSpecs:
  - path: spec/summary
