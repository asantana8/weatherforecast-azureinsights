# 🔐 Configuração Segura - Variáveis de Ambiente

## Visão Geral

Este projeto foi configurado para usar **variáveis de ambiente** em vez de manter credenciais sensíveis nos arquivos de configuração. Isso garante que chaves de API, instrumentation keys e outras informações sensíveis nunca sejam commitadas no GitHub.

## 📋 Arquivos de Configuração

### `appsettings.json` (Versionado no Git)
- Contém **placeholders** para variáveis de ambiente
- **NÃO** contém valores reais de credenciais
- Seguro para compartilhar publicamente

### `.env` (IGNORADO no Git)
- Contém os **valores reais** das variáveis de ambiente
- **NÃO** deve ser commitado
- Incluso no `.gitignore`
- Use em desenvolvimento local

### `.env.example` (Versionado no Git)
- Template mostrando a **estrutura** necessária
- Guia para novos desenvolvedores
- Sem valores sensíveis

### `appsettings.example.json` (Versionado no Git)
- Exemplo da estrutura do arquivo de configuração
- Contém valores fictícios ou reais que podem ser públicos

## 🚀 Como Configurar

### 1. Para Desenvolvimento Local

```bash
# Clone o repositório
git clone https://github.com/asantana8/weatherforecast-azureinsights.git
cd weatherforecast-azureinsights

# Crie um arquivo .env local (copie de .env.example)
cp .env.example .env

# Edite o arquivo .env com seus valores reais
# Use seu editor favorito (VS Code, Notepad++, etc)
```

### 2. Adicione Suas Credenciais no `.env`

```bash
APPLICATIONINSIGHTS__INSTRUMENTATIONKEY=sua-chave-aqui
APPLICATIONINSIGHTS__INGESTIONENDPOINT=https://eastus-8.in.applicationinsights.azure.com/
APPLICATIONINSIGHTS__LIVEENDPOINT=https://eastus.livediagnostics.monitor.azure.com/
APPLICATIONINSIGHTS__APPLICATIONID=seu-app-id-aqui
```

### 3. Execute a Aplicação

```bash
# Restaure as dependências
dotnet restore

# Execute
dotnet run
```

A aplicação carregará automaticamente as variáveis de ambiente do arquivo `.env`.

## 🔑 Variáveis de Ambiente Suportadas

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `APPLICATIONINSIGHTS__INSTRUMENTATIONKEY` | Chave de instrumentação do App Insights | `cf96a26a-76f8-4347-bbe0-0a5b231e7e92` |
| `APPLICATIONINSIGHTS__INGESTIONENDPOINT` | URL de ingestão de dados | `https://eastus-8.in.applicationinsights.azure.com/` |
| `APPLICATIONINSIGHTS__LIVEENDPOINT` | URL de live diagnostics | `https://eastus.livediagnostics.monitor.azure.com/` |
| `APPLICATIONINSIGHTS__APPLICATIONID` | ID da aplicação | `6447cad4-dddc-4720-b5e6-779681ef7a72` |

## 🔄 Como Funcionam as Variáveis de Ambiente

1. **Em Desenvolvimento (.env):**
   - O arquivo `Program.cs` lê o arquivo `.env`
   - Define as variáveis de ambiente automaticamente
   - O `appsettings.json` usa essas variáveis (placeholder: `${VAR_NAME}`)

2. **Em Produção (Azure):**
   - Use Azure Key Vault ou Application Settings
   - Defina as environment variables no App Service
   - O ASP.NET Core carrega automaticamente

## 📝 Exemplo de Arquivo `.env`

```bash
# Application Insights Configuration
APPLICATIONINSIGHTS__INSTRUMENTATIONKEY=cf96a26a-76f8-4347-bbe0-0a5b231e7e92
APPLICATIONINSIGHTS__INGESTIONENDPOINT=https://eastus-8.in.applicationinsights.azure.com/
APPLICATIONINSIGHTS__LIVEENDPOINT=https://eastus.livediagnostics.monitor.azure.com/
APPLICATIONINSIGHTS__APPLICATIONID=6447cad4-dddc-4720-b5e6-779681ef7a72
```

## ⚠️ Segurança

### ✅ O Que Fazer

- ✅ Adicione `.env` ao `.gitignore` (já adicionado)
- ✅ Nunca commit credenciais reais
- ✅ Use `.env.example` como template
- ✅ Documente quais variáveis são necessárias
- ✅ Revise o `.gitignore` antes de fazer push

### ❌ O Que NÃO Fazer

- ❌ Nunca include `.env` no Git
- ❌ Não commit `appsettings.Development.json` com valores reais
- ❌ Não coloque credenciais em commits
- ❌ Não compartilhe suas chaves pessoais

## 🔍 Verificar Arquivos Antes do Commit

```bash
# Verifique arquivos staged
git diff --cached

# Verifique se há chaves sensíveis
git grep "cf96a26a-76f8-4347-bbe0-0a5b231e7e92" # Deve estar vazio

# Verifique .gitignore
cat .gitignore
```

## 🚢 Deploy em Azure

### Azure App Service

1. Acesse o Portal Azure
2. Vá para **Configuration** → **Application settings**
3. Adicione as variáveis:
   - `APPLICATIONINSIGHTS__INSTRUMENTATIONKEY`
   - `APPLICATIONINSIGHTS__INGESTIONENDPOINT`
   - `APPLICATIONINSIGHTS__LIVEENDPOINT`
   - `APPLICATIONINSIGHTS__APPLICATIONID`

### Azure Key Vault (Recomendado)

Para máxima segurança, considere usar Azure Key Vault:

```bash
# Crie um Key Vault
az keyvault create --resource-group myRG --name myVault

# Adicione segredos
az keyvault secret set --vault-name myVault \
  --name AppInsightsInstrumentationKey \
  --value "sua-chave-aqui"
```

## 📚 Recursos Adicionais

- [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
- [Environment Variables em ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/)
- [Secrets Management Best Practices](https://docs.microsoft.com/en-us/azure/security/fundamentals/identity-management-best-practices)

---

**Lembrete:** A segurança é responsabilidade de todos. Sempre revise seus commits antes de fazer push! 🔒
