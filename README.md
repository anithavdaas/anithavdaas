stages:
  - build
  - test
  - deploy

variables:
  IMAGE_NAME: registry.gitlab.com/my-project/my-app

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $IMAGE_NAME:$CI_COMMIT_SHA .
    - docker push $IMAGE_NAME:$CI_COMMIT_SHA

test:
  stage: test
  image: node:18
  script:
    - npm install
    - npm test

deploy:
  stage: deploy
  image: alpine
  script:
    - echo "Deploying application..."
    - docker pull $IMAGE_NAME:$CI_COMMIT_SHA
    - docker run -d -p 80:3000 $IMAGE_NAME:$CI_COMMIT_SHA
  only:
    - main 
--->
