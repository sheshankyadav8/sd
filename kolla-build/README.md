# kolla-builder
[![Docker Repository on Quay](https://quay.io/repository/charter-os/kolla-build/status "Docker Repository on Quay")](https://quay.io/repository/charter-os/kolla-build)

# Overview
This Kolla build container can be used with the Jenkins build pipeline provided at [charter-ctec/ci-container-builds/kolla](https://github.com/charter-ctec/ci-container-builds/kolla). This container manifest is made public so that other organizations can build Docker artifacts of OpenStack code more easily, agnostic to preferred CI/CD tooling. The resulting artifacts can then be used either for [OpenStack-Helm](https://github.com/openstack/openstack-helm) (as in our perferred case), [Kolla-Ansible](https://github.com/openstack/kolla-ansible), or [Kolla-Kubernetes](https://github.com/openstack/kolla-kubernetes), and is intended to leverage either custom/modified vendor provided code (such as Mirantis OpenStack), or upstream OpenStack code. Ubuntu and CentOS are available for OS, and either source or binary can be selected at the time of build.

Build times average between 5-15 minutes because we are leveraging the docker.socket rather than DinD as we have before. Push times will obviously vary depending on throughput to your preferred registry (local or internet-based). We are building publicly accessible [images](https://quay.io/organization/charter-os) and OpenStack-Helm [application](https://quay.io/application/) Charts at quay.io.

# Instructions
Please have a look at the Dockerfile and the imported requirements. We're using the `docker.socket` to make the build process more streamlined. The variables should be used as in the following example below:

```
docker run -t --privileged \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name kolla-build-keystone \
  -v /tmp/kolla-build.conf:/etc/kolla/kolla-build.conf \
  -v /tmp/kolla/src/keystone:/tmp/kolla/src/keystone \
  -e KOLLA_BASE=ubuntu \
  -e KOLLA_TYPE=source \
  -e KOLLA_TAG=3.0.2-bd49 \
  -e KOLLA_PROJECT=keystone \
  -e KOLLA_NAMESPACE=charter-os \
  -e KOLLA_VERSION=3.0.2 \
  -e DOCKER_USER=user \
  -e DOCKER_PASS=pass \
  -e DOCKER_REGISTRY=quay.io \
quay.io/charter-os/kolla-build:v0.1.3
```

# Questions and Contributing
If you have any questions, or would like to contribute to either the build process (container image) or the Jenkins pipelines found at [charter-ctec/ci-container-builds/kolla](https://github.com/charter-ctec/ci-container-builds/kolla), we welcome your participation!

Charter DNA Team
