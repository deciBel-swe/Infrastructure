## GitOps: The Modern Standard for IaC
GitOps isn't just about storing code in Git; it’s about using Git as the **authoritative source of truth** for your entire system state.

### The Problem with "Traditional" IaC
* **Manual Execution:** Running `terraform apply` or `kubectl apply` from a local laptop is hard to audit.
* **Configuration Drift:** Someone manually edits a resource in the cloud console, and the code in Git no longer matches reality.
* **Security Risk:** Every developer needs high-level credentials to the cluster to deploy.

### The GitOps Solution
GitOps treats **Infrastructure as Code** the same way we treat **Application Code**.
* **Single Source of Truth:** If it’s not in Git, it doesn’t exist in the cluster.
* **Immutable History:** Every change is a commit. Want to roll back? Just `git revert`.
* **Higher Security:** The CI/CD system (or an agent) has cluster access, not the individual developers.
* evolved not only to infrastructure but 
    - X as Code (XaC)
    - Network as Code (NaC), 
    - Policy as Code (PaC)
    - Configuration as Code (CaC)
    - Security as Code (SaC)
    - etc



---

## Push vs. Pull CD Models
| Feature | Push Model (e.g., Jenkins, GitLab CI) | Pull Model (e.g., ArgoCD, Flux) |
| :--- | :--- | :--- |
| **How it works** | CI tool "pushes" changes to the cluster. | An agent *inside* the cluster "pulls" changes. |
| **Security** | Cluster firewall must allow external traffic. | Cluster is closed; agent initiates outgoing requests. |
| **Drift** | Hard to detect if someone manually changes K8s. | Continuously syncs actual state to desired state. |

---

## ArgoCD: The Kubernetes GitOps Controller
ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes. It functions as a **Kubernetes Controller** that continuously monitors your Git repo.

### Core Mechanics
1.  **Deployment:** Installed inside your K8s cluster as a set of Custom Resource Definitions (CRDs).
2.  **State Management:** It compares the **Desired State** (Git) vs. the **Actual State** (Cluster).
3.  **Self-Healing:** If someone runs a manual `kubectl` command to change a replica count, ArgoCD detects the "Out of Sync" status and automatically reverts it to the Git version.

### Key Components
* **Application:** A CRD that connects a specific Git source to a specific K8s destination/namespace.
* **AppProject:** Provides a logical grouping of Applications.
* **Sync Policy:**
    * `automated.selfHeal`: Reverts manual cluster changes.
    * `automated.prune`: Deletes resources in K8s that were removed from Git.

### Supports
- K8s Yaml Files
- Helm Charts
- Kustomize

### ArgoCD as K8s extention:
- Uses existing K8s functionalities
    - using etcd to store data, 
    - using controllers for monitoring and comparing actual and desired state

- Benefit
    - Real-time updates of application state
---

## Best Practices: Separate CI from CD
One of the most important takeaways from your notes is the **separation of repositories**:

1.  **App Repo (CI):** Contains source code. On push, it runs tests, builds an image, and pushes to a Registry. It then updates the **Config Repo**.
2.  **Config Repo (CD):** Contains K8s Manifests/Helm charts. **ArgoCD watches only this repo.**

---