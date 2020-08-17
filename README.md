- [LinkedIn](https://www.linkedin.com/in/mourik)
- [Github](https://www.github.com/bchm)
- [Docker Hub](https://hub.docker.com/u/bastiaansd)

## Docker adventure

I had some trouble in setting up Docker images. I will share the mistakes I made while experimenting and what I learned from it.



I'm following the video from [That Devops Guy to set up a docker directory](https://www.youtube.com/watch?v=d1ZMnV4yM1U). Here are my findings:

- My first experimentation was to use the a Debian Buster Slim image and not a Alpine Linux image because [this website](https://pythonspeed.com/articles/alpine-docker-python/) says that 
- the latest SLIM Buster doesn't seem to work because 


alpine image is too heavy because of .... source: 

finally the heavy buster image worked perfectly. I still seem to have issues with the cgroups. But that is temporary solved by running the two commands from this github issue comment: https://github.com/microsoft/WSL/issues/4189#issuecomment-518277265

When I turned on the Pod load balancer the external IP was constantly empty, localhost must be there.  and localhost could not be reached so I had to fix that by ......

[...](https://stackoverflow.com/questions/44519980/assign-external-ip-to-a-kubernetes-service)