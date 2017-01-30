# leKube  ![Alt text](/lekube_small.jpg?raw=true "leKube") 
logentries.com daemon set for Kubernetes

## Get started
There are a few different moving parts in the configuration.  First, you need to go to logentries.com and generate a token for this installation.  By Log Sets in the upper left corner, there's a +Add New button.  Click that, add log.  The next screen gives you lots of different agent options.  You want the Docker icon, next to the "Platforms" section.  Click the whale, and it's time to sail.  Next, under Select Set, give your cluster logs a name like "Kubernetes Dev", then you'll notice the "Create Log Token" button light up.  Click that!  We're almost done.  You want to capture the token which is highlighted in green.  We're going to put that in our lekube.yaml file.     

Open the lekube.yaml file and replace the xxxxx value with your token.  You'll notice that the image in there is jsingerdumars/lekube:latest which will do the trick.  Note that the source for my Docker image came directly from https://github.com/rapid7/docker-logentries I made my own image so it was insulated from upstream changes and simpler to install.  You can simply create your own image and swap the entry in the lekube.yaml

## Installing the daemon set in your cluster (and a few tweaks)
We assume at this point you have an up and running Kubernetes cluster and access to kubectl.  Before installing the daemon set, there are a few things you might want to customize.  First, it is configured to run in the default namespace, and you may prefer to put it in its own.  Just change the line in the lekube.yaml file.  Lastly, you can make the service account name different too.  But whatever you choose needs to be consistent between the two yaml files.  

Step one: build the service account  
```kubectl create -f le-service.yaml```

Step two: install the daemon set  
```kubectl create -f lekube.yaml```  

You're done.  Now data should be flowing to your logentries account.  Just click the "Finish & View Log" button and you'll see logs appearing.  
