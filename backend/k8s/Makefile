.PHONY: toolbox-up toolbox-down cluster-up cluster-down shell run kubectl

# Starts the toolbox container in the background
toolbox-up:
	docker compose up -d --build

# Stops and removes the toolbox container
toolbox-down:
	docker compose down

# Uses the toolbox to create the K3s cluster based on your config
cluster-up: toolbox-up
	@echo "Spinning up the cluster..."
	docker exec k8s-toolbox k3d cluster create --config backend/k8s/k3d-config.yaml
	@echo "Cluster ready! Applying manifests..."
	docker exec k8s-toolbox kubectl apply -f backend/k8s/manifests/
	@echo "All resources deployed successfully."

# Uses the toolbox to delete the cluster
cluster-down:
	docker exec k8s-toolbox k3d cluster delete dev-cluster

# Opens an interactive terminal inside the toolbox for manual debugging
shell:
	docker exec -it k8s-toolbox bash

# Run arbitrary commands via: make run k3d cluster list
run:
	@docker exec -it k8s-toolbox $(CMD)

# Run kubectl commands via: make kubectl get pods
kubectl:
	@docker exec -it k8s-toolbox kubectl $(CMD)

restart: cluster-down toolbox-down cluster-up
