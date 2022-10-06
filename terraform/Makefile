ssh:
	set -e ;\
	# Variables gained via terraform output
	REGION=$$(terraform output --json | jq -r ".region | .value") ;\
	INSTANCE=$$(terraform output --json | jq -r ".instances | .value | .$(host) | .id") ;\
	INSTANCE_IP=$$(terraform output --json | jq -r ".instances | .value | .$(host) | .ip") ;\
	BASTION=$$(terraform output --json | jq -r ".instances | .value | .$(host) | .bastion") ;\
	SESSION="$(session)" ;\
	if [ -z "$$SESSION" ]; then SESSION=$$(oci bastion session create-managed-ssh --session-ttl 10800 --bastion-id $$BASTION --display-name $$USER --ssh-public-key-file ~/.ssh/id_rsa.pub --key-type PUB --target-resource-id $$INSTANCE --target-os-username ubuntu | jq -r '.data | .id'); sleep 100; else echo "Using session from input"; fi ;\
	echo "Used session id: $$SESSION";\
	ssh -J $$SESSION@host.bastion.$$REGION.oci.oraclecloud.com ubuntu@$$INSTANCE_IP
