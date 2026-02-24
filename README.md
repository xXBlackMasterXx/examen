# Generacion de hoja de calculo para metodos numericos

El script `main.py` crea el archivo `metodos_numericos.xlsx` con tablas y formulas que permiten resolver los cuatro ejercicios solicitados con una tolerancia de `1E-6`. Cada hoja replica los pasos del algoritmo para que Excel realice las iteraciones de forma transparente.

## Requisitos y ejecucion con uv

1. Instala `uv` si no esta disponible: `pip install uv`.
2. Desde la raiz del repositorio ejecuta:
   - `uv run python main.py`
3. Se genera `metodos_numericos.xlsx` en el mismo directorio.

## Diseno y operaciones por metodo

En todas las hojas:

- Celda `B1`: tolerancia (`1E-6`, editable).
- Filas de iteracion desde la 6 con 25 pasos preparados.
- Columna `Error`: compara el paso actual con la tolerancia para detenerse visualmente (`"Cumple"` cuando se alcanza la precision).

### Metodo de biseccion (hoja `Biseccion`)

- Funcion: `f(x) = EXP(x) - COS(x)`.
- Columnas: iteracion, `a`, `b`, `f(a)`, `f(b)`, punto medio `m`, `f(m)`, ancho del intervalo y error.
- Actualizacion de intervalo:  
  `a_{n+1} = IF(ABS(f(m_n))<=tol, a_n, IF(f(a_n)*f(m_n)<0, a_n, m_n))`  
  `b_{n+1} = IF(ABS(f(m_n))<=tol, b_n, IF(f(a_n)*f(m_n)<0, m_n, b_n))`
- Intervalo inicial editable (por defecto `a=-1`, `b=1`).

### Metodo de Newton-Raphson (hoja `Newton-Raphson`)

- Funcion: `f(x) = x*ATAN(x/2) + LN(x^2+4) - 3`.
- Derivada: `f'(x) = ATAN(x/2) + x/(2*(1+(x/2)^2)) + 2*x/(x^2+4)`.
- Columnas: iteracion, `x_n`, `f(x_n)`, `f'(x_n)`, siguiente estimado `x_{n+1}`, error.
- Paso iterativo: `x_{n+1} = IF(f'(x_n)=0, x_n, x_n - f(x_n)/f'(x_n))`.
- Arranque sugerido: `x_0 = 1.5` (celda editable).

### Metodo de la secante (hoja `Secante`)

- Funcion: `f(x) = 0.5 + 0.25*x^2 - x*SIN(x) - 0.5*COS(2*x)`.
- Columnas: iteracion, `x_{n-1}`, `x_n`, valores de `f`, nuevo estimado `x_{n+1}`, error.
- Paso iterativo:  
  `x_{n+1} = IF(f(x_n)=f(x_{n-1}), x_n, x_n - f(x_n)*(x_n - x_{n-1})/(f(x_n)-f(x_{n-1})))`.
- Valores iniciales: `x_0 = 0.5`, `x_1 = 1.0` (ambos editables).

### Metodo de punto fijo (hoja `Punto Fijo`)

- Transformacion: `g(x) = SQRT((10 - x^3)/4)`, derivado de `x^3 + 4x^2 - 10 = 0`.
- Columnas: iteracion, `x_n`, `g(x_n)`, error.
- Paso iterativo: `x_{n+1} = g(x_n)`; error `=ABS(g(x_n) - x_n)`.
- Valor inicial: `x_0 = 1.0` (editable).

## Personalizacion y uso en Excel

- Ajusta la tolerancia en `B1` de cada hoja o los valores iniciales en la primera fila de iteraciones.
- Las formulas estan ya ingresadas hasta 25 pasos; aumenta el numero de filas copiando la ultima fila si necesitas mas iteraciones.
- Una vez que la columna de condicion muestre `"Cumple"`, la fila contiene una aproximacion que respeta la tolerancia solicitada.
