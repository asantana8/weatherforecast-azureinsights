# SimpleApi - Application Insights Demo

Uma API ASP.NET Core simples desenvolvida como exercício da certificação **AZ-204** (Azure Developer Associate) para demonstrar a integração e uso do **Azure Application Insights**.

## 📋 Descrição

Este projeto é uma aplicação web API que expõe um endpoint de previsão meteorológica e está totalmente integrado com o Azure Application Insights para monitoramento, telemetria e análise de performance em tempo real.

## 🛠️ Tecnologias Utilizadas

- **Framework:** ASP.NET Core 9.0
- **Linguagem:** C#
- **Monitoramento:** Azure Application Insights
- **Documentação API:** Swagger/OpenAPI
- **SDK de Telemetria:** Microsoft.ApplicationInsights
- **Contadores de Performance:** Application Insights Performance Counter Collector

## 📦 Pacotes NuGet

| Pacote | Versão | Propósito |
|--------|--------|----------|
| Microsoft.ApplicationInsights | 2.23.0 | SDK principal de telemetria |
| Microsoft.ApplicationInsights.AspNetCore | 2.22.0 | Integração ASP.NET Core |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.23.0 | Coleta de contadores de performance |
| Microsoft.AspNetCore.OpenApi | 9.0.12 | Suporte OpenAPI |
| Swashbuckle.AspNetCore | 6.5.0 | Geração da interface Swagger |

## 🚀 Como Executar

### Pré-requisitos

- .NET 9.0 SDK instalado
- Visual Studio Code ou Visual Studio 2022
- Conexão com a internet (para Application Insights)

### Pasos para Executar

1. **Clone ou abra o projeto:**
   ```bash
   cd c:\Users\Adriano Santana\OneDrive\AZURE\AZ-204\Exercicios\AppInsightsAz204
   ```

2. **Restaure as dependências:**
   ```bash
   dotnet restore
   ```

3. **Execute a aplicação:**
   ```bash
   dotnet run
   ```

4. A API estará disponível em: `http://localhost:5178`

## 🌐 Endpoints Disponíveis

### GET /weatherforecast

Retorna uma previsão meteorológica com 5 dias de antecedência.

**URL:** 
```
GET http://localhost:5178/weatherforecast/
```

**Headers:**
```
Accept: application/json
```

**Exemplo de Resposta:**
```json
[
  {
    "date": "2026-03-02",
    "temperatureC": 25,
    "summary": "Warm",
    "temperatureF": 77
  },
  {
    "date": "2026-03-03",
    "temperatureC": 18,
    "summary": "Cool",
    "temperatureF": 64
  }
]
```

## 📊 Application Insights

### Configuração

A configuração do Application Insights está definida em `appsettings.json`:

```json
{
  "ApplicationInsights": {
    "InstrumentationKey": "cf96a26a-76f8-4347-bbe0-0a5b231e7e92",
    "IngestionEndpoint": "https://eastus-8.in.applicationinsights.azure.com/",
    "LiveEndpoint": "https://eastus.livediagnostics.monitor.azure.com/",
    "ApplicationId": "6447cad4-dddc-4720-b5e6-779681ef7a72"
  }
}
```

### Telemetria Coletada

O Application Insights coleta automaticamente:
- **Requisições HTTP:** método, URL, status code, tempo de resposta
- **Exceções:** stack traces e contexto
- **Contadores de Performance:** CPU, memória, requisições por segundo
- **Dependências:** chamadas a serviços externos
- **Rastreamento Distribuído:** correlation IDs para rastreamento de transações

### Acessar Dados no Application Insights

1. Acesse o [Portal Azure](https://portal.azure.com)
2. Localize o recurso Application Insights associado
3. Use o **Analytics** para executar queries KQL
4. Consulte o arquivo `KQL-Application-Insights.md` para exemplos de queries

## 📁 Estrutura do Projeto

```
AppInsightsAz204/
├── Program.cs                          # Arquivo principal da aplicação
├── SimpleApi.csproj                    # Arquivo de projeto
├── SimpleApi.http                      # Requisições HTTP para teste
├── appsettings.json                    # Configurações (inclui App Insights)
├── appsettings.Development.json        # Configurações de desenvolvimento
├── README.md                           # Este arquivo
├── KQL-Application-Insights.md         # Exemplos de queries KQL
├── CORRECOES_APPINSIGHTS.md           # Correções implementadas
├── RESUMO_CORRECOES.md                # Resumo das mudanças
├── Dados conecção AppInsigths.md      # Dados de conexão
└── bin/                                # Arquivos compilados
    └── Debug/net9.0/                   # Build de Debug
    └── Release/net9.0/                 # Build de Release
```

## 🔧 Configuração do Development

### Launch Settings

Configurado em `Properties/launchSettings.json`:
- **URL Base:** http://localhost:5178
- **HTTPS desabilitado** para facilitar testes
- **Swagger habilitado** em desenvolvimento

### Executar com Swagger

Após iniciar a aplicação, acesse:
```
http://localhost:5178/swagger/ui/
```

## 📚 Documentação Adicional

| Arquivo | Descrição |
|---------|-----------|
| `KQL-Application-Insights.md` | Queries KQL para análise de dados |
| `CORRECOES_APPINSIGHTS.md` | Detalhes das correções implementadas |
| `RESUMO_CORRECOES.md` | Resumo executivo das alterações |
| `Dados conecção AppInsigths.md` | Credenciais e endpoints |

## 🎓 Contexto Educacional

Este projeto faz parte dos exercícios de preparação para a certificação **AZ-204: Developing Solutions for Microsoft Azure**, especificamente focado em:

- ✅ Integração de Azure Application Insights
- ✅ Coleta de telemetria
- ✅ Análise de logs com KQL (Kusto Query Language)
- ✅ Monitoramento de performance
- ✅ Rastreamento de requisições

## 🔍 Testar a API

### Usando REST Client (VS Code)

Abra o arquivo `SimpleApi.http` e clique em "Send Request"

### Usando curl

```bash
curl -X GET "http://localhost:5178/weatherforecast/" \
     -H "Accept: application/json"
```

### Usando PowerShell

```powershell
Invoke-RestMethod -Uri "http://localhost:5178/weatherforecast/" `
                  -Method Get `
                  -Headers @{"Accept"="application/json"}
```

## 📝 Logging e Diagnóstico

A aplicação está configurada para registrar:

- **Default:** Nível `Information`
- **Microsoft.AspNetCore:** Nível `Warning`

Todos os logs são enviados para o Application Insights em tempo real.

## 🤝 Contribuições

Este é um projeto educacional. Melhorias e sugestões são bem-vindas!

## 📄 Licença

Projeto educacional para fins de aprendizado - AZ-204

---

**Última atualização:** Março de 2026
