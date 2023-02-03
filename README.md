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
va fayllarimiz to'g'ri yozilganligiga ishonch hozil qilish uchun quyidagi buyruqni yozamiz
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