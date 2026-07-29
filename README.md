# CampusCar - Sistema de Gestión de Concesionario

Proyecto de base de datos para el caso de estudio **(4H) Concesionario CampusCar**.

El objetivo es diseñar una base de datos que permita gestionar el inventario de vehículos, clientes, vendedores, ventas y servicios de mantenimiento de un concesionario.

---

## Contenido del repositorio

| Archivo | Descripción |
|---------|-------------|
| `diagrama_relacional.drawio` | Diagrama lógico con las tablas, claves primarias y foráneas |
| `diagrama_chen.drawio` | Diagrama entidad-relación en notación Chen (rectángulos + rombos) |
| `Documentacion_CampusCar.pdf` | Documentación del diseño (justificación, restricciones y relaciones) |

---

## Modelo de datos

Se definieron **6 tablas**:

### 1. Clientes
Almacena la información de las personas que compran vehículos o solicitan servicios.

- `id_cliente` (PK)
- `nombre`
- `telefono`
- `email` (UNIQUE)
- `direccion`

### 2. Vendedores
Datos de los empleados que realizan las ventas.

- `id_vendedor` (PK)
- `nombre`
- `numero_empleado` (UNIQUE)
- `fecha_contratacion`

### 3. Vehículos
Inventario del concesionario.

- `id_vehiculo` (PK)
- `marca`
- `modelo`
- `anio`
- `vin` (UNIQUE)
- `precio`
- `color`
- `tipo_combustible`
- `transmision`
- `estado` (nuevo / usado)
- `disponibilidad`

### 4. Ventas
Cabecera de cada transacción de venta.

- `id_venta` (PK)
- `id_cliente` (FK → Clientes)
- `id_vendedor` (FK → Vendedores)
- `fecha_venta`
- `total_transaccion`
- `metodo_pago`

### 5. Detalle_ventas
Tabla puente para relacionar ventas con vehículos (relación N:M).  
Permite que una misma venta incluya uno o varios vehículos.

- `id_detalle` (PK)
- `id_venta` (FK → Ventas)
- `id_vehiculo` (FK → Vehículos)
- `precio_venta`

### 6. Mantenimiento
Registro de los servicios técnicos realizados a los vehículos.

- `id_mantenimiento` (PK)
- `id_vehiculo` (FK → Vehículos)
- `id_cliente` (FK → Clientes, **opcional**)
- `tipo_servicio`
- `costo`
- `fecha_servicio`

> **Nota:** El campo `id_cliente` en Mantenimiento es opcional porque un vehículo en stock también puede recibir mantenimiento sin tener un cliente asociado.

---

## Relaciones principales

| Relación | Cardinalidad | Descripción |
|----------|--------------|-------------|
| Clientes → Ventas | 1 : N | Un cliente puede realizar varias ventas |
| Vendedores → Ventas | 1 : N | Un vendedor puede atender varias ventas |
| Ventas → Detalle_ventas | 1 : N | Una venta puede incluir varios vehículos |
| Vehículos → Detalle_ventas | 1 : N | Un vehículo puede aparecer en varios detalles de venta |
| Vehículos → Mantenimiento | 1 : N | Un vehículo puede tener varios servicios |
| Clientes → Mantenimiento | 1 : 0..N | Un cliente puede solicitar cero o varios servicios |

---

## Decisiones de diseño

- Se creó la tabla **Detalle_ventas** para resolver la relación de muchos a muchos entre Ventas y Vehículos. De esta forma una sola venta puede agrupar varios autos sin duplicar la información de la cabecera.
- El campo `disponibilidad` se separó de `estado`. El estado indica si el vehículo es nuevo o usado, mientras que la disponibilidad indica si está disponible para la venta, ya vendido o en mantenimiento.
- El VIN se definió como UNIQUE porque es el identificador único de cada unidad.
- El email del cliente y el número de empleado del vendedor también se marcaron como UNIQUE para evitar duplicados.

---

## Restricciones implementadas

- **Claves primarias** en todas las tablas (`id_*`)
- **Claves foráneas** que mantienen la integridad referencial
- **UNIQUE** en:
  - `Clientes.email`
  - `Vendedores.numero_empleado`
  - `Vehiculos.vin`
- Cuando un vehículo se vende, su disponibilidad debe actualizarse a "no disponible"

---

## Cómo abrir los diagramas

1. Abrir [draw.io](https://app.diagrams.net/)
2. Ir a **Archivo → Abrir desde → Dispositivo**
3. Seleccionar el archivo `.drawio` correspondiente

---

## Documentación

El archivo `Documentacion_CampusCar.pdf` contiene:

1. Justificación del diseño de las tablas y relaciones
2. Descripción de las claves primarias, foráneas y restricciones de unicidad
3. Explicación de las relaciones y sus cardinalidades

---

## Requisitos cubiertos

- [x] Gestión de vehículos (marca, modelo, año, VIN, precio, color, combustible, transmisión, estado y disponibilidad)
- [x] Gestión de clientes
- [x] Gestión de vendedores
- [x] Registro de ventas (cliente + vendedor + uno o más vehículos)
- [x] Servicios de mantenimiento (con o sin cliente asociado)
- [x] VIN único
- [x] Actualización de disponibilidad al vender un vehículo
