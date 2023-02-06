# helm

#### Helm yaratishni o'rganamiz oddiy nginx uchun helm

Helm ni o'rganishdan oldin bizga kerakli narsalarni to'g'irlab olishimiz kerak.
+ k8s cluster
+ laptopimizga helm o'rnatilgan bo'lishi kerak. O'rnatish uchun 👉 [helm.sh](https://helm.sh/docs/intro/install/) ga bosing.

Birinchi navbatda tepadagi fayllarni to'g'irlab olamiz 

```
$ ls

nginx-chart  README.md
```
```
$ tree
.
├── nginx-chart
│   ├── charts
│   ├── Chart.yaml
│   ├── templates
│   │   ├── configmap.yaml
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   └── values.yaml
└── README.md
```
`Chart.yaml` faylimiz quyidagicha
```
apiVersion: v2
name: nginx-chart
description: My First Helm Chart
type: application
version: 0.1.0
appVersion: "1.2.0"
maintainers:
- email: abdulvoris000urolov@gmail.com
  name: abdulvoris
```
`values.yaml` faylimiz quyidagicha
```
replicaCount: 5

image:
  repository: nginx
  tag: "1.16.0"
  pullPolicy: IfNotPresent

service:
  name: nginx-service
  type: ClusterIP
  port: 80
  targetPort: 9000

env:
  name: dev
```

`configmap.yaml` faylimiz quyidagicha
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-index-html-configmap
  namespace: default
data:
  index.html: |
    <html>
    <h1>Welcome</h1>
    </br>
    <h1>Hi! I got deployed in {{ .Values.env.name }} Environment using Helm Chart </h1>
    </html
```
`deployment.yaml` faylimiz quyidagicha
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-nginx
  labels:
    app: nginx
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
```
`service.yaml` faylimiz quyidagicha
```
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-service
spec:
  selector:
    app.kubernetes.io/instance: {{ .Release.Name }}
  type: {{ .Values.service.type }}
  ports:
    - protocol: {{ .Values.service.protocol | default "TCP" }}
      port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.targetPort }}
```
va fayllarimiz to'g'ri yozilganligiga ishonch hosil qilish uchun quyidagi buyruqni yozamiz
```
helm lint nginx-chart nginx-chart
```
va siz qiymatlar almashayotganni ko'rish uchun quyidagicha tekshirib olsayiz bo'ladi
```
helm template nginx-chart nginx-chart
```

va nihoyat uni ishga tushurishimiz uchun
```
helm install nginx-chart nginx-chart
``` 
buyruqni tersak kifoya 
Agar sizda buni ishga tushurish bilan muammo bo'lsa menga yozing 👉 [Abdulvoris](https://github.com/Abdulvorisjs)