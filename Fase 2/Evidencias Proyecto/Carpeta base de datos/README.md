# Base de datos de AutoGest Aconcagua

Entrega de la **estructura PostgreSQL** correspondiente al código actual del proyecto, preparada el 19 de octubre de 2026.

## Archivo incluido

`autogest_estructura_postgresql.sql` crea 12 tablas con sus claves, relaciones e índices. Incluye el historial de 22 migraciones de Django para que el proyecto reconozca la estructura importada.

El script fue generado con el editor de esquemas PostgreSQL de Django a partir de los modelos actuales. **No es un respaldo ni una exportación de la base de producción de Railway.** No contiene cuentas, contraseñas, sesiones ni datos comerciales reales. Los únicos registros INSERT corresponden al historial técnico de migraciones.

## Qué contiene

- `clientes_cliente`: contacto y preferencias de búsqueda del cliente.
- `vehiculos_vehiculo`: datos del vehículo, precios, estado y referencia a su fotografía.
- Tablas de Django para usuarios, grupos, permisos, sesiones, tipos de contenido, administración e historial de migraciones.

La app `finanzas` todavía no define modelos, por lo que esta entrega no incluye tablas de ventas o gastos. Clientes y vehículos aún no tienen una relación comercial entre sí: esa relación corresponderá al modelo de venta cuando se implemente.

Las imágenes son archivos externos. La columna `imagen` guarda su ruta y no incluye el contenido de la fotografía.

## Importación con PostgreSQL

Usar PostgreSQL 14 o superior y una base de datos **nueva y vacía**, con codificación UTF-8. No ejecutar el script sobre una base existente ni sobre producción.

En pgAdmin:

1. Crear una base llamada `autogest_entrega`.
2. Seleccionarla y abrir Query Tool.
3. Abrir `autogest_estructura_postgresql.sql` y ejecutar el script completo.
4. Actualizar la vista de tablas del esquema `public`.

O, desde una terminal que tenga PostgreSQL instalado:

```powershell
createdb -U postgres autogest_entrega
psql -U postgres -d autogest_entrega -v ON_ERROR_STOP=1 -f "autogest_estructura_postgresql.sql"
```

Los comandos pedirán la contraseña del servidor del evaluador. No se proporciona una contraseña predeterminada.

## Conexión con el proyecto Django

Desde `backend`, con el entorno virtual activo y las dependencias instaladas, definir las variables para la base recién creada. Sustituir los valores de ejemplo y codificar los caracteres especiales de la contraseña para una URL:

```powershell
$env:DEBUG = "true"
$env:DATABASE_URL = "postgresql://USUARIO:CONTRASENA@localhost:5432/autogest_entrega"
python manage.py migrate --noinput
python manage.py createsuperuser
python manage.py runserver
```

`migrate` sincroniza los tipos de contenido y permisos de Django. `createsuperuser` permite al evaluador crear su propia cuenta de acceso.

Si se prefiere crear la estructura directamente con el código, se puede **omitir la importación del SQL** y ejecutar `python manage.py migrate` sobre una base vacía.

## Alcance de la verificación

La estructura se generó mediante Django y se revisaron las tablas, referencias y migraciones incluidas. No se ejecutó esta importación en un servidor PostgreSQL durante la preparación. El script no modifica la base de Railway.
