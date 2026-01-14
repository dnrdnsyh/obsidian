```
stages:
 #- sonarqube-check
  - build
  - deploy
variables:
  DOCKER_DRIVER: overlay2
  REGISTRY: 10.1.50.170
  REGISTRY_BARU: 10.99.20.63
  SONAR_TOKEN: "sqp_7e2c6536a0a4ba4b8bb26d832b1469b60a061d68"
  SONAR_HOST_URL: "http://10.1.50.170:9000"
# sonarqube-check:
#   stage: sonarqube-check
#   image: 
#     name: sonarsource/sonar-scanner-cli:latest
#     entrypoint: [""]
#   variables:
#     SONAR_USER_HOME: "${CI_PROJECT_DIR}/.sonar"  # Defines the location of the analysis task cache
#     GIT_DEPTH: "0"  # Tells git to fetch all the branches of the project, required by the analysis task
#   cache:
#     key: "${CI_JOB_NAME}"
#     paths:
#       - .sonar/cache
#   script: 
#     - /opt/sonar-scanner/bin/sonar-scanner
#   allow_failure: true
#   rules:
#     - if: '$ROLLBACK == "true"'
#       when: never
#     - if: '$CI_COMMIT_BRANCH == "develop"'
#   tags:
#     - runner-104
#build-quarkus:
#  stage: build
#  image: maven:3.9-eclipse-temurin-21
#  script:
#    - mvn clean package -DskipTests
#    - ls -lh target
#    - ls -lh target/quarkus-app
#  artifacts:
#    paths:
#      - target/quarkus-app/
#    expire_in: 1h
#  tags:
#    - runner-billing-stg
service_build_staging:
  stage: build
  image: docker:latest
  # JANGAN ADA services: di sini!
  variables:
    DOCKER_TLS_CERTDIR: ""
    DOCKER_HOST: unix:///var/run/docker.sock
  script:
   # - cp env/.env_staging .env
    - echo ${HARBOR_PASSWORD_NEW} | docker login 10.99.20.63 -u ${HARBOR_USER_NEW} --password-stdin
    - docker build --network host -t 10.99.20.63/newap2t-staging/kafka-consume-billing:staging -f Dockerfile .
    - docker push 10.99.20.63/newap2t-staging/kafka-consume-billing:staging
  tags:
    - runner-billing-stg
  only:
    - master
service_deploy_staging:
  stage: deploy
  image: docker:latest
  variables:
    DOCKER_TLS_CERTDIR: ""
    DOCKER_HOST: unix:///var/run/docker.sock
  script:
    - scp kubernetes/kafka-consume-billing.yaml root@10.99.20.58:/root/ap2t
    - scp .env root@10.99.20.58:/root/ap2t/kafka-consume-billing/.env
    - ssh root@10.99.20.58 "kubectl -n staging create configmap kafka-consume-billing-module-env --from-env-file=/root/ap2t/kafka-consume-billing/.env --dry-run=client -o yaml | kubectl apply -f -"
    - ssh root@10.99.20.58 "kubectl apply -f /root/ap2t/kafka-consume-billing.yaml"
    - ssh root@10.99.20.58 "kubectl rollout restart deployment kafka-consume-billing -n staging"
   
  tags:
    - runner-20-64
  only:
    - master
```