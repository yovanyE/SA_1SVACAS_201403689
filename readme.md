
# Online Shopping System - Microservicios

Este proyecto implementa la descomposición de un sistema monolítico de compras en línea hacia una arquitectura basada en **microservicios**, utilizando tecnologías modernas como **Docker**, **Kafka**, y bases de datos independientes por servicio.

## Estructura del Proyecto

```bash
.
├── gateway-api/
├── product-service/
├── order-service/
├── payment-service/
├── shipping-service/
├── customer-service/
├── review-service/
├── return-service/
├── offer-service/
├── analytics-service/
├── supplier-service/
├── docker-compose.yml
└── README.md
```

## Tecnologías Utilizadas

- Docker & Docker Compose  
- Apache Kafka  
- PostgreSQL  
- API Gateway 
- HTTP REST entre servicios  
- Comunicación asincrónica con Kafka  

## Requisitos

- Docker (v20+)  
- Docker Compose (v1.29+ o v2)  
- Git  

## Instrucciones de Despliegue

1. Clona este repositorio

```bash
git clone https://github.com/yovanyE/SA_1SVACAS_201403689.git
cd SA_1SVACAS_201403689
```

2. Inicia todos los servicios

```bash
docker-compose up --build
```

3. Accede a los servicios

| Servicio          | URL                   |
|-------------------|-----------------------|
| Gateway API       | http://localhost      |
| Product Service   | http://localhost:8001 |
| Order Service     | http://localhost:8002 |
| Payment Service   | http://localhost:8003 |
| Kafka UI (opcional)| http://localhost:9092 |

## Eventos manejados (Kafka)

- OrderCreated  
- PaymentCompleted  
- RefundProcessed  

## Informe

Para Informe consultar el archivo [informe.md](./informe.md) para más detalles sobre:

- Justificación del rediseño  
- Descripción de los servicios
- Diagrama de arquitectura  
- Lecciones aprendidas  y desafios enfrentados
- Docker compose

