# cloud-ia-portfolio

Ruta Cloud IA Remoto — labs AWS hacia portfolio para trabajo remoto.

## P1 — Lab serverless (Fase 1)

### Resumen
- Cuenta AWS (últimos 4): `2623`
- Usuario IAM: `pepe`
- Regiones: `us-east-2` (S3 / setup temprano); `us-east-1` (Lambda, API Gateway, Secrets Manager)
- Objetivo: practicar piezas serverless y documentar decisiones

## Diagrama (texto)
Cliente → API Gateway → Lambda hola-portfolio
EventBridge (schedule, ahora Disabled) → Lambda
Producer (consola) → SQS hola-cola-lab → Consumer (Poll)
Secrets Manager: demo/api-key-lab
S3: cloud-ia-portfolio-pepe-135110952623

### Recursos
| Servicio | Nombre | Notas |
|----------|--------|-------|
| S3 | `cloud-ia-portfolio-pepe-135110952623` | Block public access ON |
| Lambda | `hola-portfolio` | Python; responde Hello from Lambda |
| API Gateway | HTTP API, ruta `/hola` | us-east-1 |
| CloudWatch Logs | `/aws/lambda/hola-portfolio` | START/END/REPORT |
| EventBridge | `hola-cada-5min` | schedule → Lambda; **Disabled** |
| SQS | `hola-cola-lab` | send/receive + delete |
| Secrets Manager | `demo/api-key-lab` | key api_key (demo); Lambda hola-portfolio lo lee con boto3 (solo preview) |

### IAM y seguridad
- Root con MFA
- pepe ahora usa CloudIaPortfolioLab (sin Admin).
- Budget ~$10
- Secreto solo demo; **nunca** keys reales en el repo

### Fallos / lecciones
1. Región por defecto del CLI ≠ región del secreto → usar `--region us-east-1`
2. No dejar EventBridge Enabled tras el lab (disparo de costo)
3. En Secrets Manager, errores de RDS/Redshift/DocumentDB se pueden ignorar con other type of secret
4. Lambda necesita GetSecretValue en su execution role; región del cliente boto3 debe coincidir con el secreto

### Coste
Budget + Free Tier / créditos; apagar schedules; no subir datos sensibles.

### Extra — Lambda + Secrets (10B)
`hola-portfolio` usa `secretsmanager:GetSecretValue` sobre `demo/api-key-lab` y responde solo un preview (`sk-demo…`), sin loguear la key completa.
