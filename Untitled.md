```
stages:
  - build_development
  - build_staging
  - deploy_development
  - deploy_staging
variables:
  GIT_STRATEGY: clone
  GIT_DEPTH: 0
  DOCKER_DRIVER: overlay2
  REGISTRY: 10.1.50.170
Build Development:
  stage: build_development
  script:
    - docker login 10.1.50.170 -u ${HARBOR_USER} -p ${HARBOR_PASSWORD}
    - docker build --no-cache -t ${REGISTRY}/rtr/rtr-instansi-vertikal:development -f dockerfile/dockerfile-dev .
    - docker push ${REGISTRY}/rtr/rtr-instansi-vertikal:development
  rules:
    - if: '$ROLLBACK == "true"'
      when: never
    - if: '$CI_COMMIT_BRANCH == "development"'
  tags:
    - runner-20-51
service_build_staging:
  stage: build_staging
  image: docker:latest
  # JANGAN ADA services: di sini!
  variables:
    DOCKER_TLS_CERTDIR: ""
    DOCKER_HOST: unix:///var/run/docker.sock
  script:
    - docker login 10.99.20.63 -u ${HARBOR_USER_NEW} -p ${HARBOR_PASSWORD_NEW}
    - docker build --network host -t 10.99.20.63/newap2t-staging/rtr-instansi-vertikal:staging -f dockerfile/Dockerfile-staging .
    - docker push 10.99.20.63/newap2t-staging/rtr-instansi-vertikal:staging
  tags:
    - runner-20-51
  only:
    - staging
Deploy Development:
  stage: deploy_development
  script:
    - echo "Mulai Deploy RTR Instansi Vertikal"
	- scp env/.env root@10.99.248.77:/root/rtr/instansi-vertikal/.env
    - kubectl -n rtr-development create configmap rtr-instansi-vertikal --from-env-file=.env --dry-run=client -o yaml | kubectl apply -f -
    - kubectl apply -f kubernetes/rtr-instansi-vertikal-dev.yaml
    - kubectl replace --force -f kubernetes/rtr-instansi-vertikal-dev.yaml
    - echo "Deploy Development Selesai"
  rules:
    - if: '$ROLLBACK == "true"'
      when: never
    - if: '$CI_COMMIT_BRANCH == "development"'
  tags:
    - runner-master
  allow_failure: false
  variables:
    GIT_STRATEGY: clone
service_deploy_staging:
  stage: deploy_staging
  image: docker:latest
  variables:
    DOCKER_TLS_CERTDIR: ""
    DOCKER_HOST: unix:///var/run/docker.sock
  script:
    - scp env/.env-staging root@10.99.20.58:/root/rtr/instansi-vertikal/.env
    - ssh root@10.99.20.58 "kubectl -n rtr create configmap instansi-vertikal-env --from-env-file=/root/rtr/instansi-vertikal/.env --dry-run=client -o yaml | kubectl apply -f -"
    - scp kubernetes/rtr-instansi-vertikal-staging.yaml root@10.99.20.58:/root/rtr
    - ssh root@10.99.20.58 "kubectl apply -f /root/rtr/rtr-instansi-vertikal-staging.yaml"
    - ssh root@10.99.20.58 "kubectl rollout restart deployment rtr-instansi-vertikal-staging -n rtr"
  tags:
    - runner-20-64
  only:
    - staging
```