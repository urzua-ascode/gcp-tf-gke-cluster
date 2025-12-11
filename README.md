# GCP Terraform Lab: Kubernetes Engine (GKE)

This project provisions a Google Kubernetes Engine (GKE) cluster.

## Resources Created

1.  **VPC Network & Subnet**: Dedicated network for GKE.
2.  **GKE Cluster**: A zonal cluster.
3.  **Node Pool**: A managed node pool with `e2-medium` instances.
4.  **Service Account**: Identity for GKE nodes.

### Architecture Diagram

```mermaid
graph TD
    subgraph GCP [Google Cloud Platform]
        subgraph VPC [VPC: gke-vpc-network]
            subgraph Subnet [Subnet: gke-subnet (10.10.0.0/24)]
                
                subgraph ControlPlane [Control Plane]
                    master[GKE Master]
                end

                subgraph Workers [Worker Nodes]
                    pool[Node Pool: my-node-pool <br/> Machine: e2-medium <br/> Preemptible: true]
                end
                
                sa[Service Account: gke-node-sa]
                pool -.-> sa
                master --- pool
            end
        end
    end
```

## Usage

1.  **Authenticate**:
    ```bash
    gcloud auth application-default login
    ```

2.  **Initialize**:
    ```bash
    terraform init
    ```

3.  **Plan**:
    ```bash
    terraform plan -var="project_id=YOUR_PROJECT_ID"
    ```

4.  **Apply**:
    ```bash
    terraform apply -var="project_id=YOUR_PROJECT_ID"
    ```

5.  **Connect to Cluster**:
    ```bash
    gcloud container clusters get-credentials $(terraform output -raw kubernetes_cluster_name) --zone us-central1-a
    ```
