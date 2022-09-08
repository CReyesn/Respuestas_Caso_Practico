# Respuestas_Caso_Practico

Adjunto base de datos que cree para poder comprobar las consultas, tambien agrego las respuestas aquí:

1.- Por Paquete:

SELECT 
    proveedor.id,
    proveedor.nombre,
    SUM(transaccion.total)
FROM 
    proveedor
INNER JOIN 
    actividad,
    actividad_evento,
    item,
    transaccion,
    paquete
WHERE 
    proveedor.id=actividad.proveedor_id &&
    actividad.id=actividad_evento.actividad_id &&
    actividad_evento.id=item.evento_id &&
    transaccion.id=item.transaccion_id &&
    item.id=paquete.item_id
GROUP BY 
    proveedor.id;


Por Entrada:

SELECT
    proveedor.id,
    proveedor.nombre,
    SUM(transaccion.total) 
FROM proveedor 
INNER JOIN 
    actividad,
    actividad_evento,
    item,
    transaccion,
    entrada 
WHERE
    proveedor.id=actividad.proveedor_id && 
    actividad.id=actividad_evento.actividad_id && 
    actividad_evento.id=item.evento_id && 
    transaccion.id=item.transaccion_id && 
    item.id=entrada.item_id 
GROUP BY 
    proveedor.id;


2.- Podemos crear un proceso de automatización para hacer comparaciones entre las fechas de creación de la tabla transacción y
la fecha de creación de item, en caso de que la diferencia entre ambas fechas de creación se mayor a un cierto delta de tiempo 
(por ejemplo 1 día) el sistema envía un alerta, esto significa que desde que el usuario hizo el pago y que se procesa su pedido
e ingresa al sistema hay un excedente de tiempo, lo que se traduce finalmente en un retraso de toda la cadena y finalmente mayor
tiempo de espera para el cliente. 


3.- 

	public void getAllData() {
	  // le asignamos a una lista de transacciones la informacion obtenida desde la base de datos de la tabla transacción
	  List<Transaccion> ltsTransaccion=transaccionRepository.findAll();

	  // le asignamos a una lista de transacciones la informacion obtenida desde la base de datos de la tabla transacción
	  List<Item> ltsItem=itemRepository.findAll();

	     // recorremos la lista de item
	     for (List<Item> i : ltsItem) {

		// recorre la lista de transaccion  	
		for (List<Transaccion> t : ltsTransaccion) {  

		   // condicion: si transaccion_id=id y created de la tabla item es mayor a 1 dia de created de la tabla transaccion
		   if(i.getTransaccionId()==t.getId() && i.getCreated()>t.getCreated()+(1000 * 60 * 60 * 24)) { 

			logger.trace("Cretead of list item exceed over 1 day, item id:"+i.getId()+" transacion id:"+t.getId());
				}
			}			
		}
	}
