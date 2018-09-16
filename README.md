# gitlabci-node-deploy 
[![MicroBadger Size](https://img.shields.io/microbadger/image-size/alexanderlindner/gitlabci-node-deploy.svg?style=for-the-badge)](https://hub.docker.com/r/alexanderlindner/gitlabci-node-deploy/) [![Docker Build Status](https://img.shields.io/docker/build/alexanderlindner/gitlabci-node-deploy.svg?style=for-the-badge)](https://hub.docker.com/r/alexanderlindner/gitlabci-node-deploy/) [![MicroBadger Layers](https://img.shields.io/microbadger/layers/alexanderlindner/gitlabci-node-deploy.svg?style=for-the-badge)](https://hub.docker.com/r/alexanderlindner/gitlabci-node-deploy/) [![Docker Pulls](https://img.shields.io/docker/pulls/alexanderlindner/gitlabci-node-deploy.svg?style=for-the-badge)](https://hub.docker.com/r/alexanderlindner/gitlabci-node-deploy/)

This is a simple docker image for automatic build and deployment via ftp of node websites. I use it to automate my website using gitlab and it's ci. It contains node, npm, yarn and lftp.
## Example
.gitlab-ci.yml
```markdown
image: alexanderlindner/gitlabci-node-deploy
stages:
- build

services:

cache:
  paths:
  - node_modules/

build:
  stage: build
  script:
   - yarn
   - yarn run build
   - echo "set ssl:verify-certificate false" > ~/.lftprc
   - >
    cd dist && PASSWORD=$PASSWORD USERNAME=$USERNAME HOST=$HOST lftp -c " \
      set ftp:ssl-allow yes; \
      open -u $USERNAME,$PASSWORD $HOST; \
      mirror -pr static /static --parallel=10 \
      mirror -pr index.html /index.html \
       "
   - echo "finished"
  only:
    - master
 ```   
 The HOST, USERNAME and PASSWORD is provided via *Variables* tabs in the project setting. The build script is provided by [vue](https://cli.vuejs.org/) 
