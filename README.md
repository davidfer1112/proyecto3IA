Para ejecutar el proyecto se deben tener las librerías de requierements, las cuales pueden instalarse con el siguiente comando:

```cmd
pip install -r requirements.txt
```

El proyecto, es un modelo sencillo de redes bayesianas con un motor de inferencia por enumeración y cuenta con dos casos de ejemplo.

Para crear los propios casos de ejemplo, cada modelo se vale de un .json con la siguiente configuración:

```json
{
    "<nombre_nodo>" : ["<path_nodo>", ["<nodo_dependencia_1>", "<nodo_dependencia_2>", ... , "<nodo_dependencia_n>" ], ["<nodo_apuntado_1>","<nodo_apuntado_2>", ... , "<nodo_apuntado_n>], ["<nombre_estado_0>: 0","<nombre_estado_1>: 1", ... , "<nombre_estado_n>: n"] ], 
}
```

Ejemplo:

```json
{
    "R": ["data/ejemplo_clase/rain.csv", [], ["M","T"], ["none: 0","light: 1","heavy: 2"] ],
}
```

después, para cada nodo debe crearse una tabla que indique las relaciones, para cada nodo, los estados se representan en columnas, por ejemplo:

imaginemos un nodo independiente "R": Rain

VISTA DE TABLA DE NODO INDEPENDIENTE

| none | light | heavy |
|------|-------|-------|
| 0.7  | 0.2   | 0.1   |


CSV equivalente a la tabla:

```csv
0.7
0.2
0.1
```

sin embargo, para tablas con dependencias el modelo cambia un poco, imaginemos al nodo "M" maintenance, que depende del nodo "R" rain:

VISTA DE TABLA DE NODO DEPENDIENTE

|      | none | light | heavy |
|------|------|-------|-------|
| yes  | 0.4  | 0.2   | 0.1   |
| no   | 0.6  | 0.8   | 0.9   |

CSV equivalente a la tabla:

```csv
0.4,0.2,0.1
0.6,0.8,0.9
```

Y así sucesivamente, entonces para un nodo, ligeramente más complejo "T" train, que se compone de las dos tablas anteriores:

VISTA DE TABLA DE NODO DEPENDIENTE

|           | none  | none   | light | light | heavy | heavy |
|-----------|------ |--------|-------|-------|-------|-------|
|           | yes   | no     | yes   | no    | yes   | no    |
| on time   | 0.8   | 0.9    | 0.6   | 0.7   | 0.4   | 0.5   | 
| delayed   | 0.2   | 0.1    | 0.4   | 0.3   | 0.6   | 0.5   |

CSV equivalente a la tabla:

```csv
0.8,0.9,0.6,0.7,0.4,0.5
0.2,0.1,0.4,0.3,0.6,0.5
```

---
Una vez realizados todos los pasos anteriores es posible ejecutar el programa, allí todo estará funcional y el usuario deberá hacer modificaciones en relación a sus deseos (para inferencias o probabilidades conjuntas)
