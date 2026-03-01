# AppInsigths - Instrumentação

## ResourceName
   MonitoriaRGAz204
    
## CHAVE DE INSTRUMENTAÇÃO
cf96a26a-76f8-4347-bbe0-0a5b231e7e92

## CADEIA DE CONEXÃO
InstrumentationKey=cf96a26a-76f8-4347-bbe0-0a5b231e7e92;IngestionEndpoint=https://eastus-8.in.applicationinsights.azure.com/;LiveEndpoint=https://eastus.livediagnostics.monitor.azure.com/;ApplicationId=6447cad4-dddc-4720-b5e6-779681ef7a72

## TRECHO PARA INSERIR NO APPSETTINGS.JSON

    "ApplicationInsights": {
        "InstrumentationKey": "cf96a26a-76f8-4347-bbe0-0a5b231e7e92",
        "IngestionEndpoint": "https://eastus-8.in.applicationinsights.azure.com/",
        "LiveEndpoint": "https://eastus.livediagnostics.monitor.azure.com/",
        "ApplicationId": "6447cad4-dddc-4720-b5e6-779681ef7a72"
    },

# Sequência de comandos para upload do projeto ao arquivo markdown.

### Os comandos incluem:

### 1.Login no Azure - 
    az login
### 2.Configurar assinatura - 
    az account set
### 3.Upload para App Service - usando 
    az webapp deployment source config-zip
### 4.Alternativa para Azure Functions - usando 
    az functionapp deployment source config-zip
### 5.Verificar credenciais de deployment - para monitorar o status
Você só precisa substituir os placeholders:

    <seu-resource-group> → Nome do seu resource group
    <seu-app-service-name> ou <seu-function-app-name> → Nome do seu serviço no Azure

O caminho do arquivo ZIP já está configurado corretamente:
    C:\Users\Adriano Santana\OneDrive\AZURE\AZ-204\Exercicios\AppInsightsAz204\bin\Release\net9.0\win-x86\publish\api.zip
