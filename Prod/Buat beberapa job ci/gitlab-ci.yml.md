```
stages:
  - build
  - deploy

variables:
  REGISTRY: 10.1.50.170
  REGISTRY_NEW: 10.99.20.63
  SONAR_TOKEN: "sqp_dbca71389a642b07b60afcbff9e83470ba060021"
  SONAR_HOST_URL: "http://10.1.50.170:9000"
  SERVICE_NAME: kafka-produce-cargo

service_build:
  stage: build
  rules:
    - if: '$CI_COMMIT_MESSAGE =~ /\[build-skip\]/'
      when: never
    - if: '$CI_COMMIT_BRANCH == "master"'
      when: on_success  
  script:
    - docker login 10.1.50.170 -u ${HARBOR_USER} -p ${HARBOR_PASSWORD}
    - docker build -t ${REGISTRY}/newap2t-staging/kafka-produce-cargo:dev -f docker/Dockerfile .
    - docker push ${REGISTRY}/newap2t-staging/kafka-produce-cargo:dev
  tags:
    - runner-163

staging_build:
  stage: build
  rules:
    - if: '$CI_COMMIT_MESSAGE =~ /\[build-skip\]/'
      when: never
    - if: '$CI_COMMIT_BRANCH == "staging"'
      when: on_success
  script:
    - docker login ${REGISTRY_NEW} -u ${HARBOR_USER_NEW} -p ${HARBOR_PASSWORD_NEW}
    - docker build -t ${REGISTRY_NEW}/new-ap2t/kafka-produce-cargo:staging -f docker/Dockerfile-staging .
    - docker push ${REGISTRY_NEW}/new-ap2t/kafka-produce-cargo:staging

  tags:
    - runner-billing-stg


service_deploy:
  stage: deploy
  script:
    - docker stack rm kafka-produce-cargo || true
    - scp .env root@10.1.51.104:/root/ap2t/kafka-produce-cargo
    - scp docker/docker-compose.yml root@10.1.51.104:/root/ap2t/kafka-produce-cargo/
    - ssh root@10.1.51.104 docker stack deploy -c /root/ap2t/kafka-produce-cargo/docker-compose.yml kafka-produce-cargo
    - ssh root@10.1.51.104 docker stack services kafka-produce-cargo

  tags:
    - runner-163
  only:
    - master


deploy_staging:
 stage: deploy
 script:
    - ssh -o StrictHostKeyChecking=no root@10.99.20.58 "mkdir -p /root/ap2t/${SERVICE_NAME}"  
    - scp -o StrictHostKeyChecking=no kubernetes/deployment-staging.yaml root@10.99.20.58:/root/ap2t/${SERVICE_NAME}/  
    - scp -o StrictHostKeyChecking=no env/.env-staging root@10.99.20.58:/root/ap2t/${SERVICE_NAME}/.env
    - ssh root@10.99.20.58 "kubectl -n ap2t create configmap ${SERVICE_NAME}-env --from-env-file=/root/ap2t/${SERVICE_NAME}/.env --dry-run=client -o yaml | kubectl apply -f -"
    - ssh root@10.99.20.58 "kubectl replace --force -f /root/ap2t/${SERVICE_NAME}/deployment-staging.yaml" 

 rules:
   - if: '$ROLLBACK == "true"'
     when: never
   - if: '$CI_COMMIT_BRANCH == "staging"'
 tags:
   - runner-20-64

```