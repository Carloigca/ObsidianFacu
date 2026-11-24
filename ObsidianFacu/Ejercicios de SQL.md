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
```

```
### **Código SQL del Trigger**

#### **Opción 1: Con estructura de control condicional (** **IF ... THEN ... ELSE** **)**

Esta es la forma procedimental más clara y estándar para exámenes de la cátedra:

```
DELIMITER //

CREATE TRIGGER trg_actualizar_resumen_monitoreo
AFTER INSERT ON MEDICION_MONITOREO
FOR EACH ROW
BEGIN
    IF NEW.supera_limite = 'T' THEN
        -- Si superó el límite, incrementa tanto monitoreos como anomalías
        UPDATE RESUMEN_POZO
        SET total_monitoreos = total_monitoreos + 1,
            anomalias_detectadas = anomalias_detectadas + 1,
            ultima_actualizacion = NOW()
        WHERE id_pozo = NEW.id_pozo;
    ELSE
        -- Si no superó el límite, solo incrementa el total de monitoreos
        UPDATE RESUMEN_POZO
        SET total_monitoreos = total_monitoreos + 1,
            ultima_actualizacion = NOW()
        WHERE id_pozo = NEW.id_pozo;
    END IF;
END //

DELIMITER ;

```

---

#### **Opción 2: Simplificada en un único** **UPDATE** **(usando** **IF()** **inline)**

También puedes resolverlo en una sola sentencia SQL aprovechando la función condicional `IF(condición, valor_si_verdadero, valor_si_falso)`:

```
DELIMITER //

CREATE TRIGGER trg_actualizar_resumen_monitoreo
AFTER INSERT ON MEDICION_MONITOREO
FOR EACH ROW
BEGIN
    UPDATE RESUMEN_POZO
    SET total_monitoreos = total_monitoreos + 1,
        anomalias_detectadas = anomalias_detectadas + IF(NEW.supera_limite = 'T', 1, 0),
        ultima_actualizacion = NOW()
    WHERE id_pozo = NEW.id_pozo;
END //

DELIMITER ;

```

---
## Dudas
- Para poner fecha y hora actual solo hay que poner NOW() ?
- Hay alguna otra función usada seguido como la que te da fecha y hora actual?
- 