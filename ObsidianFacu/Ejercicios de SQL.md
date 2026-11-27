### Parcial 5/11/25 primera fecha, tema1
``` SQL
DELIMITER//
CREATE TRIGGER facturar
AFTER UPDATE ON VIAJE 
BEGIN
	IF OLD.finalizado = TRUE AND NEW.finalizado = FALSE THEN
		INSERT INTO FACTURA (id_viaje, monto, fecha_emision) 
		VALUES (NEW.id_viaje, NEW.costo_viaje, NEW.fecha_fin);
	END IF;
END;
DELIMITER; 
```

### Parcial 5/11/25 primera fecha, tema 2
``` SQL
DELIMITER//
CREATE TRIGGER actualizar_stock
AFTER INSERT ON DEVOLUCION
FOR EACH ROW
BEGIN
	DECLARE cant_por_caja INT;
	DECLARE total INT; 
	
	SELECT p.unidades_por_caja INTO cant_por_caja
	FROM PRODUCTO p
	WHERE p.id_producto = NEW.id_producto;

	SET total = cant_por_caja * NEW.cantidad_cajas;
	
	UPDATE INVENTARIO i
	SET i.stock_unidades = i.stock_unidades + total
	WHERE i.id_producto = NEW.id_producto 
		AND i.id_bodega = NEW.id_bodega;
	
END;
DELIMITER;
```

### Parcial 2026 primera fecha tema 1
``` SQL 
DELIMITER //
CREATE TRIGGER actualizar_resumen
AFTER INSERT 
ON MEDICION_MONITOREO FOR EACH ROW
BEGIN
	DECLARE anomalia INT
	IF (NEW.supera_limite) THEN
		SET anomalia= 1;
	ELSE
		SET anomalia= 0;
	
	UPDATE RESUMEN_POZO 
	SET (total_monitoreos= total_monitoreos+1, 
		anomalias_detectadas= anomalias_detectadas+anomalia,
		ultima_actualizacion= NOW())
	WHERE NEW.id_pozo = r.id_pozo
END;
DELIMITER;
```

### Parcial 2025 Segunda fecha 
``` SQL
DELIMITER//
CREATE PROCEDURE vender (IN #producto INT, IN cant INT, IN #cliente INT)
BEGIN
	DECLARE stock INT;
	DECLARE precio_unit INT;
	DECLARE precio_total INT;
	
	DECLARE EXIT HANDLER FOR SQLEXCEPTION
	BEGIN
		ROLLBACK;
	END;
	
	START TRANSACTION 
		SELECT p.stock_actual INTO stock, p.precio INTO precio_unit
		FROM PRODUCTO p
		WHERE p.#producto = #producto
		FOR UPDATE;
		
		IF (stock-cant >= 0) THEN BEGIN
			SET precio_total= precio_unit * cant;
			
			INSERT INTO VENTA (producto, cantidad, monto, #cliente)
			VALUES (#producto, cant, precio_total, #cliente)
			
			UPDATE PRODUCTO 
			SET stock_actual= stock_actual-stock;
			WHERE p.#producto = #producto;
			
			COMMIT;
		ELSE
			ROLLBACK;
		END IF;
END;
DELIMITER; 
```

### Parcial 2025 tercera fecha
``` SQL
DELIMITER//
CREATE TRIGGER actualizar_puntos
AFTER INSERT ON RENOVACION
FOR EACH ROW
BEGIN
	DECLARE puntos INT
	
	SELECT p.puntaje_otorgado INTO puntos
	FROM PLAN p
	WHERE p.id_plan = NEW.id_plan;
	
	UPDATE SUSCRIPCION 
	SET 
		(puntaje_obtenido= puntaje_obtenido + puntos,
		cant_renovaciones= cant_renovaciones + 1)
	WHERE id_suscripcion = NEW.id_suscripcion;
END;
DELIMITER;
```

---
## Dudas
- Para poner fecha y hora actual solo hay que poner NOW() ?
- Hay alguna otra función usada seguido como la que te da fecha y hora actual?
  CURRENT_USER da el nombre. Hay otra para obtener nombre y otra cosa.
- Cómo se manejan los INSERT cuando hay datos autoincrementales? 
  Puedo obtener el ultimo ID en una variable
  DECLARE variable INT;
  SET variable= LAST_INSERT_ID();

#### Triggers
CREATE TRIGGER nombre
AFTER|BEFORE ACCION ON tabla
FOR EACH ROW
BEGIN
	//Codigo
END;

#### SP Store Procedures
CREATE PROCEDURE nombre (IN/OUT nombreParam TIPO)
BEGIN
	DECLARE var1
	DECLARE EXIT HANDLER FOR SQLEXCEPTION
		ROLLBACK
	END;

	//Código
END;

#### Transacciones
Tienen q tener el exit handler por si falla, con un rollback
Dsp del START TRANSACTION siempre tiene que haber un COMMIT o ROLLBACK. Si hay un if para hacer la transaccion, en los dos casos del if se debe hacer una de las dos opciones (commit o rollback)

En los SELECT puedo bloquear filas con FOR UPDATE después del WHERE. Se bloquea durante toda la transaccion, no?