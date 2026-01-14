```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kafka-consume-billing
  namespace: staging
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kafka-consume-billing
  template:
    metadata:
      labels:
        app: kafka-consume-billing
    spec:
      securityContext:
        fsGroup: 1000
      containers:
        - name: kafka-consume-billing
          image: '10.99.20.63/newap2t-staging/kafka-consume-billing:staging'
          imagePullPolicy: "Always"
          env:
            - name: TZ
              value: Asia/Jakarta
          ports:
            - containerPort: 8001
          volumeMounts:
            - name: uploads-volume
              mountPath: /app/uploads
            - name: log-volume
              mountPath: /log
      imagePullSecrets:
        - name: newap2t-stg-registry-secret
      restartPolicy: Always
      volumes:                 
        - name: uploads-volume
          hostPath:
            path: /mnt/nfs_billing
            type: Directory
#      persistentVolumeClaim:
#        claimName: module-module-claim0
        - name: log-volume
          hostPath:
            path: /log
            type: Directory
#      persistentVolumeClaim:
#        claimName: module-module-claim2
---
apiVersion: v1
kind: Service
metadata:
  name: kafka-consume-billing
  namespace: staging
spec:
  selector:
    app: kafka-consume-billing
  ports:
    - protocol: TCP
      port: 8008
      targetPort: 8008
      nodePort: 30999
  type: NodePort
```