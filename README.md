## 1. ¿Qué es un Trigger?

Un **trigger** (o disparador) es un objeto de base de datos que se activa automáticamente en respuesta a un evento específico en una tabla. Piensa en él como un "guardián" silencioso que observa tu base de datos y, cuando algo predefinido ocurre (como una inserción, actualización o eliminación de datos), ejecuta un bloque de código SQL sin que tengas que pedirlo explícitamente.

## 2. ¿Para qué sirve un Trigger?

Los triggers son muy úctiles para **automatizar tareas** y mantener la **integridad de los datos** en tu base de datos. Sirven para:

* **Aplicar reglas de negocio complejas**
* **Auditar datos**
* **Mantener la integridad referencial**
* **Replicar datos**
* **Generar valores automáticos**

## 3. ¿Cuándo se puede usar un Trigger?

Puedes usar triggers cuando necesites una acción automática en respuesta a:

* **Eventos DML** (`INSERT`, `UPDATE`, `DELETE`)
* **Eventos DDL** (como `CREATE`, `ALTER`, `DROP`)
* **Eventos de base de datos** (inicio/cierre de sesión, etc.)

## 4. ¿Cuál es la estructura general de un Trigger?

```sql
CREATE TRIGGER nombre_del_trigger
ON nombre_de_la_tabla
[FOR | AFTER | INSTEAD OF] [INSERT | UPDATE | DELETE]
AS
BEGIN
    -- Código SQL a ejecutar
END;
```
![image](https://github.com/user-attachments/assets/7e83dfa7-18ac-48f6-a791-48b9fc96d3ee)


## 5. ¿Qué tipos de Trigger existen?

**Por momento de ejecución:**

* `BEFORE` Trigger
* `AFTER` Trigger
* `INSTEAD OF` Trigger

**Por nivel de ejecución:**

* Trigger a nivel de fila (`ROW-level`)
* Trigger a nivel de sentencia (`STATEMENT-level`)

![image](https://github.com/user-attachments/assets/0414e6dd-1732-4f8d-9acc-3ec56b1df080)


## 6. ¿Cuándo se ejecuta un Trigger?

Un trigger ejecuta su código exactamente en el momento configurado (`BEFORE`, `AFTER`, `INSTEAD OF`) y solo cuando ocurre el evento (`INSERT`, `UPDATE`, `DELETE`).

## 7. ¿Cómo crear un ejemplo de Trigger en MySQL?

```sql
CREATE TABLE productos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    precio DECIMAL(10, 2),
    ultima_actualizacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE auditoria_precios (
    id_auditoria INT PRIMARY KEY AUTO_INCREMENT,
    id_producto INT,
    precio_antiguo DECIMAL(10, 2),
    precio_nuevo DECIMAL(10, 2),
    fecha_cambio DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_producto) REFERENCES productos(id)
);

DELIMITER //
CREATE TRIGGER trg_auditar_precio_producto
AFTER UPDATE ON productos
FOR EACH ROW
BEGIN
    IF OLD.precio <> NEW.precio THEN
        INSERT INTO auditoria_precios (id_producto, precio_antiguo, precio_nuevo)
        VALUES (OLD.id, OLD.precio, NEW.precio);
    END IF;
END;
//
DELIMITER ;
```

## 8. ¿Qué es un Trigger en Marketing?

En marketing, un **trigger** es un evento que dispara una acción automatizada hacia un cliente (como un correo por abandono de carrito, cumpleaños, etc.).

## 9. ¿Cómo se crea un Trigger paso a paso?

1. Define tu necesidad.
2. Elige tabla y evento (`INSERT`, `UPDATE`, `DELETE`).
3. Decide el momento (`BEFORE`, `AFTER`, `INSTEAD OF`).
4. Escribe la lógica SQL.
5. Prueba cuidadosamente.
6. Despliega el trigger.

## 10. ¿Cuáles son las características de un Trigger?

* Automatización
* Transaccionalidad
* Transparencia para la aplicación
* Integridad de datos
* Capacidad de lógica compleja

## 11. ¿Cuáles son las desventajas de usar Triggers?

* Difícil de depurar
* Impacto en rendimiento
* Complejidad y mantenimiento
* Dependencias ocultas
* Portabilidad limitada
* Riesgo de cascadas

## 12. ¿Qué alternativas existen a los Triggers en otras bases de datos?

* **Procedimientos almacenados**
* **Lógica a nivel de aplicación**
* **Colas de mensajes (Kafka, RabbitMQ)**
* **Change Data Capture (CDC)**
* **Hooks y callbacks en frameworks ORM**
