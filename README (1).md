### Recuperacion parcial – Segundo Momento de Sistemas Operativos

**Pregunta 1.**
Una oficina contable almacena sus documentos en una carpeta compartida. Un script Bash comprime esa carpeta diariamente con tar y gzip, y guarda cada copia en un archivo diferente cuyo nombre incluye la fecha y la hora de ejecución. ¿Qué mecanismo de Bash permite incorporar la fecha y la hora en ese nombre?

**Respuesta: Opción B: La sustitución de comandos, que asigna a una variable la salida del comando date.**

**Justificación:** La sustitución de comandos permite ejecutar una instrucción y utilizar su resultado dentro de otra expresión. En Bash se puede utilizar fecha=$(date +%Y%m%d_%H%M%S) para guardar la fecha y la hora actuales en una variable. Después, esta variable se incorpora al nombre del archivo de respaldo, evitando que las copias tengan el mismo nombre.

------------------------------------------------------

**Pregunta 2.**
El script de respaldo agrega una línea con la fecha y el resultado de cada ejecución al final de un archivo de bitácora, conservando las líneas anteriores. ¿Qué operador de redirección cumple esta función?

**Respuesta: Opción B: El operador >>, que anexa la nueva línea después del contenido existente.**

**Justificación:** El operador >> agrega la salida de un comando al final de un archivo sin eliminar su contenido anterior. Esto permite conservar el historial de las ejecuciones del script. En cambio, el operador > reemplaza el contenido existente.

-----------------------------------------------------

**Pregunta 3**
Un administrador monitorea el uso del disco de un servidor Debian 13 mediante df -h. Después de obtener el porcentaje de uso como un número entero, el script debe generar una alerta cuando el valor supera el umbral de 85 %. ¿Qué estructura de control y operador de comparación permiten realizar esta verificación?

**Respuesta: Opción A: Una condicional if con el operador -gt, que compara valores numéricos enteros.**

**Justificación:** La estructura if permite ejecutar instrucciones cuando se cumple una condición. El operador -gt significa greater than (mayor que) y compara valores numéricos enteros. Por ejemplo, if [ "$uso" -gt 85 ]; then permite generar una alerta cuando el uso del disco supera el 85 %.

-----------------------------------------------------

**Pregunta 4**
El monitor de disco se ejecuta cada 30 minutos mediante cron. El archivo crontab utiliza cinco campos en este orden: minuto, hora, día del mes, mes y día de la semana. ¿Qué expresión programa la ejecución del script cada 30 minutos durante todo el día?

**Respuesta: Opción C: */30 * * * *, que ejecuta el script cada treinta minutos durante todo el día.**

**Justificación:** En una expresión de cron, el primer campo representa los minutos. El valor */30 indica que la tarea debe ejecutarse cada 30 minutos, es decir, en los minutos 0 y 30 de cada hora. Los cuatro asteriscos restantes permiten que se ejecute durante todas las horas, días, meses y días de la semana.

----------------------------------------------------

**Pregunta 5**
Una academia recibe un archivo CSV con una fila por estudiante y los campos usuario, nombre y grupo. Un script Bash debe leer el archivo fila por fila y separar cada campo en una variable para crear cuentas en lote. ¿Qué construcción permite realizar esa lectura?

**Respuesta: Opción D: Un bucle while read con IFS igual a coma y el archivo redirigido al bucle.**

**Justificación:** La variable IFS define los separadores utilizados por Bash para dividir los campos de una línea. Al establecer IFS=,, los datos separados por comas pueden asignarse a variables diferentes. El bucle while IFS=, read -r usuario nombre grupo; do ... done < archivo.csv permite procesar cada fila del archivo.

----------------------------------------------------

**Pregunta 6**
Un script Bash utiliza useradd para crear todas las cuentas listadas en un archivo CSV. Se necesita determinar si puede crear la totalidad de las cuentas. Se dispone de dos informaciones: I. El script se ejecuta con privilegios de superusuario. II. Cada nombre de usuario del archivo es distinto de los existentes en el sistema. ¿Qué información es suficiente para resolver el problema?

**Respuesta: Opción D: Ambas informaciones juntas son necesarias para resolver el problema.**

**Justificación:** Los privilegios de superusuario permiten ejecutar operaciones administrativas como la creación de usuarios. Sin embargo, tener esos privilegios no garantiza que los nombres estén disponibles. Por otra parte, que los nombres no existan previamente no garantiza que el script tenga permisos para crearlos. Por eso se necesitan ambas condiciones para concluir que el lote puede crearse correctamente, suponiendo que no se presentan otros errores.

---------------------------------------------------

**Pregunta 7**
Un script de mantenimiento en Debian 13 elimina archivos temporales antiguos utilizando find. Se analizan dos afirmaciones: 
1. La opción -mtime +7 selecciona archivos cuya última modificación ocurrió hace más de siete días. 
2. La opción -mtime evalúa la fecha del último acceso de lectura del archivo. ¿Cuál afirmación es verdadera?

**Respuesta: Opción A: Solo la afirmación 1 es verdadera.**

**Justificación:** La opción -mtime evalúa el tiempo transcurrido desde la última modificación del contenido del archivo, expresado en periodos de 24 horas. El signo + selecciona valores superiores al número indicado; debido al redondeo de find, -mtime +7 selecciona archivos cuyo tiempo de modificación tiene al menos ocho periodos completos de 24 horas. Para consultar el último acceso se utiliza -atime, no -mtime.

---------------------------------------------------

**Pregunta 8.**
Un script de limpieza ejecuta rm -rf sobre el contenido de una ruta almacenada en una variable. Se afirma que el script valida que la variable contenga texto antes de ejecutar rm -rf, porque una variable vacía puede convertir la ruta de borrado en la raíz del sistema de archivos. ¿Cómo se evalúan la afirmación y la razón?

**Respuesta: Opción C, según la intención de seguridad del enunciado: La afirmación y la razón son verdaderas, y la razón explica la afirmación.**

**Justificación:** Antes de realizar un borrado recursivo, es importante comprobar que la variable de ruta esté definida y contenga una ruta válida. Una variable vacía puede provocar borrados no deseados si se utiliza para construir una expresión como "$ruta"/*, pues el patrón podría terminar apuntando a archivos de la raíz. La validación evita operaciones destructivas accidentales. Técnicamente, una variable vacía no convierte por sí sola rm -rf "$ruta" en un borrado de la raíz; el riesgo depende de cómo se construya la ruta.

----------------------------------------------------

**Pregunta 9**
Un script supervisa el servicio de base de datos de un servidor Debian 13 y lo reinicia cuando se detiene. Para decidir, utiliza el código de salida de un comando de systemctl que consulta el estado del servicio. ¿Qué comando entrega ese código de salida?

**Respuesta: Opción B: systemctl is-active --quiet, cuyo código cero indica que el servicio está activo.**

**Justificación:** El comando systemctl is-active comprueba si un servicio se encuentra activo. Con la opción --quiet, no muestra el estado como texto y permite utilizar directamente su código de salida en una condicional. Un código de salida igual a cero indica que está activo; un valor distinto de cero indica que no se encuentra activo según la consulta.


--------------------------------------------------

**Pregunta 10.**
El script de monitoreo realiza cuatro acciones: I. Consultar con systemctl el estado del servicio. II. Reiniciar el servicio si está detenido. III. Escribir en la bitácora el resultado final con fecha y hora. IV. Comprobar nuevamente el estado después del reinicio. ¿Cuál es la secuencia lógica?

**Respuesta: Opción D: I, II, IV, III.**

**Justificación:** Primero se consulta el estado del servicio para determinar si necesita reiniciarse. Si está detenido, se ejecuta el reinicio. Después se comprueba nuevamente el estado para verificar si la operación funcionó. Finalmente, se registra en la bitácora el resultado obtenido, dejando evidencia del procedimiento.


-------------------------------------------------

**Pregunta 11**
En un equipo Debian 13 con rsyslog, el archivo /var/log/syslog registra eventos del sistema. Un script diario cuenta cuántas líneas contienen la palabra error, sin distinguir entre mayúsculas y minúsculas. ¿Qué línea de comandos realiza ese conteo?

**Respuesta: Opción A: grep -i error archivo | wc -l, que filtra las líneas y cuenta cuántas resultan.**

**Justificación:** La opción -i de grep permite buscar sin distinguir entre mayúsculas y minúsculas. El operador | envía las líneas encontradas al comando wc -l, que cuenta las líneas recibidas. Para el archivo del enunciado, se puede utilizar grep -i error /var/log/syslog | wc -l.

------------------------------------------------

**Pregunta 12**
Un reporte diario debe mostrar el nombre del proceso que genera cada evento. En las líneas procesadas, separadas por espacios, el nombre ocupa el quinto campo. ¿Qué expresión de awk imprime únicamente ese dato?

**Respuesta: Opción B: Utilizar la acción print aplicada a la variable de campo $5.**

**Justificación:** awk divide cada línea en campos y permite acceder a ellos mediante variables como $1, $2 y $5. La expresión awk '{print $5}' archivo imprime únicamente el quinto campo de cada línea, que corresponde al nombre del proceso según el enunciado.

------------------------------------------------

**Pregunta 13.**
Un script lee una lista de paquetes desde un archivo de texto e instala cada uno mediante apt-get en Debian 13. Para ejecutarse automáticamente requiere privilegios de superusuario y aceptación automática de las confirmaciones. ¿Qué combinación cumple ambos propósitos?

**Respuesta: Opción C: Anteponer sudo y agregar la opción -y para aceptar las confirmaciones.**

**Justificación:** sudo permite ejecutar el comando con privilegios administrativos cuando el usuario está autorizado. La opción -y de apt-get responde afirmativamente a las preguntas de confirmación durante la instalación. Por ejemplo, sudo apt-get install -y nombre_paquete permite instalar un paquete sin solicitar confirmación interactiva, aunque la autenticación de sudo puede seguir siendo necesaria.


-----------------------------------------------

**Pregunta 14.**
El script de aprovisionamiento registra cada instalación como exitosa o fallida en la bitácora, aprovechando el estado de salida del comando de instalación en una sola línea de código. ¿Qué encadenamiento de operadores registra cada resultado?

**Respuesta: Opción A: Éxito registrado con && y fallo registrado con ||, ambos después de la instalación.**

**Justificación:** El operador && ejecuta el siguiente comando cuando el anterior termina correctamente, mientras que || ejecuta el siguiente comando cuando el anterior falla. De esta forma se puede escribir comando_instalacion && echo "Éxito" >> log.txt || echo "Fallo" >> log.txt. Este encadenamiento es el esperado en el ejercicio, aunque para garantizar un registro totalmente fiable ante posibles fallos del propio comando de registro conviene utilizar una estructura if/else.

-----------------------------------------------------

**Pregunta 15.**
Recorrido de arreglos en Bash

Un técnico de redes almacena las direcciones de varios equipos en un arreglo llamado hosts y diseña un script que ejecuta ping sobre cada dirección. ¿Qué expresión de bucle recorre los elementos individuales del arreglo?

**Respuesta: Opción B: Un bucle for sobre "${hosts[@]}", que entrega cada elemento por separado.**

**Justificación:** La expansión "${hosts[@]}" permite recorrer cada elemento del arreglo como un argumento independiente, conservando los espacios que pueda contener un elemento. Por ejemplo, for host in "${hosts[@]}"; do ping -c 1 "$host"; done ejecuta la instrucción para cada dirección almacenada.

-----------------------------------------------------

**Pregunta 16.**
Evaluación del resultado de ping

Un script ejecuta ping -c 1 dentro de una condicional if. El comando retorna cero cuando recibe respuesta y un valor distinto de cero cuando vence el tiempo de espera. La rama then registra el host como disponible y la rama else como inaccesible. Si el host está apagado, ¿qué ocurre?

**Respuesta: Opción D: La rama else se ejecuta y el host queda registrado como inaccesible.**

**Justificación:** En Bash, una condición if evalúa el código de salida del comando ejecutado. Si ping no recibe respuesta y devuelve un valor distinto de cero, la condición falla y se ejecuta la rama else. Por tanto, el script registra el host como inaccesible.

-----------------------------------------------------

**Pregunta 17.**
Agrupación de patrones en case

Un script organiza la carpeta de descargas de Debian 13. Debe mover las imágenes con extensión .jpg, .png o .gif hacia un mismo subdirectorio, evaluando cada nombre mediante una estructura case. ¿Qué patrón agrupa las tres extensiones en una sola rama?

**Respuesta: Opción C: *.jpg|*.png|*.gif, seguido de ) y los comandos correspondientes.**

**Justificación:** En una estructura case de Bash, el símbolo | permite separar varios patrones alternativos que comparten las mismas instrucciones. Por ejemplo, *.jpg|*.png|*.gif) ... ;; permite ejecutar una misma acción para los archivos que coincidan con cualquiera de las tres extensiones.

-----------------------------------------------------

**Pregunta 18.**
Limpieza automática de archivos temporales

Un script de respaldo ejecuta mysqldump y escribe un archivo temporal. Si el script termina por un error o por una interrupción del usuario, el archivo temporal incompleto debe eliminarse. ¿Qué técnica de Bash permite asociar una función de limpieza con la finalización del script?

**Respuesta: Opción A: Registrar con trap una función de limpieza asociada a la pseudoseñal EXIT.**

**Justificación:** El comando trap permite ejecutar una acción cuando se produce una señal o un evento de salida del shell. Asociar una función de limpieza con EXIT hace que se ejecute cuando el script termina, lo que permite eliminar archivos temporales. Por ejemplo, trap 'rm -f -- "$temporal"' EXIT. Esta técnica no puede ejecutarse si el proceso termina mediante señales que no se pueden capturar, como SIGKILL.

-----------------------------------------------------

**Pregunta 19.**
Rotación de copias de seguridad

Un script de respaldo conserva únicamente las N copias más recientes del directorio de destino y elimina las más antiguas. ¿Qué procedimiento aplica esa rotación?

**Respuesta: Opción B: Ordenar con ls -t, saltar las N primeras con tail y borrar las restantes.**

**Justificación:** La opción -t de ls ordena los archivos por fecha de modificación, colocando primero los más recientes. Al omitir las primeras N entradas, se pueden identificar las copias más antiguas para eliminarlas y conservar las más recientes. En una implementación real se recomienda utilizar una lista segura de archivos y comprobar cuidadosamente las rutas antes de borrarlas, en lugar de depender de ls para procesar nombres arbitrarios.

-----------------------------------------------------

**Pregunta 20.**
Identificación de cuentas con UID cero

Un auditor revisa un servidor Debian 13. Cada línea de /etc/passwd contiene siete campos separados por dos puntos, entre ellos el UID y el GID. El script debe listar las cuentas con privilegios equivalentes a root, que poseen UID igual a cero. ¿Qué filtro de awk lo logra?

**Respuesta: Opción C: Utilizar awk con separador de dos puntos y una condición sobre el campo 3 igual a cero.**

**Justificación:** En /etc/passwd, los campos están separados por dos puntos. El primer campo contiene el nombre de usuario, el segundo la contraseña representada o su marcador, el tercero el UID y el cuarto el GID. Por eso, awk -F: '$3 == 0' /etc/passwd selecciona las cuentas cuyo UID es cero.

-----------------------------------------------------

**Pregunta 21.**
Consulta de sockets de red con ss

Un auditor genera un informe de puertos abiertos con el comando ss -tuln. ¿Qué información entrega ese comando?

**Respuesta: Opción D: Sockets TCP y UDP en escucha, con puertos expresados en forma numérica.**

**Justificación:** La opción -t incluye sockets TCP, -u incluye sockets UDP, -l muestra sockets en estado de escucha y -n presenta las direcciones y los puertos numéricamente, sin resolver nombres de servicios. Es útil para revisar los puertos en los que el servidor está escuchando. El comando no muestra por sí solo los procesos propietarios, para lo cual se puede utilizar, por ejemplo, ss -tulnp con los permisos adecuados.

-----------------------------------------------------

**Pregunta 22.**
Conversión de campos a CSV con awk

Un script captura mediante ps las columnas PID, nombre y porcentaje de CPU, separadas por espacios, y debe exportarlas a un archivo CSV. awk reescribe cada línea utilizando comas entre los campos. ¿Qué recurso de awk realiza esa conversión?

**Respuesta: Opción A: Utilizar la variable OFS igual a coma y la asignación $1=$1 para reconstruir la línea.**

**Justificación:** La variable OFS (Output Field Separator) establece el separador que awk utiliza entre los campos al reconstruir una línea. Al asignar OFS="," y ejecutar $1=$1, se fuerza la reconstrucción de los campos utilizando comas. Por ejemplo, awk 'BEGIN {OFS=","} {$1=$1; print}' archivo.txt. Si los nombres contienen espacios o comas, se necesita un tratamiento adicional para generar un CSV válido.

-----------------------------------------------------

**Pregunta 23.**
Inicio y habilitación de Apache

Un script de despliegue instala el servidor web Apache en Debian 13 y crea un sitio de prueba. Al terminar, el servicio debe quedar iniciado inmediatamente y habilitado para cada arranque del sistema mediante un único comando de systemctl. ¿Cuál comando cumple ambos propósitos?

**Respuesta: Opción D: systemctl enable --now apache2.**

**Justificación:** El comando systemctl enable configura el servicio para que se inicie automáticamente durante el arranque del sistema. La opción --now también inicia el servicio inmediatamente. Por eso, sudo systemctl enable --now apache2 cumple los dos objetivos en una sola instrucción.

-----------------------------------------------------

**Pregunta 24.**
Validación de parámetros posicionales

Un script de sincronización con rsync recibe dos parámetros posicionales: la ruta de origen y la ruta de destino. Antes de copiar, valida que el usuario haya suministrado exactamente esos dos argumentos. ¿Qué validación inicial debe aplicar?

**Respuesta: Opción A: Comparar $# con el valor 2 y terminar con exit 1 si difiere.**

**Justificación:** En Bash, la variable especial $# contiene la cantidad de argumentos posicionales recibidos por el script. La expresión if [ "$#" -ne 2 ]; then exit 1; fi comprueba que se hayan proporcionado exactamente dos argumentos. Si la cantidad es diferente, el script termina con código de salida 1 para indicar que hubo un error de uso.

-----------------------------------------------------

**Pregunta 25.**
Prompt de la instrucción select

Un script presenta en Terminator un menú numerado con tareas de respaldo, limpieza y monitoreo mediante la instrucción select. El texto que invita al usuario a elegir un número se define con una variable especial de indicador de entrada. ¿Cuál variable controla ese texto?

**Respuesta: Opción C: PS3, que almacena el texto con el que select solicita la opción.**

**Justificación:** En Bash, la variable PS3 define el mensaje que se muestra al solicitar una opción dentro de un menú creado con select. Antes de ejecutar el menú se puede establecer, por ejemplo, PS3="Seleccione una opción: ". Cuando se ejecuta select, Bash muestra ese texto para solicitar la selección del usuario.


### Trabajo realizado por el grupo de: Juan Jose Acevedo Melo - Juan Pablo Diaz Perez - Randhy Jair Lugo Moreno