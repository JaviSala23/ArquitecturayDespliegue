Arquitectura y Despliegue de Aplicaciones

Reader teórico-prácticoTecnicatura Universitaria en Programación Full Stack - Primer añoAsignatura: Diseños y Arquitectura de Despliegue 1Docente: Prof. Javier Sala

Presentación

Este reader organiza el recorrido de la materia alrededor de una pregunta central:

¿Qué debe suceder para que una aplicación que funciona en la computadora del programador pueda ser utilizada por otras personas?

El punto de partida es distinguir desarrollo de despliegue. A partir de allí se avanza desde los componentes lógicos de una aplicación hacia las redes que los conectan, los modelos donde pueden ejecutarse, la infraestructura que los sostiene y, finalmente, un despliegue real y documentado.

[!IMPORTANT]Programar y poner en funcionamiento una aplicación son actividades relacionadas, pero no equivalentes. El despliegue conecta el trabajo de desarrollo con el uso real del software.

Sobre este material

La Unidad 1 se basa directamente en la Clase 1, Del código al usuario. Las Unidades 2 a 5 desarrollan el mapa de ruta presentado para la materia -redes y HTTP, modelos de despliegue, infraestructura y proyecto integrador- incorporando contenido didáctico complementario.

Resultados de aprendizaje

Al finalizar el recorrido se espera que el estudiante pueda:

Explicar el ciclo que lleva una aplicación desde la necesidad hasta producción y mantenimiento.

Diferenciar componentes lógicos -frontend, backend y base de datos- de su ubicación física.

Comprender cómo viaja una solicitud a través de una red utilizando DNS, puertos y HTTP/HTTPS.

Comparar modelos de despliegue local, on-premise, VPS/nube y plataformas gestionadas.

Reconocer la función de un servidor web, un servidor de aplicación, procesos, servicios, variables de entorno y contenedores.

Planificar, ejecutar, verificar y documentar un despliegue básico de una aplicación full stack.

Mapa de la materia

Unidad

Pregunta guía

Producto de aprendizaje

1. Del código al usuario

¿Qué significa desplegar?

Diagrama lógico y primeras decisiones de despliegue.

2. Redes y HTTP

¿Cómo viaja la información?

Trazado de una solicitud web y diagnóstico básico.

3. Modelos de despliegue

¿Dónde vivirá el código?

Comparación argumentada de alternativas.

4. Infraestructura

¿Cómo ejecutamos y sostenemos la aplicación?

Arquitectura de producción y procedimiento de despliegue.

5. Proyecto integrador

¿Podemos llevar una aplicación real a producción?

Despliegue funcional, documentación y defensa técnica.

Unidad 1 - Del código al usuario

1.1 Desarrollo, pruebas, despliegue, producción y mantenimiento

Una aplicación puede funcionar correctamente en el equipo del desarrollador y todavía no estar disponible para otras personas. Para llegar al uso real, el software debe atravesar una serie de etapas y decisiones.

flowchart LR
    A[Necesidad] --> B[Análisis y diseño]
    B --> C[Desarrollo]
    C --> D[Pruebas]
    D --> E[Despliegue]
    E --> F[Producción]
    F --> G[Mantenimiento]
    G --> C

Concepto

Pregunta principal

Ejemplo

Desarrollo

¿Cómo construimos la solución?

Se escribe y modifica el código.

Pruebas

¿Funciona como se espera?

Se verifica que una reserva no se duplique.

Despliegue

¿Cómo lo ponemos en funcionamiento?

Se instala o publica en el entorno elegido.

Producción

¿Cómo lo usan personas reales?

Los usuarios trabajan con información verdadera.

Mantenimiento

¿Cómo lo sostenemos y mejoramos?

Se corrigen errores y se publican actualizaciones.

[!NOTE]Desplegar una aplicación es preparar un entorno y colocar allí el software para que pueda ejecutarse y ser utilizado. Incluye instalación, configuración, puesta en funcionamiento y verificación.

El despliegue no ocurre necesariamente una sola vez. Cada corrección, mejora o nueva versión puede requerir un nuevo ciclo de despliegue.

1.2 Componentes básicos de una aplicación

Antes de decidir dónde ejecutar una aplicación, debemos reconocer qué partes existen y qué responsabilidad tiene cada una.

flowchart LR
    U[Usuario] --> F[Frontend]
    F --> B[Backend]
    B --> DB[(Base de datos)]
    DB --> B
    B --> F
    F --> U

Componente

Responsabilidad

Ejemplo

Usuario

Realiza una tarea mediante el sistema.

Mozo que registra un pedido.

Frontend

Parte visible e interactiva: pantallas, botones, formularios y mensajes.

Pantalla para elegir una mesa y cargar productos.

Backend

Procesa acciones, aplica reglas y coordina operaciones.

Valida la mesa, calcula el total y registra el pedido.

Base de datos

Conserva la información necesaria para el funcionamiento.

Usuarios, productos, pedidos, pagos y estados.

¿Qué significa Full Stack?

Full stack es el conjunto de capas que permiten que una aplicación funcione:

Interfaz - frontend.

Lógica - backend.

Almacenamiento - base de datos.

[!WARNING]Comprender todas las capas no significa que todas deban ejecutarse en la misma computadora.

1.3 Lo lógico y lo físico

Identificar componentes responde al QUÉ. Diseñar una arquitectura de despliegue agrega el DÓNDE y el CÓMO.

Preguntas de despliegue

¿El frontend se abre en un navegador o se instala como aplicación?

¿El backend se ejecuta en una computadora local o en un servidor?

¿La base de datos está en la misma computadora que el backend?

¿Se puede acceder solamente desde un edificio o también mediante Internet?

¿Qué ocurre si uno de los componentes deja de estar disponible?

[!TIP]Arquitectura de despliegue: representa los componentes de un sistema, los lugares donde se ejecutan y las conexiones que existen entre ellos.

flowchart TB
    subgraph Logico[Lo lógico - Qué hace el sistema]
        F[Frontend]
        B[Backend]
        DB[(Base de datos)]
    end

    subgraph Fisico[Lo físico - Dónde se ejecuta]
        C[Celular / PC del usuario]
        S[Servidor]
        D[(Servidor de datos)]
    end

    F -. puede ejecutarse en .-> C
    B -. puede ejecutarse en .-> S
    DB -. puede ejecutarse en .-> D

1.4 Caso guiado - Sistema de pedidos de un restaurante

Situación

Un restaurante necesita que los mozos carguen pedidos desde sus celulares. Los pedidos deben aparecer en cocina. La caja debe consultar mesas, cobrar y guardar las ventas.

Elemento

Posibles responsabilidades

Usuarios

Mozos, personal de cocina, cajero y administrador.

Frontend

Ingreso, lista de mesas, menú, envío de pedido, pantalla de cocina y cobro.

Backend

Validar usuarios, registrar pedidos, calcular totales, cambiar estados, buscar productos y registrar pagos.

Base de datos

Usuarios, mesas, productos, precios, pedidos, pagos y ventas.

Ubicación posible

Computadora local, servidor propio o servicio accesible por Internet.

flowchart LR
    M[Mozo / Caja] -->|usa| F[Frontend]
    F -->|envía acciones| B[Backend]
    B -->|lee / escribe| DB[(Base de datos)]
    B -->|pedido procesado| C[Cocina]
    B -->|respuesta| F

1.5 Preguntas de control

¿Por qué "funciona en mi computadora" no garantiza que una aplicación esté lista para usuarios reales?

¿Qué diferencia existe entre desarrollar y desplegar?

¿Qué diferencia hay entre un componente lógico y el lugar donde se ejecuta?

¿Por qué el mantenimiento forma parte del recorrido del software?

¿Por qué el despliegue puede repetirse varias veces durante la vida de una aplicación?

Actividad de unidad

Elegí una aplicación cotidiana y analizala sin nombrar todavía tecnologías concretas.

¿Quiénes son sus usuarios?

¿Qué elementos pertenecen al frontend?

¿Qué responsabilidades tendría el backend?

¿Qué información debería guardar?

Dibujá el flujo Usuario -> Frontend -> Backend -> Base de datos -> Respuesta.

Proponé dos lugares posibles donde podría ejecutarse y explicá qué cambiaría.

Unidad 2 - Redes, DNS, puertos y HTTP

2.1 ¿Cómo viaja la información?

Cuando una aplicación deja de ser puramente local, sus componentes necesitan comunicarse. Esa comunicación depende de redes, direcciones, puertos, nombres de dominio y protocolos.

sequenceDiagram
    participant U as Usuario
    participant B as Navegador
    participant DNS as DNS
    participant S as Servidor
    participant A as Aplicación
    participant DB as Base de datos

    U->>B: Ingresa https://ejemplo.com
    B->>DNS: ¿Qué IP corresponde al dominio?
    DNS-->>B: Dirección IP
    B->>S: Solicitud HTTPS :443
    S->>A: Reenvía solicitud
    A->>DB: Consulta / modifica datos
    DB-->>A: Resultado
    A-->>S: Respuesta
    S-->>B: HTTP response
    B-->>U: Muestra resultado

2.2 Cliente, servidor y red

Cliente: dispositivo o programa que inicia una solicitud, por ejemplo un navegador.

Servidor: equipo o proceso que escucha solicitudes y devuelve respuestas.

Red: medio que permite la comunicación entre ambos; puede ser local o atravesar Internet.

Dirección IP: identifica un equipo o interfaz dentro de una red.

Puerto: identifica un servicio concreto dentro de un equipo.

Ejemplo

http://127.0.0.1:8000

127.0.0.1 es la dirección de loopback del propio equipo.

8000 es el puerto del servicio.

Esa dirección no hace que la aplicación sea accesible automáticamente desde Internet.

2.3 DNS - De un nombre a una dirección

Las personas recuerdan nombres como ejemplo.com; las redes utilizan direcciones. DNS -Domain Name System- permite resolver un nombre de dominio hacia una dirección IP.

Registro

Función

A

Asocia un nombre con una dirección IPv4.

AAAA

Asocia un nombre con una dirección IPv6.

CNAME

Declara que un nombre es alias de otro nombre.

TTL

Indica cuánto tiempo puede almacenarse una respuesta DNS en caché.

[!NOTE]El dominio no "contiene" la aplicación. Permite localizar el servidor o servicio asociado.

2.4 HTTP - Solicitud y respuesta

HTTP organiza la conversación entre cliente y servidor.

Una solicitud puede incluir:

Método.

URL.

Cabeceras.

Cuerpo, cuando corresponde.

Una respuesta incluye:

Código de estado.

Cabeceras.

Contenido.

Método

Uso típico

Ejemplo

GET

Obtener un recurso

Listar productos.

POST

Crear o ejecutar una acción

Registrar un pedido.

PUT / PATCH

Modificar un recurso

Actualizar el estado de una mesa.

DELETE

Eliminar un recurso

Borrar un registro permitido.

HEAD / OPTIONS

Consultar metadatos o capacidades

Diagnóstico y negociación del servicio.

2.5 Códigos de estado HTTP

Familia

Significado

Ejemplos

1xx

Información

100 Continue

2xx

Éxito

200 OK, 201 Created, 204 No Content

3xx

Redirección

301 Moved Permanently, 302 Found

4xx

Error atribuible a la solicitud

400, 401, 403, 404

5xx

Error del servidor

500, 502, 503

2.6 HTTPS y TLS

HTTPS es HTTP protegido mediante TLS. Busca brindar:

Confidencialidad.

Integridad.

Autenticación del servidor mediante certificados.

[!WARNING]HTTPS protege el transporte de datos, pero no corrige por sí solo errores de autorización, contraseñas débiles, inyecciones o configuraciones inseguras de la aplicación.

2.7 Diagnóstico básico

# Verificar resolución DNS
nslookup ejemplo.com

# Ver cabeceras de una respuesta
curl -I https://ejemplo.com

# Ver una solicitud/respuesta con más detalle
curl -v https://ejemplo.com/ruta

# Ver puertos escuchando en Linux
ss -lntp

También puede utilizarse la pestaña Network de las herramientas de desarrollo del navegador para observar:

Método.

URL.

Código de estado.

Tiempo de respuesta.

Contenido transferido.

Práctica guiada

Tomá una aplicación web conocida y seguí una solicitud desde el navegador.

Identificá el dominio y explicá qué papel cumple DNS.

Elegí una solicitud en Network y anotá método, URL y código de estado.

Indicá si utiliza HTTP o HTTPS.

Explicá qué podría fallar si el dominio resuelve correctamente pero el servidor no responde.

Construí un diagrama desde el cliente hasta el backend y la base de datos.

Unidad 3 - Modelos y entornos de despliegue

3.1 ¿Dónde vivirá el código?

No existe un único lugar correcto para ejecutar una aplicación. La decisión depende de:

Alcance.

Presupuesto.

Cantidad de usuarios.

Conocimientos del equipo.

Conectividad.

Seguridad.

Disponibilidad requerida.

Necesidad de escalar.

3.2 Modelo local u on-premise

La aplicación se ejecuta en equipos que pertenecen a la organización o están físicamente en sus instalaciones.

Ventajas

Control directo.

Puede funcionar sin Internet si todos los componentes son locales.

Acceso físico al equipo.

Desventajas

Mantenimiento propio.

Energía y conectividad a cargo de la organización.

Mayor dificultad para acceso externo y alta disponibilidad.

3.3 VPS o infraestructura en la nube

Se alquila capacidad de cómputo en un centro de datos. El proveedor entrega una máquina virtual y el equipo administra el sistema operativo y la aplicación.

Ventajas

Acceso público.

Flexibilidad.

Control del sistema operativo.

Menor inversión inicial que comprar hardware propio.

Desventajas

Requiere administración del servidor.

Deben resolverse seguridad, actualizaciones, monitoreo y backups.

3.4 Plataforma gestionada - PaaS

Una plataforma abstrae gran parte de la infraestructura. El equipo entrega código o una imagen y la plataforma administra parte de la ejecución.

Ventajas

Menor carga operativa.

Despliegues más rápidos.

Desventajas

Límites de la plataforma.

Costos que pueden crecer.

Dependencia de funciones específicas del proveedor.

3.5 Distribución de componentes

Patrón

Frontend

Backend

Base de datos

Todo en un servidor

Mismo servidor

Mismo servidor

Mismo servidor

Cliente + servidor

Dispositivo del usuario

Servidor

Servidor

Tres capas separadas

Cliente o CDN

Servidor de aplicación

Servidor de BD dedicado

[!IMPORTANT]Separar componentes puede mejorar aislamiento y escalabilidad, pero también agrega conexiones, configuración y puntos de falla. Una arquitectura más distribuida no es automáticamente mejor.

3.6 Entornos de ejecución

Entorno

Objetivo

Características

Desarrollo

Construir y modificar

Datos de prueba, cambios frecuentes, herramientas de depuración.

Pruebas

Validar comportamiento

Pruebas manuales o automatizadas y datos controlados.

Staging / preproducción

Ensayar condiciones similares a producción

Configuración cercana a producción, sin usuarios finales reales.

Producción

Uso real

Datos verdaderos, seguridad, estabilidad, backups y monitoreo.

3.7 Criterios para elegir un modelo

Disponibilidad: ¿cuánto tiempo puede estar fuera de servicio?

Escalabilidad: ¿qué ocurre si crecen usuarios o tráfico?

Seguridad: ¿qué datos se almacenan y dónde?

Costo total: hosting, administración y soporte.

Conectividad: ¿debe funcionar sin Internet?

Mantenibilidad: ¿el equipo sabe operar la solución elegida?

Recuperación: ¿cómo se restaura el servicio ante una falla?

Actividad comparativa

Para un sistema de turnos médicos y un sistema de caja que debe seguir funcionando si se corta Internet, compará al menos dos modelos de despliegue.

¿Qué modelo elegirías para cada sistema?

¿Dónde ubicarías frontend, backend y base de datos?

¿Qué ventaja principal obtendrías?

¿Qué riesgo o costo asumirías?

¿Qué información adicional necesitarías antes de decidir?

Unidad 4 - Infraestructura para producción

4.1 ¿Cómo ejecutamos y sostenemos una aplicación?

Una arquitectura de producción necesita algo más que copiar archivos a un servidor. Debemos mantener procesos activos, gestionar conexiones, servir contenido, configurar secretos, almacenar datos, registrar errores y poder recuperar el sistema ante una falla.

flowchart LR
    U[Usuario] -->|HTTPS :443| W[Servidor web / Reverse proxy]
    W -->|Puerto interno| A[Servidor de aplicación]
    A --> B[Backend]
    B <--> DB[(Base de datos)]
    W --> ST[Archivos estáticos]

4.2 Sistema operativo, proceso y servicio

Sistema operativo: administra recursos del equipo y permite ejecutar software.

Proceso: instancia de un programa en ejecución.

Servicio: proceso administrado por el sistema para iniciar, detener, reiniciar y arrancar automáticamente.

Usuario del sistema: identidad con permisos específicos para ejecutar una aplicación y acceder a archivos.

# Ver estado de un servicio
systemctl status miapp

# Reiniciar un servicio
sudo systemctl restart miapp

# Ver logs recientes
journalctl -u miapp -n 100 --no-pager

4.3 Servidor web y servidor de aplicación

En muchas aplicaciones web, un servidor web público -por ejemplo Nginx o Apache- recibe conexiones HTTP/HTTPS y deriva las solicitudes dinámicas al servidor de aplicación.

Servidor web

Puede encargarse de:

TLS/HTTPS.

Archivos estáticos.

Redirecciones.

Límites y políticas de acceso.

Proxy inverso.

Servidor de aplicación

Ejecuta el framework y el código del backend.

Mantiene procesos capaces de atender solicitudes.

Proxy inverso

El cliente se conecta al servidor web. El servidor web actúa como punto de entrada y reenvía determinadas solicitudes al proceso interno de la aplicación.

4.4 Variables de entorno y configuración

Una aplicación no debería depender de valores rígidos escritos en el código para cada entorno.

Valores que suelen variar:

Claves secretas.

Credenciales de base de datos.

Dominio.

Modo de depuración.

URLs externas.

[!CAUTION]Nunca publiques secretos reales, contraseñas o claves privadas en un repositorio Git.

Buenas prácticas iniciales:

Usar valores distintos para desarrollo y producción.

Desactivar el modo de depuración detallada en producción.

Aplicar permisos mínimos a archivos de configuración sensibles.

4.5 Dependencias y aislamiento

El entorno de ejecución debe conocer las versiones de las bibliotecas que necesita la aplicación.

Ejemplo en Python:

python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

4.6 Contenedores - Idea inicial

Un contenedor empaqueta una aplicación con sus dependencias y una configuración reproducible.

Conceptos básicos:

Imagen: plantilla inmutable desde la que se crean contenedores.

Contenedor: instancia en ejecución de una imagen.

Volumen: almacenamiento persistente separado del ciclo de vida del contenedor.

Red de contenedores: conecta servicios como aplicación y base de datos.

[!NOTE]Docker no elimina la necesidad de comprender puertos, redes, persistencia, logs, seguridad y backups. Cambia la forma de empaquetar y ejecutar componentes.

4.7 Base de datos y persistencia

La base de datos contiene información que debe sobrevivir a reinicios y nuevas versiones.

Por eso debemos distinguir:

Código: puede reemplazarse por una nueva versión.

Datos persistentes: necesitan protección, migraciones y copias de respaldo.

Buenas prácticas:

Realizar backups con una frecuencia coherente con el valor de los datos.

Probar periódicamente que los backups puedan restaurarse.

Evitar cambios destructivos sin respaldo.

Aplicar migraciones de esquema como parte explícita del despliegue.

4.8 Archivos estáticos y archivos de usuario

Los archivos estáticos forman parte de la aplicación:

CSS.

JavaScript.

Iconos.

Imágenes de interfaz.

Los archivos media son generados o subidos durante el uso de la aplicación.

Los archivos de usuario deben tratarse como datos persistentes: requieren almacenamiento y respaldo adecuados.

4.9 Secuencia básica de despliegue

flowchart TD
    A[1. Obtener versión exacta del código] --> B[2. Preparar entorno]
    B --> C[3. Instalar dependencias]
    C --> D[4. Configurar variables y secretos]
    D --> E[5. Aplicar migraciones]
    E --> F[6. Preparar archivos estáticos]
    F --> G[7. Reiniciar o reemplazar procesos]
    G --> H[8. Verificar funcionalidad y logs]

Obtener la versión exacta del código que se desea publicar.

Preparar o validar el entorno de ejecución.

Instalar dependencias.

Configurar variables y secretos.

Aplicar migraciones de base de datos.

Preparar archivos estáticos si corresponde.

Reiniciar o reemplazar procesos de la aplicación.

Verificar funcionalidad y observar logs.

4.10 Ejemplo - Django + Gunicorn + Nginx

El siguiente esquema es deliberadamente resumido. Su finalidad es identificar responsabilidades.

flowchart LR
    U[Usuario] -->|HTTPS :443| N[Nginx]
    N -->|127.0.0.1:8000| G[Gunicorn]
    G --> D[Django]
    D <--> DB[(Base de datos)]
    N --> S[Static / Media según configuración]

Responsabilidades

Nginx: punto de entrada público, HTTPS, proxy inverso y archivos estáticos.

Gunicorn: ejecuta workers WSGI de la aplicación.

Django: resuelve URLs, vistas, reglas de negocio y acceso a datos mediante el ORM.

Base de datos: conserva información persistente.

4.11 Observabilidad, logs y recuperación

Logs: registran eventos útiles para entender qué ocurrió.

Health check: prueba o endpoint que confirma que el servicio responde.

Monitoreo: observa disponibilidad, recursos, errores y tiempos de respuesta.

Backup: copia recuperable de datos importantes.

Rollback: permite volver a una versión anterior ante una falla grave.

[!IMPORTANT]Un despliegue no termina cuando finaliza el comando de instalación. Termina cuando se verifica que la versión correcta funciona, los usuarios pueden acceder y existe una forma razonable de detectar y recuperar fallas.

Laboratorio de infraestructura

Sobre una aplicación web de práctica, diseñá una arquitectura de producción sin necesidad de implementarla todavía.

Indicá dónde escucha el servidor web y dónde escucha la aplicación.

Explicá qué servicio debería iniciar automáticamente.

Listá cinco variables de entorno que podrían variar entre desarrollo y producción.

Decidí cómo guardarías datos persistentes y dónde harías backups.

Definí una prueba concreta para verificar que el despliegue fue exitoso.

Dibujá la arquitectura completa incluyendo cliente, HTTPS, reverse proxy, aplicación y base de datos.

Unidad 5 - Proyecto integrador

5.1 Primer despliegue real y documentado

El proyecto integrador reúne los conceptos de la materia. El objetivo no es solamente que una aplicación quede "online", sino que el estudiante pueda:

Explicar cómo está desplegada.

Justificar decisiones.

Verificar su funcionamiento.

Diagnosticar fallas básicas.

Dejar documentación suficiente para repetir el proceso.

[!IMPORTANT]Desafío: tomar una aplicación full stack funcional en desarrollo y publicarla en un entorno de producción o preproducción accesible, aplicando una arquitectura de despliegue explícita y documentada.

5.2 Requisitos mínimos

Aplicación funcional con frontend, backend y persistencia de datos.

Repositorio con una versión identificable del código.

Entorno de despliegue definido: local, on-premise, VPS/nube o plataforma gestionada.

Configuración separada de los secretos del código fuente.

Servicio de aplicación que pueda reiniciarse de manera controlada.

Acceso de usuarios mediante una dirección estable.

Dominio y HTTPS cuando el entorno lo permita.

Procedimiento de backup o explicación concreta de recuperación.

Logs o mecanismo para diagnosticar errores.

Documento de arquitectura y pasos de despliegue.

5.3 Entregables

Entregable

Contenido

Evidencia

1. Diagrama de despliegue

Componentes, nodos, puertos y conexiones.

Diagrama incluido en el repositorio o informe.

2. Procedimiento

Pasos reproducibles desde código hasta servicio activo.

Comandos y explicaciones.

3. Configuración

Variables, servicios y dependencias sin exponer secretos.

Fragmentos sanitizados.

4. Verificación

Pruebas posteriores al despliegue.

HTTP 200, login, operación principal y logs.

5. Recuperación

Backup, restauración o rollback.

Procedimiento explicado y, si es posible, probado.

6. Defensa técnica

Justificación de decisiones y límites.

Presentación breve y preguntas.

5.4 Estructura sugerida de la documentación del proyecto

Descripción del sistema y usuarios.

Arquitectura lógica: frontend, backend y datos.

Arquitectura de despliegue: dónde se ejecuta cada componente.

Redes, dominio, puertos y flujo HTTP/HTTPS.

Infraestructura y servicios utilizados.

Variables de entorno y dependencias.

Procedimiento completo de despliegue.

Pruebas de verificación.

Logs, backup y recuperación.

Problemas encontrados, decisiones y mejoras futuras.

5.5 Rúbrica orientativa

Criterio

Qué se observa

Puntaje

Arquitectura

Diagrama correcto y relación entre componentes y nodos.

20

Despliegue funcional

Aplicación accesible y operación principal validada.

25

Infraestructura

Servicios, puertos, persistencia y configuración coherentes.

20

Seguridad básica

Secretos separados, HTTPS si aplica, permisos razonables.

10

Operación y recuperación

Logs, verificación, backup/rollback.

10

Documentación y defensa

Procedimiento reproducible y explicación técnica.

15



Total

100

5.6 Preguntas para la defensa

¿Qué componente recibe primero una solicitud del usuario?

¿Qué puertos están expuestos públicamente y cuáles son internos?

¿Qué ocurriría si la base de datos deja de estar disponible?

¿Dónde se encuentran los secretos y por qué no están en el repositorio?

¿Cómo reiniciás la aplicación sin reiniciar todo el servidor?

¿Cómo comprobás que la versión desplegada es la correcta?

¿Qué recuperarías primero ante una falla total?

¿Qué cambiarías si el sistema pasara de 20 a 20.000 usuarios?

Anexo A - Glosario esencial

Concepto

Definición

Arquitectura de despliegue

Representación de componentes, lugares de ejecución y conexiones.

Backend

Parte que procesa acciones y aplica reglas del sistema.

Base de datos

Sistema donde se conserva información persistente.

Contenedor

Proceso aislado que ejecuta una imagen con dependencias definidas.

Despliegue

Actividades para instalar, configurar y poner una aplicación en funcionamiento.

DNS

Sistema que resuelve nombres de dominio a direcciones de red.

Frontend

Parte visible e interactiva con la que trabaja el usuario.

HTTP

Protocolo de solicitud/respuesta utilizado por la web.

HTTPS

HTTP protegido mediante TLS.

IP

Dirección utilizada para identificar interfaces en una red.

Proceso

Programa en ejecución.

Producción

Entorno real utilizado por usuarios finales.

Proxy inverso

Servidor que recibe solicitudes y las reenvía a servicios internos.

Puerto

Número que identifica un servicio dentro de un equipo.

Servicio

Proceso administrado por el sistema operativo para ejecución continua.

Staging

Entorno de preproducción similar a producción.

TLS

Protocolo criptográfico que protege la comunicación.

Variable de entorno

Valor de configuración suministrado al proceso sin fijarlo en el código.

Anexo B - Comandos de consulta rápida

Red y HTTP

nslookup dominio.com
curl -I https://dominio.com
curl -v https://dominio.com/ruta
ss -lntp

Servicios Linux

systemctl status nombre.service
sudo systemctl restart nombre.service
journalctl -u nombre.service -n 100 --no-pager

Archivos y procesos

pwd
ls -lah
ps aux
top

Git

git status
git log --oneline -5
git pull

[!CAUTION]Antes de ejecutar un comando en producción, entendé qué cambia, qué permisos requiere y cómo volver atrás si el resultado no es el esperado.

Anexo C - Checklist de despliegue

Antes

La versión del código está identificada.

Existe backup de los datos importantes.

Las variables y secretos de producción están configurados.

Las dependencias están definidas.

Las migraciones necesarias fueron revisadas.

El servicio puede iniciarse y reiniciarse.

Después

La aplicación responde por la URL esperada.

La operación principal funciona con un usuario de prueba.

No aparecen errores críticos en los logs.

Los archivos estáticos y media se sirven correctamente.

La base de datos conserva los cambios.

Se registró qué versión quedó desplegada.

Anexo D - Plantilla de diagrama de despliegue

Para construir un diagrama inicial, respondé:

¿Qué dispositivos usan las personas?

¿Cuál es el punto de entrada público?

¿Qué dominio, IP y puertos intervienen?

¿Dónde se ejecuta el frontend?

¿Dónde se ejecuta el backend?

¿Dónde se almacenan los datos?

¿Qué conexiones son internas y cuáles atraviesan Internet?

¿Qué componente sirve archivos estáticos o media?

¿Qué servicios deben permanecer activos?

¿Qué partes deben respaldarse?

flowchart TB
    U[Usuario / Navegador] -->|HTTPS :443| W[Servidor web / Reverse proxy]
    W -->|Puerto interno| A[Servidor de aplicación / Backend]
    A <--> DB[(Base de datos)]
    A --> E[Servicios externos]
    W --> F[Archivos estáticos / media]

Cierre

Desplegar software es transformar una solución que existe en el entorno del desarrollador en un servicio que otras personas pueden usar, sostener y recuperar. La arquitectura de despliegue hace visible cómo se organiza esa transformación.

Fuentes de base del material inicial

Apunte de Clase 1: Del código al usuario - Introducción al despliegue de software.

Presentación de Clase 1: Del Código al Usuario.

Full Stack Open - Universidad de Helsinki, Fundamentos de las aplicaciones web.

The Open University, An introduction to web applications architecture.

The Open University, Approaches to software development.

Arquitectura y Despliegue de AplicacionesTecnicatura Universitaria en Programación Full Stack - Primer año
