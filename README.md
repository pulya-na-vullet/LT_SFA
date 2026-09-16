После клонирования репозитория необходимо обновить хелм чарт командой:
```
git submodule update --init --recursive
git submodule update --rebase --remote
```

### HELM install:
```
helm upgrade --install -n ufr-ncins gateway                  ufr-chart/ -f values-gateway.yml
helm upgrade --install -n ufr-ncins virtualservice           ufr-chart/ -f values-virtualservice.yml

helm upgrade --install -n ufr-ncins ufr-truststore-jks       ufr-chart/ -f values-secret-ufr-trustca.yml
helm upgrade --install -n ufr-ncins alfabank-trustca-secret  ufr-chart/ -f values-secret-alfabank-trustca.yml

helm upgrade --install -n ufr-ncins ufr-eos-ul-ncins-settings                ufr-chart/ -f ufr-eos-ul-ncins-settings.yml
helm upgrade --install -n ufr-ncins ufr-eos-ul-ncins-core-api                ufr-chart/ -f ufr-eos-ul-ncins-core-api.yml
helm upgrade --install -n ufr-ncins ufr-eos-ul-ncins-module-info-api         ufr-chart/ -f ufr-eos-module-info-api.yml
helm upgrade --install -n ufr-ncins ufr-eos-ul-ncins-gateway                 ufr-chart/ -f spring-application-gateway.yml

helm upgrade --install -n ufr-ncins ufr-eos-ul-ncins-ui                 ufr-chart/ -f ufr-eos-ul-ncins-ui.yml
```

### HELM uninstall:
```shell
helm uninstall -n ufr-ncins ufr-eos-ul-ncins-settings
helm uninstall -n ufr-ncins ufr-eos-ul-ncins-core-api
helm uninstall -n ufr-ncins ufr-eos-module-info-api
helm uninstall -n ufr-ncins spring-application-gateway

helm uninstall -n ufr-ncins ufr-eos-ul-ncins-ui

helm uninstall -n ufr-ncins ufr-truststore-jks
helm uninstall -n ufr-ncins alfabank-trustca-secret
helm uninstall -n ufr-ncins gateway    
helm uninstall -n ufr-ncins virtualservice
```

### Пример команд для деплоймента с JWT токеном, как это делает платформа
```shell
# получить сертификат для доступа к кластеру, скопировать его в отдельный файл cert(пример для интеграционного стенда)
openssl s_client -connect eosulk8smint1.moscow.alfaintra.net:6443 -showcerts

# произвести деплоймент(пример для интеграционного стенда)
helm upgrade --kube-token <token> --kube-apiserver "https://eosulk8smint1.moscow.alfaintra.net:6443" --kube-ca-file <path-to-cert> --install -n ufr-ncins ufr-eos-ul-ncins-core-api ufr-chart/ -f ufr-eos-ul-ncins-core-api.yml

# если необходимо декодировать токен JWT токена
echo <token> | cut -d '.' -f 2 | base64 -d
```
