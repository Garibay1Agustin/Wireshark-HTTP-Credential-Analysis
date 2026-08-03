# Análisis de Tráfico de Red: HTTP (Texto Plano) vs HTTPS (TLS)

## Descripción

En este proyecto utilicé **Wireshark** para comparar cómo viaja la información cuando una aplicación utiliza **HTTP** y cuando utiliza **HTTPS (TLS)**.

El objetivo fue demostrar la diferencia entre transmitir información sin cifrado y hacerlo mediante una conexión protegida con TLS, observando el contenido de los paquetes capturados en ambos escenarios.

---

## Información del proyecto

| Herramienta | Wireshark |
|-------------|-----------|
| Protocolos analizados | HTTP, HTTPS (TLS) |
| Objetivo | Comparar tráfico sin cifrar y tráfico cifrado |
| Tipo de análisis | Captura e inspección de paquetes |
| Nivel | Principiante |

---

# 1. Captura del tráfico HTTP

Para comenzar, envié un formulario utilizando **HTTP** y capturé el tráfico generado con Wireshark.

Con el objetivo de localizar únicamente las solicitudes enviadas mediante el método **POST**, apliqué el siguiente filtro:

```text
http.request.method == "POST"
```

Este filtro permitió identificar rápidamente el paquete que contenía la información enviada desde el navegador hacia el servidor.

![Filtro HTTP POST](evidencia/http.request.method%20==%20POST.png)

---

# 2. Inspección del contenido transmitido

Una vez identificada la comunicación, utilicé la opción **Follow TCP Stream** para reconstruir la conversación completa entre el cliente y el servidor.

Al inspeccionar el flujo fue posible comprobar que los datos enviados por el formulario, como nombres, direcciones de correo electrónico y números de teléfono, viajaban completamente en **texto plano**.

Esto demuestra que HTTP no cifra la información durante la transmisión. Como consecuencia, cualquier persona con acceso al tráfico de la red podría visualizar estos datos mediante técnicas de captura de paquetes.

![Datos visibles en texto plano](evidencia/Evidencia%20de%20texto%20plano.png)

---

# 3. Comparación con HTTPS (TLS)

Después repetí exactamente la misma prueba utilizando una conexión protegida mediante **HTTPS**.

Para visualizar únicamente el tráfico cifrado utilicé el siguiente filtro:

```text
tls
```

Aunque los paquetes seguían siendo visibles en Wireshark, el contenido ya no podía interpretarse. Al reconstruir la conversación utilizando **Follow TCP Stream**, toda la información aparecía cifrada.

Esto demuestra cómo **TLS protege la confidencialidad de los datos durante la transmisión**, evitando que un tercero pueda interpretar la información capturada.

![Tráfico TLS cifrado](evidencia/tráfico%20TLS%20SSL%20y%20QUIC.png)

---

# Conclusiones

Este proyecto permitió comprobar de forma práctica la diferencia entre utilizar **HTTP** y **HTTPS**.

Las principales conclusiones fueron:

- HTTP transmite la información sin cifrado, por lo que cualquier persona que capture el tráfico puede visualizar los datos enviados.
- HTTPS utiliza TLS para cifrar la comunicación entre el cliente y el servidor, impidiendo que el contenido pueda leerse aunque los paquetes sean interceptados.
- TLS protege la información durante la transmisión, pero no garantiza por sí solo que una aplicación web sea completamente segura.

---

# Preguntas frecuentes

## ¿Wireshark es un IPS?

No.

Wireshark es un **analizador de protocolos de red** (*Network Protocol Analyzer*). Su función consiste en capturar e inspeccionar paquetes para tareas de monitoreo, resolución de problemas y análisis forense.

No bloquea, modifica ni filtra el tráfico de la red.

---

## ¿HTTPS garantiza que un sitio web sea completamente seguro?

No.

HTTPS protege la comunicación entre el cliente y el servidor mediante **TLS**, asegurando la confidencialidad de los datos durante la transmisión.

Sin embargo, una aplicación puede utilizar HTTPS y seguir siendo vulnerable a ataques como:

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Errores de autenticación
- Configuraciones inseguras del servidor

Por este motivo, HTTPS representa una medida de seguridad muy importante, pero no reemplaza otras buenas prácticas de seguridad.

---

## Tecnologías utilizadas

- Wireshark
- HTTP
- HTTPS
- TLS
- TCP
- Follow TCP Stream

---

**Proyecto realizado con fines educativos para practicar el análisis de tráfico de red mediante Wireshark.**
