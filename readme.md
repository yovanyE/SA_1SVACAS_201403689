# Informe de Rediseño - Online Shopping System

## 1. Justificación del Rediseño

El sistema monolítico original del Online Shopping System presentaba una estructura centralizada, donde todas las funcionalidades (productos, pagos, pedidos, devoluciones, reseñas, envíos, atención al cliente, reportes, ofertas, proveedores) estaban acopladas en una única base de código. Esta arquitectura, aunque funcional, tenía varios inconvenientes:

* **Escalabilidad limitada**: No se podía escalar módulos de forma independiente.
* **Despliegue complejo**: Cualquier cambio requería recompilar y desplegar todo el sistema.
* **Difícil mantenimiento**: Un fallo en un módulo podía afectar todo el sistema.

Por ello, se propone rediseñar el sistema usando una arquitectura basada en **microservicios**, aplicando los principios de:

* Separación de responsabilidades.
* Independencia de despliegue.
* Bases de datos separadas.
* Comunicación vía HTTP REST y eventos.

---

## 2. Descripción de Microservicios

| Microservicio         | Funciones principales                                     |
| --------------------- | --------------------------------------------------------- |
| **Product Service**   | Listar, agregar y actualizar productos, stock.            |
| **Order Service**     | Checkout, seguimiento, gestión de pedidos.                |
| **Payment Service**   | Procesamiento de pagos, comunicación con Payment Gateway. |
| **Customer Service**  | Datos de cliente, atención al cliente.                    |
| **Review Service**    | Reseñas y calificaciones de productos.                    |
| **Return Service**    | Devoluciones, reembolsos.                                 |
| **Offer Service**     | Visualización y administración de ofertas.                |
| **Analytics Service** | Reportes y predicciones de ventas.                        |
| **Supplier Service**  | Stock de proveedores y pronósticos.                       |
| **Shipping Service**  | Logística y envío de pedidos.                             |
| **Gateway API**       | Punto de entrada y ruteo de peticiones.                   |

---

## 3. Diagrama de Arquitectura
![''](/imagen/architect.png)

---

## 4. Lecciones Aprendidas y Desafíos

### Lecciones Aprendidas:

* Comprender el dominio de negocio es clave para una correcta división de microservicios.
* El desacoplamiento de servicios permite mayor agilidad de desarrollo y despliegue.
* Kafka es útil para desacoplar eventos complejos entre servicios.

### Desafíos:

* Diseñar correctamente los contratos de comunicación (APIs y eventos).
* Manejar la consistencia eventual entre servicios.
* La configuración de Docker y redes internas requiere una buena planificación.

---

## 5. Docker Compose

```yaml
version: "3.8"
services:
  gateway-api:
    build: ./gateway-api
    ports:
      - "80:80"
    depends_on:
      - product-service
      - order-service
      - payment-service
      - customer-service
      - review-service
      - return-service
      - offer-service
      - analytics-service
      - supplier-service
      - shipping-service

  product-service:
    build: ./product-service
    ports:
      - "8001:8001"
    environment:
      - DB_HOST=product-db

  product-db:
    image: postgres:13
    environment:
      POSTGRES_DB: products
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  order-service:
    build: ./order-service
    ports:
      - "8002:8002"
    environment:
      - DB_HOST=order-db

  order-db:
    image: postgres:13
    environment:
      POSTGRES_DB: orders
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  payment-service:
    build: ./payment-service
    ports:
      - "8003:8003"
    environment:
      - PAYMENT_GATEWAY_URL=http://external-payment-gateway.com

  customer-service:
    build: ./customer-service
    ports:
      - "8004:8004"
    environment:
      - DB_HOST=customer-db

  customer-db:
    image: postgres:13
    environment:
      POSTGRES_DB: customers
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  review-service:
    build: ./review-service
    ports:
      - "8005:8005"
    environment:
      - DB_HOST=review-db

  review-db:
    image: postgres:13
    environment:
      POSTGRES_DB: reviews
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  return-service:
    build: ./return-service
    ports:
      - "8006:8006"
    environment:
      - DB_HOST=return-db

  return-db:
    image: postgres:13
    environment:
      POSTGRES_DB: returns
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  offer-service:
    build: ./offer-service
    ports:
      - "8007:8007"
    environment:
      - DB_HOST=offer-db

  offer-db:
    image: postgres:13
    environment:
      POSTGRES_DB: offers
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  analytics-service:
    build: ./analytics-service
    ports:
      - "8008:8008"
    environment:
      - DB_HOST=analytics-db

  analytics-db:
    image: postgres:13
    environment:
      POSTGRES_DB: analytics
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  supplier-service:
    build: ./supplier-service
    ports:
      - "8009:8009"
    environment:
      - DB_HOST=supplier-db

  supplier-db:
    image: postgres:13
    environment:
      POSTGRES_DB: suppliers
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass

  shipping-service:
    build: ./shipping-service
    ports:
      - "8010:8010"
    environment:
      - SHIPPING_API_URL=http://external-shipping.com

  kafka:
    image: bitnami/kafka:latest
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092

  zookeeper:
    image: bitnami/zookeeper:latest
    ports:
      - "2181:2181"
    environment:
      ALLOW_ANONYMOUS_LOGIN: "yes"

```

