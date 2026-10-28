Paso 1: Modificación de la carga a Memoria Compartida (Boundary Checks)

1. Razonamiento previo y lógica de coordenadas

En la multiplicación de matrices tradicional con tiling, dividimos las matrices en submatrices (bloques) de tamaño $\text{BS} \times \text{BS}$ (en nuestro caso $16 \times 16$).

Cuando las dimensiones de las matrices no son múltiplos de $16$ (como en nuestro caso de $1503 \times 711$ y $711 \times 1053$), se crean bloques perimetrales en los bordes de la cuadrícula que exceden el tamaño real de los datos.

Para cargar los elementos en la memoria compartida (As y Bs) de forma segura sin acceder a memoria no asignada, debemos calcular las coordenadas globales de cada hilo en cada iteración del tile:

Coordenadas globales en la matriz $A$:

- Fila global: $\text{Row} = \text{blockIdx.y} \times \text{BS} + \text{threadIdx.y}$
- Columna global en el tile $m$: $\text{Col}_A = m \times \text{BS} + \text{threadIdx.x}$
- Condición de validez: Para que un elemento de $A$ exista dentro de sus límites reales, se debe cumplir que $\text{Row} < \text{MATRIX\_A\_ROWS}$ y $\text{Col}_A < \text{MATRIX\_A\_COLS}$.

Coordenadas globales en la matriz $B$:
- Fila global en el tile $m$: $\text{Row}_B = m \times \text{BS} + \text{threadIdx.y}$
- Columna global: $\text{Col}_B = \text{blockIdx.x} \times \text{BS} + \text{threadIdx.x}$
- Condición de validez: Para que un elemento de $B$ exista, se debe cumplir que $\text{Row}_B < \text{MATRIX\_B\_ROWS}$ (donde $\text{MATRIX\_B\_ROWS} = \text{MATRIX\_A\_COLS}$) y $\text{Col}_B < \text{MATRIX\_B\_COLS}$.

Manejo de ceros (Padding):

Si cualquiera de las condiciones no se cumple, el hilo no debe leer de la memoria global de la GPU. En su lugar, escribe 0.0f en su posición correspondiente dentro de As[ty][tx] o Bs[ty][tx].Matemáticamente, multiplicar por 0.0f y sumarlo al acumulador no altera el resultado final de los elementos válidos.

