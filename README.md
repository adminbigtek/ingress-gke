"# ingress-gke" 
# 1. tep by step for config file ext-full-config.yaml


# 2. internal ingress step by step for config file int-full-config.yaml
Cách thực hiện chi tiết:
Tạo chứng chỉ SSL tự quản lý:
Bạn có thể tạo chứng chỉ SSL tự quản lý bằng công cụ openssl:

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=app3-internal.mrcloud.vn/O=internal"
Điều này sẽ tạo ra hai file tls.crt và tls.key.

Tạo Kubernetes Secret:
Bạn sẽ tạo một Kubernetes Secret từ các file chứng chỉ đã tạo:
    kubectl create secret tls app3-tls --cert=tls.crt --key=tls.key
    
View value ssl form secret:
    kubectl get secret app3-tls -o yaml

Apply file yaml
    kubectl apply -f int-full-config.yaml 

testing domain ingress:
    curl -I -k https://app3-internal.mrcloud.vn   #phai them -k de bo qua chung chi ssl khong tin cay

(Option) cau hinh tin cay cho chung chi:

    sudo cp tls.crt /usr/local/share/ca-certificates/app3-internal.crt
    sudo update-ca-certificates

    test lại: curl -I https://app3-internal.mrcloud.vn












