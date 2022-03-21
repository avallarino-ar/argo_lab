# Argo Workflows - Lab

Argo Workflows is an open source container-native workflow engine for orchestrating parallel jobs on Kubernetes. Argo Workflows is implemented as a Kubernetes CRD (Custom Resource Definition).  

+ Define workflows where each step in the workflow is a container.  
+ Model multi-step workflows as a sequence of tasks or capture the dependencies between tasks using a directed acyclic graph (DAG).  
+ Easily run compute intensive jobs for machine learning or data processing in a fraction of the time using Argo Workflows on Kubernetes.  
+ Run CI/CD pipelines natively on Kubernetes without configuring complex software development products.  

--- 


## Core concepts  


- **Workflow**: a Kubernetes resource defining the execution of one or more **template**. Workflows are named.
- **Template**: a **step**, **steps** or **dag**.
- **Step**: a single step of a **workflow**, typically run a container based on **inputs** and capture the **outputs**.
- **Steps**: a list of **steps**
- **Entrypoint**: the first **step** to execute when running a **workflow**
- **Node**: a step
- **Directed Acyclic Graph (DAG)**: a set of **steps** (nodes) and the dependencies (edges) between them.
- **Workflow Template**: a Kubernetes resource defining a reusable workflow for a namespace
- **Cluster Workflow Template**: a Kubernetes resource defining a reusable workflow for a cluster
- **Inputs**: **parameters** and **artifacts** passed to the **step**,
- **Outputs**: **parameters** and **artifacts** outputted by a **step**
- **Parameters**: objects, strings, booleans, arrays
- **Artifacts**: files saved by a container
- **Artifact Repository**: a place where **artifacts** are stored
- **Executor**: the method to execute a container, e.g. Docker, PNS ([learn more](https://argoproj.github.io/argo-workflows/workflow-executors/))
- **Workflow Service Account**: the service account that a workflow is executed as ([learn more](https://argoproj.github.io/argo-workflows/service-accounts/))

## Templates

- **Template Definitions**
    - CONTAINER
        - it will schedule a Container. The spec of the template is the same as the K8s container spec, so you can define a container here the same way you do anywhere else in K8s.
    - SCRIPT
        - A convenience wrapper around a container. The spec is the same as a container, but adds the source: field which allows you to define a script in-place. The script will be saved into a file and executed for you. The result of the script is automatically exported into an Argo variable either {{tasks.<NAME>.outputs.result}} or {{steps.<NAME>.outputs.result}}, depending how it was called.
    - RESOURCE
        - Performs operations on cluster Resources directly. It can be used to get, create, apply, delete, replace, or patch resources on your cluster.
    - SUSPEND
        - A suspend template will suspend execution, either for a duration or until it is resumed manually. Suspend templates can be resumed from the CLI (with argo resume), the API endpoint, or the UI

- **Template Invocators**
    - STEPS
        - A steps template allows you to define your tasks in a series of steps. The structure of the template is a "list of lists". Outer lists will run sequentially and inner lists will run in parallel. If you want to run inner lists one by one, use the Synchronization feature. You can set a wide array of options to control execution, such as when: clauses to conditionally execute a step.
    - DAG
        - A dag template allows you to define your tasks as a graph of dependencies. In a DAG, you list all your tasks and set which other tasks must complete before a particular task can begin. Tasks without any dependencies will be run immediately.

        
## Template tag
- **simple** (variable)
- **expression** (filter, map, etc.)

--- 

## Commands

## Minikube

[https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

`brew install minikube`

[https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos)


`minikube start --driver=<hypervisor>`

**hypervisors***: hyperkit, parallels, virtualbox, ssh*

## kubectl

- `brew install kubectl`
- `kubectl version --client`
- `kubectl cluster-info`


## Argo

[https://argoproj.github.io/argo-workflows/quick-start](https://argoproj.github.io/argo-workflows/quick-start)

- `kubectl create ns argo`
- `kubectl apply -n argo -f [https://raw.githubusercontent.com/argoproj/argo-workflows/master/manifests/quick-start-postgres.yaml](https://raw.githubusercontent.com/argoproj/argo-workflows/master/manifests/quick-start-postgres.yaml)`
- `kubectl -n argo get pods`
- `kubectl create clusterrolebinding avallarino-cluster-admin-binding --clusterrole=cluster-admin --user=abasharino@gmail.com`
- `kubectl -n argo port-forward deployment/argo-server 2746:2746`


## Argo CLI

[https://github.com/argoproj/argo-workflows/releases/tag/v3.2.9](https://github.com/argoproj/argo-workflows/releases/tag/v3.2.9)


## Submit a workflow

`argo submit -n argo --watch <file>`

HelloWorld

`argo submit -n argo --watch https://raw.githubusercontent.com/argoproj/argo-workflows/master/examples/hello-world.yaml`

List submited WF:  
`argo list -n argo`

Templates  
`argo submit -n argo --watch wf02-container-template.yml`

### Params

1. `argo submit -n argo --watch wf10-input-parameter-dag.yml`
2. `argo submit -n argo --watch wf10-input-parameter-dag.yml -p message1="param from terminal"`
3. `argo submit -n argo --watch wf10-input-parameter-dag.yml --parameter-file wf10-parameters.yml`