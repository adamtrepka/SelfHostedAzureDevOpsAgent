# Azure DevOps Agent
Poniższa instrukcja przedstawia w jaki sposób uruchomić agenta Azure DevOps w kontenerze Docker'owym.

## 0. Wymagania
- Ubuntu/Debian/Fedora etc.
- docker-ce https://docs.docker.com/engine/install/ubuntu/
- docker-compose https://docs.docker.com/compose/install/

## 1. Przygotowanie obrazu agenta

W pierwszej kolejności należy w katalogu `tools` wywołać polecenie:
```
sudo docker build -t dockeragent:latest .
```
spowoduje zbudowanie obrazu docker'owego zawierającego agenta Azure DevOps. Przygotowana konfiguracja została przygotowana na podstawie [dokumentacji](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops). 

W celu dodania brakujących pakietów np. python, netcore sdk należy zmodyfikować plik [Dockerfile](tools\AgentImage\Dockerfile) i ponownie wykonać powyższe polecenie.

## 2. Uruchomienie agenta

W pliku [docker-compose.yaml](tools\AgentImage\docker-compose.yaml) należy zmodyfikować sekcję `environment` uzupełniając wszystkie wymienione zmienne.
>* AZP_URL - Adres url do swojej instancji Azure DevOps np. https://dev.azure.com/xyz
>* AZP_TOKEN - Wygenerowany pesonal access tocken [Dokumentacja](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page)
>* AZP_AGENT_NAME - Nazwa agenta np. SelfHostedAgent
>* AZP_POOL - Nazwa własnej puli agentów [Dokumentacja](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues?view=azure-devops&tabs=yaml%2Cbrowser)

Po wprowadzeniu modyfikacji w katalogu `tools` należy wykonać polecenie:
```
sudo docker-compose up -d
```
Polecenie spowoduje uruchomienie kontenera z naszym agentem Azure DevOps


> W repozytorium zamieściłem również przykładowy [pipeline](azure-pipelines.yml) budujący obraz docker'owy z wykorzystaniem własnego agenta. Pliku projektu znajdują się w katalogu `src`.