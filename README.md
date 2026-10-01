# cloud-ia-portfolio
# P1 — Lab serverless (Fase 1)

## Resumen
Cuenta AWS (últimos 4 dígitos OK): …  
Regiones usadas: us-east-2 (…), us-east-1 (Lambda/API/Secrets)  
Usuario IAM: pepe

## Diagrama (texto)
Cliente → API Gateway → Lambda hola-portfolio
EventBridge (schedule, ahora Disabled) → Lambda
Producer (consola) → SQS hola-cola-lab → Consumer (Poll)
Secrets Manager: demo/api-key-lab
S3: cloud-ia-portfolio-pepe-…

## Recursos
| Servicio | Nombre | Notas |
|----------|--------|-------|
| S3 | … | privado |
| Lambda | hola-portfolio | Python |
| API | … /hola | |
| EventBridge | hola-cada-5min | Disabled |
| SQS | hola-cola-lab | |
| Secret | demo/api-key-lab | demo only |

## IAM y seguridad
- Root con MFA
- pepe: (qué policies tiene hoy)
- Budget $10
- Secreto demo (no keys reales en el repo)

## Fallos / lecciones
1. Región CLI ≠ región del recurso
2. …
3. …

## Coste
Budget, Free Tier / créditos; apagar schedules al terminar labs.
