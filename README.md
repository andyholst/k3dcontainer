# K3dcontainer

## Get started

Here is an example of initiating the kubernetes cluster with help of the k3dcontainer. It's important to provide
docker socket volume to be to talk to the Kubernetes cluster container. Directories mounted and used for config map 
setups. Secret environment you want to create with the defined secret environment variables defined by the HOST
environment and used by the K8 manifest files. Volume directory containing the Kubernetes manifest yaml files to deploy
with right number ordering if they are dependent on each other.

### Reference

https://hub.docker.com/r/jsquadab/k3dcontainer/

### Example

```bash
docker run --net=host \
-v /var/run/docker.sock:/var/run/docker.sock \
-v "$(pwd)"/service:/service \
-v "$(pwd)"/security:/security \
-v "$(pwd)"/deployment:/deployment \
--env CLUSTER_NAME="jsquad" \
--env CLUSTER_API_PORT="6443" \
--env LOAD_BALANCER_PORTS="1080 80 443" \
--env SECRET_ENVIRONMENT="openbank-spring-secret" \
--env SECRETS_ENVIRONMENT_VARIABLES="MASTER_KEY=$MASTER_KEY ROOT_PASSWORD=$ROOT_PASSWORD \
OPENBANK_PASSWORD=$OPENBANK_PASSWORD SECURITY_PASSWORD=$SECURITY_PASSWORD" \
--env CREATE_CONFIG_MAPS_VOLUMES_FROM_FILES_COMMAND="kubectl create configmap ssl-volume \
--from-file=/service/src/test/resources/test/ssl/truststore && \
kubectl create configmap flyway-ddl-volume --from-file=/security/sql" \
--env DEPLOY_YAML_FILES_PATH="/deployment" jsquadab/k3dcontainer:latest
```