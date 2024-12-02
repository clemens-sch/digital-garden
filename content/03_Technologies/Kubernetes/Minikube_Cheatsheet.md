#Technologies 

---


### # Notes

Kubernetes vs. Docker 

- Pod = Container
- Deployment = Image

- LoadBalancer = Service, that distributes users equally between pods

---

### # Kubernetes

- Build an image
- `docker build -t kub-first-app .`
- Make sure cluster is running
- `minikube status`
- See list of available kubectl commands
- `kubectl help`
- Create deployment object
- `kubectl create deployment first-app --image=kub-first-app`
- Get list of created deployments and pods
- `kubectl get deployments`
- `kubectl get pods`
- This should not have worked. Images need to be pulled from registry by Kubernetes. You need to push your image first to docker hub, or use the trueberryless/kub-first-app image.
- First delete the previous deployment
- `kubectl delete deployment first-app`
- then create the new deployment
- `kubectl create deployment first-app --image=trueberryless/kub-first-app`
- `minikube dashboard`
- Expose a port
- `kubectl expose deployment first-app --type=LoadBalancer --port=8080`
- See services
- `kubectl get services`

---
### # Minikube

Local Kubernetes

- On a cloud infrastructure we would get an external IP. For minikube we need:
- `minikube service first-app`
- You can crash the app by going to the `/error` route. You will see the pod gets restarted automatically (taking longer and longer each time).
	- ![[Pasted image 20241129123901.png]]
- You can also scale your deployment
- `kubectl scale deployment/first-app --replicas=3`
	- ![[Pasted image 20241129124144.png]]
- Have a look at your pods again
- `kubectl get pods`
- You can update your deployment by running. Images will only be pulled if they have a different tag.
- `kubectl set image deployment/first-app kub-first-app=trueberryless/kub-first-app:v2`
- view the deployment status
- `kubectl rollout status deployment/first-app`
- Run a deployment with a non-existing image/tag.
- `kubectl set image deployment/first-app kub-first-app=trueberryless/kub-first-app:not-exist`
- Checking the status of your rollout you will see, the old pods won’t be terminated because the new ones can’t be started due to an image pulling error.
- Rollout to last deployment
- `kubectl rollout undo deployment/first-app`
- See deployment history
- `kubectl rollout history deployment/first-app`
- `kubectl rollout history deployment/first-app --revision=3`
- You can also rollback to a specific revision
- `kubectl rollout undo deployment/first-app --to-revision=1`


| Command                | Explanation                                                            | Use-Case                                     |
| ---------------------- | ---------------------------------------------------------------------- | -------------------------------------------- |
| minikube status        | Making sure, cluster is running                                        | --                                           |
| git commit -m          | Create new commit with message-flag                                    | --                                           |
| git branch newBranch   | Creates a new branch                                                   | Here: creates 'newBranch'                    |
| git checkout newBranch | Move to other branch                                                   | Here: moves to 'newBranch'                   |
| git merge newBranch    | Combines two branches                                                  | Here: combining 'newBranch' & **'main'**     |
| git rebase main        | Moves commits from current branch in other branch - new linear history | Here: combining **'newBranch'** & 'main'<br> |
|                        |                                                                        |                                              |
