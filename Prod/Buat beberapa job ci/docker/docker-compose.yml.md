```
version: '3.8'

services:
  kafka-produce-cargo-transaksi-nontaglis:
    image: 10.1.50.170/newap2t-staging/kafka-produce-cargo:dev #image harbor
    environment:
      - TZ=Asia/Jakarta
      - JENISKAFKA=TRANSAKSI_NONTAGLIS #cara panggil
    volumes:
      - /log:/log
    networks:
      - kafka-produce-cargo-network
    deploy:
      replicas: 1

  kafka-produce-cargo-nontaglis-qa1:
    image: 10.1.50.170/newap2t-staging/kafka-produce-cargo:dev
    environment:
      - TZ=Asia/Jakarta
      - JENISKAFKA=TRANSAKSI_NONTAGLIS_QA1
    volumes:
      - /log:/log
    networks:
      - kafka-produce-cargo-network
    deploy:
      replicas: 1

  kafka-produce-cargo-transaksi-prepaid:
    image: 10.1.50.170/newap2t-staging/kafka-produce-cargo:dev
    environment:
      - TZ=Asia/Jakarta
      - JENISKAFKA=TRANSAKSI_PREPAID
    volumes:
      - /log:/log
    networks:
      - kafka-produce-cargo-network
    deploy:
      replicas: 1

  kafka-produce-cargo-lunas-qa1:
    image: 10.1.50.170/newap2t-staging/kafka-produce-cargo:dev
    environment:
      - TZ=Asia/Jakarta
      - JENISKAFKA=LUNAS_QA1
    volumes:
      - /log:/log
    networks:
      - kafka-produce-cargo-network
    deploy:
      replicas: 1

  kafka-produce-cargo-lunas-qas:
    image: 10.1.50.170/newap2t-staging/kafka-produce-cargo:dev
    environment:
      - TZ=Asia/Jakarta
      - JENISKAFKA=LUNAS_QAS
    volumes:
      - /log:/log
    networks:
      - kafka-produce-cargo-network
    deploy:
      replicas: 1

  kafka-produce-cargo-lunas-qab:
    image: 10.1.50.170/newap2t-staging/kafka-produce-cargo:dev
    environment:
      - TZ=Asia/Jakarta
      - JENISKAFKA=LUNAS_QAB
    volumes:
      - /log:/log
    networks:
      - kafka-produce-cargo-network
    deploy:
      replicas: 1      

networks:
  kafka-produce-cargo-network:
    driver: overlay
```