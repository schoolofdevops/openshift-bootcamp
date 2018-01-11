## Rolling updates with deployments

Update the version of the image in vote_deploy.yaml

File: vote_deploy.yaml
```
...
    app: vote
    spec:
      containers:
      - image: schoolofdevops/vote:movies

```

Apply Changes and monitor the rollout

```
oc apply -f vote-deploy.yaml
oc rollout status deployment/vote
```

## Rolling Back a Failed Update

Lets update the image to a tag which is non existant. We intentionally introduce this intentional error to fail fail the deployment.

File: vote_deploy.yaml
```
...
    app: vote
    spec:
      containers:
      - image: schoolofdevops/vote:movi

```

Do a new rollout and monitor

```
oc apply -f vote_deploy.yaml
oc rollout status deployment/vote
```

Also watch the pod status which might look like

```
vote-3040199436-sdq17   1/1       Running            0          9m
vote-4086029260-0vjjb   0/1       ErrImagePull       0          16s
vote-4086029260-zvgmd   0/1       ImagePullBackOff   0          15s
vote-rc-fsdsd               1/1       Running            0          27m
vote-rc-mcxs5               1/1       Running            0
```

To get the revision history and details  
```
oc rollout history deployment/vote
oc rollout history deployment/vote --revision=x
[replace x with the latest revision]
```

[Sample Output]

```
root@kube-01:~# oc rollout history deployment/vote
deployments "vote"
REVISION	CHANGE-CAUSE
1		oc scale deployment/vote --replicas=5
3		<none>
6		<none>
7		<none>

root@kube-01:~# oc rollout history deployment/vote --revision=7
deployments "vote" with revision #7
Pod Template:
  Labels:	app=vote
	env=dev
	pod-template-hash=4086029260
	role=ui
	stack=voting
	tier=front
  Containers:
   vote:
    Image:	schoolofdevops/vote:movi
    Port:	80/TCP
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>
```

To undo rollout,

```
oc rollout undo deployment/vote
```

or

```
oc rollout undo deployment/vote --to-revision=1
oc get rs
oc describe deployment vote
```
