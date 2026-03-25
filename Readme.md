1)

a) ετρεξα την εντολη kubectl apply -f ass3548.yaml και για επαληθευση των οριων της ραμ και μετα αυτην kubectl describe deployment hello για να δω Limits cpu: 200m, memory: 256Mi
b)kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml ετρεξα αυτο για να τρεξει ο μετριξ σερβερ 

kubectl patch deployment metrics-server -n kube-system --type='json' -p='[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value": "--kubelet-insecure-tls"}]' και μετα αυτη για να δουλεψει τοπικα 

και μετα αυτη για να τρεχουν 100 συνδεσεις  μαζι και η καθε μια  διαρκειας 30 δευτερολεπτων 
kubectl run fortio --rm -i --tty --image=fortio/fortio -- load -qps 0 -t 30s -c 100 http://hello:8080/hello

επισης το scaling σταματησε στα 8 containers οπως οριστηκε απο το maxReplicas


2)

a)helm create hello-chart και μεσα στο φακελο templates δημιουργησα τα αρχεια deployment.yaml service.yaml και hpa.yaml ενω στο values.yaml που ειναι κεντρικο οριστικαν οι προεπιλεγμενες τιμες 
b)περιεχομενο values.yaml : 
message: "Hello world!"
endpoint: "/hello"
cpuLimit: ""
maxReplicas: 10

και παραμετροι του χελμ :

MESSAGE
Endpoint
Resources
Max Replicas

c) εντολη εγκαταστασης goodbye : helm install goodbye ./hello-chart --set message="Goodbye world!" --set endpoint="/goodbye" --set cpuLimit="250m" --set maxReplicas=20

d) kubectl describe deployment goodbye-deploy ελεγχος μηνυματος και cpu 25% 
kubectl get hpa goodbye-hpa ελεγχος οριου scaling 20 pods 

kubectl get svc goodbye-svc


3)
ετρεξα τις εντολες
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install my-ingress ingress-nginx/ingress-nginx

και αυτο που χρειαστηκε να κανω ειναι να παω στο yaml και να βαλω αυτο 
  ingressClassName: nginx  
