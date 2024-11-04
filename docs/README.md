This guide is intended to include common troubleshooting steps and solutions for problems encoutered.

If nothing in here helps please don't hesitate to reach out to the class support (lecturer and/or demonstrators).

* Table of Contents
{:toc}

# Kubernetes (Rancher) Deployment Issues

## No services (Sorry no matching options) when adding an ingress rule

The service has not been created as part of the workload creation. Have a look at the practical where we setup workloads (top of page 4) and make sure you add a Cluster IP networking service on any workload you wish to expose via ingress.

It's possible but complex to add this after the fact - your best bet would be to delete the workload and re-create it with the networking step included.

## Deployment does not meet minimum availability

Please note this is a completely *generic* error i.e. it is just saying _something has happened_ and we can't meet your target (you've asked for X instances, we can't provide that).

Clicking into the workload will hopefully provide more detail such as ImagePullBackoff or CrashLoopBackoff and you can also view the logs to see the more specific problem.

## CrashLoopBackoff

The container is crashing on startup. You can view the logs within the container that has just failed to hopefully see some output.

Note: a common cause of this is containers built on a Mac/Windows machine with an ARM chip (i.e. Apple Silicon M series) without the --platform flag; it builds for ARM and can't execute on an x64 chip, see the instructions on Canvas.

## ImagePullBackoff

The container image can't be pulled. Most commonly this is an authentication issue (the credentials for the registry secret are wrong; did you change your password?) or perhaps the image is badly named.

Check gitlab and see what you can find, and re-create the secret if needed.

# Docker Issues

## When trying to build on a mac see an error that has something to do with osxkeychain

Check your config.json:
```cat ~/.docker/config.json```

If you see the following entry ```"credsStore": "osxkeychain"```
Change it to 
```"credStore" : "desktop"```
Just use nano to do this:
```nano ~/.docker/config``` and then do Control+X and press Y to save and exit.

Note if this keeps happening then after changing the store try: ```docker logout``` to remove any cached Dockerhub credentials.

## Invalid Manifest

You can see an error in the container that shows a manifest error and possibly some crazy creation dates similar to the below:

![Example of manifest error in gitlab container page](/assets/images/invalid-tag-manifest.png)

Try building with ```--provenance false``` in your build command.

## Docker build generates an error failed to resolve source metadata with a failed to authorise 401

You may have a stale connection to Dockerhub. In a terminal run:

```docker logout```

And try again.

## Docker login gives an error about storing credentials "pass not initialised" (in WSL or Linux most likely)

Initialise the pass store: ```pass init```
