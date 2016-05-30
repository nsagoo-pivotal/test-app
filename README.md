# A Cloud-Native App in GoLang

This is a concourse demo showing continuous deployment of a cloud-native app to Cloud Foundry using [Concourse](http://concourse.ci/), an open-source, continuous integration tool.

### Setup Concourse

[Setup & run Concourse](http://concourse.ci/installing.html) in a local VirtualBox VM using Vagrant 

```bash
vagrant init concourse/lite # creates ./Vagrantfile
```
```bash 
vagrant up
```

Visit `192.168.100.4:8080` in your local browser to see the pipeline; download & configure Concourse [CLI](http://concourse.ci/hello-world.html)

### Deploy Pipeline

Target your local VirtualBox Concourse
```bash 
fly -t lite login -c http://192.168.100.4:8080
```
Download the applicaiton to your local workspace
```bash
git clone https://github.com/nsagoo-pivotal/concourse-demo-dhs
```

Update the credentials.yaml file found in ci directory of the source code.

Create pipeline
```bash
fly -t lite set-pipeline -p hello-world -c ci/manifest.yaml --load-vars-from ci/credentials.yaml
```

Unpause pipeline
```bash
fly -t lite unpause-pipeline -p hello-world
```

Deploy Job
```bash
fly -t lite trigger-job --job hello-world/deploy
```
You can also select the "deploy" job from the browser and click on the "+" sign to start the job. 

### App Endpoints

`/`: a simple landing page displaying the index and uptime  
`/env`: displays environment variables  
`/exit`: instructs app to self-delete with status code 1  

### Build via dockerimage:

```bash
./build.sh
```

Assumes you have the go toolchain (with the ability to cross-compile to different platforms) and docker installed and pointing at your docker daemon.
