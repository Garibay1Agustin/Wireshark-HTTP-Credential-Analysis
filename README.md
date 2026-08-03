# Wireshark-HTTP-Credential-Analysis
Análisis de tráfico de red con Wireshark para la detección e intercepción de credenciales en texto plano sobre el protocolo HTTP.

# Análisis de Tráfico de Red: HTTP (Texto Plano) vs HTTPS (TLS)

## Descripción

En este laboratorio utilicé Wireshark para comparar cómo viaja la información cuando una aplicación utiliza HTTP y cuando utiliza HTTPS (TLS).

El objetivo fue observar qué datos pueden obtenerse al capturar tráfico de red y comprobar cómo cambia el contenido de los paquetes cuando la comunicación está protegida mediante TLS.

---

## 1. Captura del tráfico HTTP

Como primera prueba envié un formulario a través de HTTP y capturé el tráfico con Wireshark.

Para localizar únicamente las solicitudes realizadas mediante el método **POST**, utilicé el siguiente filtro:

```text
http.request.method == "POST"
```

Con este filtro fue sencillo identificar los paquetes correspondientes al envío de información desde el navegador hacia el servidor.

![Filtrado de solicitudes HTTP POST](evidencia/foto1_filtro_post.png)

---

## 2. Inspección del contenido transmitido

Una vez localizada la comunicación, utilicé la opción **Follow TCP Stream** para reconstruir toda la conversación entre el cliente y el servidor.

Al revisar el flujo pude comprobar que los datos enviados por el formulario, como nombres, direcciones de correo electrónico y números de teléfono, viajaban completamente en texto plano.

Esto demuestra que HTTP no protege la información durante la transmisión. Cualquier persona con acceso al tráfico de la red podría capturar esos paquetes y visualizar su contenido sin necesidad de realizar ningún proceso de descifrado.

![Datos visibles utilizando HTTP](evidencia/foto2_tcp_stream_http.png)

---

## 3. Comparación con HTTPS (TLS)

Después repetí la misma prueba utilizando una conexión protegida mediante HTTPS.

Para visualizar únicamente este tráfico apliqué el siguiente filtro:

```text
tls
```

Aunque los paquetes seguían siendo visibles en Wireshark, el contenido ya no podía interpretarse. Al abrir la conversación mediante **Follow TCP Stream**, toda la información aparecía cifrada.

Esta diferencia demuestra cómo TLS protege los datos durante la transmisión, impidiendo que un tercero pueda leer la información capturada simplemente observando el tráfico de red.

![Comunicación protegida mediante TLS](evidencia/foto3_tcp_stream_tls.png)

---

# Conclusiones

Durante el laboratorio fue posible observar claramente la diferencia entre ambos protocolos.

* HTTP transmite la información sin cifrado, permitiendo que los datos puedan visualizarse directamente al capturar el tráfico.
* HTTPS utiliza TLS para cifrar la comunicación entre el cliente y el servidor, evitando que la información pueda leerse durante la transmisión.

Si bien TLS protege los datos mientras viajan por la red, no reemplaza otras medidas de seguridad. Un servidor puede utilizar HTTPS y aun así presentar vulnerabilidades en la aplicación.

---

# Preguntas frecuentes

### ¿Wireshark es un IPS?

No. Wireshark es un analizador de protocolos de red (Network Protocol Analyzer). Su función consiste en capturar e inspeccionar paquetes para tareas de monitoreo, análisis e investigación. No bloquea, modifica ni filtra el tráfico de la red.

### ¿HTTPS garantiza que un sitio web es completamente seguro?

No. HTTPS únicamente protege la comunicación entre el cliente y el servidor mediante cifrado. La seguridad de la aplicación depende también de otros factores, como una correcta configuración del servidor y la ausencia de vulnerabilidades, por ejemplo SQL Injection o Cross-Site Scripting (XSS).

---

*Laboratorio realizado con fines educativos para practicar el análisis de tráfico de red utilizando Wireshark.*
