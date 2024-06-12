******************
Kubectl Cheatsheet
******************

:title: Kubectl Cheatsheet
:date: 2022-03-08
:category: Posts
:slug: kubectl-cheatsheet
:tags: kubernetes

Here are some useful common operations when using ``kubectl``.

Tail logs from a pod
####################
::

    kubectl logs -f <pod-name>

Get a list of a pod's env vars
##############################
Use the following to get a list of a pod's environment variables. Useful for
debugging certain issues.
::

    kubectl -n <namespace> exec <pod_name> -- env

Drop into a shell on a specific pod
###################################
Use the following to drop into a shell on a specific pod. This is useful to
explore the contents of a container or run a command.
::

    kubectl -n <namespace> exec <pod_name> --tty --stdin -- sh

Port Forward a pod to localhost
###############################
Use the following to port forward a pod to localhost. This will allow you to
interact with services like prometheus or alertmanager in the browser.
::

    kubectl -n <namespace> port-forward <pod_name> <local_port>:<pod_port>

Delete pods matching a grep/awk pattern
#######################################
This is a useful command to perform actions on a set of Pods whose names match 
a specific pattern. Often, one may need to restart all the replicas of a pod. 
This is a convenient way to do just that type of bulk operation with kubectl.
::

    kubectl get pods --no-headers=true | awk '/astrid/{print $1}' | xargs kubectl delete pods

Get all resources in a namespace
################################
This command is useful to get a listing of everything under a namespace. I've
used this in the past to track down hidden resources that may be contain sensitive
data or old resources that are no longer needed.
::

    kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found -n <namespace>
