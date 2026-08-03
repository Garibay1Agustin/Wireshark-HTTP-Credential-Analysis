# Análisis de Tráfico de Red: HTTP (Texto Plano) vs HTTPS (TLS)

## Descripción

En este laboratorio utilicé **Wireshark** para analizar las diferencias entre una comunicación realizada mediante **HTTP** y otra protegida con **HTTPS (TLS)**.

El objetivo fue observar qué información puede obtener un atacante al capturar tráfico de red y comprobar cómo el cifrado de TLS protege la confidencialidad de los datos durante su transmisión.

---

## Información del laboratorio

| Herramienta | Wireshark |
|------------|-----------|
| Protocolos analizados | HTTP, HTTPS (TLS) |
| Objetivo | Comparar tráfico sin cifrar y tráfico cifrado |
| Tipo de análisis | Captura e inspección de paquetes |
| Nivel | Principiante |

---

# 1. Captura del tráfico HTTP

Como primera prueba envié un formulario utilizando **HTTP** y capturé el tráfico generado con Wireshark.

Para localizar únicamente las solicitudes enviadas mediante el método **POST**, apliqué el siguiente filtro:

```text
http.request.method == "POST"
```

Este filtro permitió identificar rápidamente los paquetes que contenían la información enviada desde el navegador hacia el servidor.

![Filtrado HTTP POST](evidencia/foto2_post_credentials.png)

---

# 2. Inspección del contenido transmitido

Una vez localizada la comunicación, utilicé la opción **Follow TCP Stream** para reconstruir la conversación completa entre el cliente y el servidor.

Al revisar el flujo observé que los datos enviados por el formulario, como nombres, direcciones de correo electrónico y números de teléfono, viajaban completamente en **texto plano**.

Esto demuestra que HTTP no incorpora mecanismos de cifrado durante la transmisión de la información. Como consecuencia, cualquier persona con acceso al tráfico de la red podría inspeccionar los paquetes y visualizar su contenido sin necesidad de realizar ningún proceso de descifrado.

![Datos visibles mediante HTTP](evidencia/foto3_follow_stream.png)

---

# 3. Comparación con HTTPS (TLS)

Después repetí la misma prueba utilizando una conexión protegida mediante **HTTPS**.

Para visualizar únicamente el tráfico correspondiente a TLS utilicé el siguiente filtro:

```text
tls
```

Aunque los paquetes seguían siendo visibles en Wireshark, el contenido ya no podía interpretarse. Al reconstruir la conversación mediante **Follow TCP Stream**, toda la información aparecía cifrada.

Esta diferencia demuestra cómo **TLS protege la confidencialidad de los datos durante la transmisión**, impidiendo que un tercero pueda leer la información capturada mediante técnicas de sniffing.

![Tráfico cifrado mediante TLS](evidencia/trafico_TLS_SSL_y_QUIC_2.jpg)

---

# 4. Evidencias del laboratorio

Las siguientes capturas corresponden a las distintas etapas del análisis realizado:

| Evidencia | Descripción |
|-----------|-------------|
| foto2_post_credentials.png | Captura de una solicitud HTTP POST. |
| foto3_follow_stream.png | Reconstrucción del flujo TCP mostrando los datos en texto plano. |
| trafico_TLS_SSL_y_QUIC_2.jpg | Comparación entre tráfico HTTP y tráfico protegido mediante TLS. |

---

# Conclusiones

Este laboratorio permitió comprobar de forma práctica la diferencia entre transmitir información mediante **HTTP** y hacerlo utilizando **HTTPS**.

Las principales conclusiones fueron:

- HTTP transmite la información sin cifrado, por lo que los datos pueden visualizarse fácilmente al capturar el tráfico de la red.
- HTTPS utiliza TLS para cifrar la comunicación entre el cliente y el servidor, evitando que la información pueda interpretarse aunque los paquetes sean interceptados.
- TLS protege la información durante su transmisión, pero no reemplaza otras medidas de seguridad necesarias para proteger una aplicación web.

---

# Preguntas frecuentes

## ¿Wireshark es un IPS?

No.

Wireshark es un **analizador de protocolos de red (Network Protocol Analyzer)** utilizado para capturar e inspeccionar paquetes con fines de monitoreo, resolución de problemas y análisis forense.

No bloquea, modifica ni filtra el tráfico de la red, por lo que no debe confundirse con un IDS o un IPS.

---

## ¿HTTPS garantiza que un sitio web es completamente seguro?

No.

HTTPS protege la comunicación entre el cliente y el servidor mediante el uso de **TLS**, asegurando la confidencialidad e integridad de los datos durante su transmisión.

Sin embargo, una aplicación web puede utilizar HTTPS y seguir siendo vulnerable a ataques como:

- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Errores de autenticación
- Configuraciones inseguras del servidor

Por este motivo, HTTPS representa una medida de seguridad fundamental, pero solo constituye una parte de la seguridad total de una aplicación.

---

## Tecnologías utilizadas

- Wireshark
- HTTP
- HTTPS
- TLS
- TCP
- Follow TCP Stream

---

*Laboratorio realizado con fines educativos para practicar el análisis de tráfico de red utilizando Wireshark.*
