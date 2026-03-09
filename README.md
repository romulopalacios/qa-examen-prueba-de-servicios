# 🚀 QA API Testing - Billetera Digital

[![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)](https://www.postman.com/)
[![QA Testing](https://img.shields.io/badge/QA_Automation-0052CC?style=for-the-badge&logo=testing&logoColor=white)](#)
[![JSON](https://img.shields.io/badge/JSON-000000?style=for-the-badge&logo=json&logoColor=white)](#)

Repositorio con la automatización de pruebas de API REST para una Billetera Digital (Wallet). Este proyecto fue desarrollado como evaluación práctica de QA, enfocado en la validación de reglas de negocio, interacción entre microservicios y automatización de variables de entorno mediante scripts de Postman.

## 📋 Descripción del Proyecto

El objetivo de esta suite de pruebas es validar el flujo transaccional y de autenticación de un sistema bancario simulado, asegurando el correcto funcionamiento de los siguientes microservicios:

* **Auth Service (Puerto 4001):** Gestión de identidades, registro y generación de tokens JWT.
* **Wallet Service (Puerto 4002):** Gestión de saldos, recargas y consulta de historial.
* **Transaction Service (Puerto 4003):** Core bancario para el procesamiento de transferencias.
* **Admin Service (Puerto 4004):** Backoffice administrativo para ajustes de saldo manuales.

## 🛠️ Características de la Automatización

* **Variables Dinámicas:** Cero datos "hardcodeados". Los IDs de usuario y Tokens JWT se capturan de las respuestas y se inyectan en las variables de entorno automáticamente.
* **Validaciones (Assertions):** Uso de scripts en JavaScript (Chai) para validar códigos de estado HTTP (200, 201, 400), estructura del payload JSON y tipos de datos.
* **Negative Testing:** Validación del bloqueo del sistema ante escenarios no deseados (ej. transferencias con saldo insuficiente).
* **Validación de Headers de Seguridad:** Comprobación del uso obligatorio del header `x-channel`.

## ⚙️ Cómo ejecutar las pruebas

1.  Clona este repositorio en tu máquina local.
2.  Abre **Postman**.
3.  Importa el archivo de la colección: `Examen-Wallet-QA.postman_collection.json`.
4.  Importa el archivo del entorno: `wallet-test.postman_environment.json`.
5.  Asegúrate de seleccionar el entorno `wallet-test` en la esquina superior derecha de Postman.
6.  Ejecuta los requests en orden secuencial o utiliza el **Collection Runner** para una ejecución automatizada.

## 📊 Conclusiones y Hallazgos de QA

Durante la ejecución exploratoria y automatizada, se levantaron los siguientes hallazgos:

1.  **Diferencias entre documentación y arquitectura real:** Las instrucciones iniciales indicaban una URL base con el puerto `5173`. Sin embargo, se identificó que el sistema opera bajo una arquitectura de microservicios estricta. Se aplicó una refactorización parametrizando las variables de entorno para enrutar correctamente las peticiones a los puertos 4001, 4002, 4003 y 4004.
2.  **El sistema bloquea bien las transacciones sin fondos (Prueba Negativa):** El flujo requería transferir $950 teniendo un saldo fondeado de solo 500. Al intentarlo, la API bloqueó la transacción y devolvió un error `400 Bad Request` (`WALL_001` - Fondos insuficientes). Esto certifica que el Core Bancario valida correctamente las reglas de negocio.
3.  **Resolución estratégica (Uso del servicio Admin):** Para completar el flujo y validar que el receptor recibiera el dinero sin romper el requerimiento original, se aplicó un *workaround*. Mediante el servicio de Administrador, se inyectó saldo manualmente a la cuenta del emisor, habilitando así el *Happy Path*.
4.  **Validación estricta de seguridad en los Headers:** La API demostró ser robusta a nivel de seguridad, exigiendo estrictamente el header `x-channel` y devolviendo un error `GEN_001` ante su omisión.

---
**Autor:** Rómulo Palacios Rivas
*QA Tester / Estudiante de Ingeniería en Tecnologías de la Información*
