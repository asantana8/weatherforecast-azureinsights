# KQL - Application Insights para AZ-204

Mini roteiro de KQL bem focado em Application Insights para AZ-204, indo do básico ao que costuma ser cobrado.

## 1. Requests (requisições HTTP)

### Requests nos últimos 30 minutos, com status e duração média

```kusto
requests
| where timestamp >= ago(30m)
| summarize avg_duration_ms = avg(duration), count() by name, resultCode
| order by avg_duration_ms desc
```

### Requests que retornaram erro (HTTP 5xx) no último dia

```kusto
requests
| where timestamp >= ago(1d)
| where toint(resultCode) >= 500
| summarize Count = count() by name
| order by Count desc
```

### Requests mais lentos

```kusto
requests
| where timestamp >= ago(1h)
| top 20 by duration desc
| project timestamp, name, url, duration, resultCode, operation_Id
```

## 2. Dependencies (SQL, HTTP externos etc.)

### Dependências com falha

```kusto
dependencies
| where timestamp >= ago(1d)
| where success != "True"
| project timestamp, name, type, target, resultCode, operation_Id
| take 50
```

### Chamadas SQL lentas

```kusto
dependencies
| where timestamp >= ago(1h)
| where type == "SQL"
| top 20 by duration desc
| project timestamp, name, data, target, duration, success, operation_Id
```

### Juntar dependências com requests (correlação por operação)

```kusto
dependencies
| where timestamp >= ago(1d)
| join kind=inner (
    requests
    | where timestamp >= ago(1d)
) on operation_Id
| project request_name = name1, dependency_name = name, type, target, resultCode, duration
```

## 3. Exceptions (erros)

### Lista de exceptions recentes

```kusto
exceptions
| where timestamp >= ago(1h)
| project timestamp, type, problemId, outerMessage, operation_Id
| order by timestamp desc
```

### Top tipos de exception por quantidade

```kusto
exceptions
| where timestamp >= ago(1d)
| summarize Count = count() by type
| order by Count desc
```

### Exceptions ligadas a uma operação específica

```kusto
exceptions
| where operation_Id == "<operation-id-aqui>"
| project timestamp, type, problemId, outerMessage
```

## 4. Traces (logs de aplicação)

### Traces de nível Warning ou Error

```kusto
traces
| where timestamp >= ago(2h)
| where severityLevel >= 2   // 2=Warning, 3=Error, 4=Critical
| project timestamp, severityLevel, message, operation_Id
| order by timestamp desc
```

### Procurar por uma mensagem específica

```kusto
traces
| where timestamp >= ago(1d)
| where message has "pedido inválido"
```

## 5. Unindo telemetrias (requests, dependencies, traces, exceptions)

### Tudo relacionado a uma operação (correlacionado por operation_Id)

```kusto
let opId = "<operation-id-aqui>";
union isfuzzy=true
    requests,
    dependencies,
    traces,
    exceptions
| where operation_Id == opId
| project timestamp, itemType = itemType, name, message, type, resultCode, duration
| order by timestamp asc
```

### Requests com falha e suas exceptions

```kusto
requests
| where timestamp >= ago(1d)
| where toint(resultCode) >= 500
| join kind=leftouter (
    exceptions
    | project exception_timestamp = timestamp, operation_Id, type, outerMessage
) on operation_Id
| project timestamp, name, resultCode, exception_timestamp, type, outerMessage
| order by timestamp desc
```

## 6. Métricas básicas por período

### Quantidade de requests por minuto

```kusto
requests
| where timestamp >= ago(1h)
| summarize count() by bin(timestamp, 1m)
| order by timestamp asc
```

### Taxa de sucesso vs falha

```kusto
requests
| where timestamp >= ago(1d)
| summarize total = count(), failed = countif(toint(resultCode) >= 400)
| extend successRate = 100.0 * (total - failed) / total
```

## 7. Padrão de consulta para alertas

Quase todo alerta baseado em log segue esse padrão: filtra, agrega e verifica condição.

Exemplo: muitas falhas de dependência SQL em 5 min.

```kusto
dependencies
| where timestamp >= ago(5m)
| where type == "SQL" and success != "True"
| summarize failures = count()
```

Esta consulta é usada como base em um alerta de log (quando failures > X).
