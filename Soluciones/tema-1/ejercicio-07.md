# Problema 7 ★★★

**Enunciado.** Mediana de dos arreglos ordenados de largo $n$: el $n$-ésimo menor de los $2n$ elementos. Diseña un algoritmo $O(\log n)$, demuestra su correctitud y da la cota de información.

## Correctitud del algoritmo de la mediana de dos arreglos

Tenemos dos arreglos ordenados $A[0..n-1]$ y $B[0..n-1]$. Queremos el $n$-ésimo menor de los $2n$ elementos (el que quedaría en la posición $n-1$ si los mezcláramos). El algoritmo hace búsqueda binaria sobre un índice $i$ entre $0$ y $n$, define $j = n - i$, y busca que se cumplan estas dos condiciones:

$$
A[i-1] \le B[j] \qquad \text{y} \qquad B[j-1] \le A[i].
$$

Usamos $A[-1] = B[-1] = -\infty$ y $A[n] = B[n] = +\infty$ para los extremos.

Si encuentra un $i$ que cumple las dos, devuelve $\max(A[i-1], B[j-1])$.

Vamos a ver por qué esto funciona.

### ¿Por qué siempre existe una partición válida?

Imagina que ya mezclamos $A$ y $B$ en un solo arreglo ordenado $M$ de $2n$ elementos. Los primeros $n$ elementos de $M$ forman la “mitad izquierda”, y los otros $n$ la “mitad derecha”.

Supón que en esa mitad izquierda hay exactamente $a$ elementos que vienen de $A$. Entonces los otros $n-a$ vienen de $B$. Toma $i = a$ y $j = n - i = n - a$.

- Los primeros $a$ elementos de $A$ están en la mitad izquierda, y los restantes en la derecha.
- Los primeros $j$ elementos de $B$ están en la izquierda, y los restantes en la derecha.

Como todos los de la izquierda son menores o iguales que todos los de la derecha, en particular se cumple:

$$
A[a-1] \le B[j] \quad \text{y} \quad B[j-1] \le A[a].
$$

Así que $i = a$ es una partición válida. **Siempre hay al menos una**.

### ¿Por qué el valor que devuelve es el correcto?

Si $i$ es válido, la mitad izquierda $L$ tiene exactamente $n$ elementos: $i$ de $A$ y $j = n-i$ de $B$. La mitad derecha $R$ tiene los otros $n$.

Las condiciones de validez garantizan que **todo elemento de $L$ es $\le$ que todo elemento de $R$**:

- Dentro de $A$ ya está ordenado: $A[0..i-1] \le A[i..n-1]$.
- Dentro de $B$ también: $B[0..j-1] \le B[j..n-1]$.
- Para comparar $A$ con $B$ en la frontera, las condiciones nos dan justo lo necesario: $A[i-1] \le B[j]$ y $B[j-1] \le A[i]$.

Por lo tanto, $L$ contiene exactamente los $n$ elementos más pequeños del total. El mayor de $L$ es, entonces, el $n$-ésimo menor. Y el mayor de $L$ es simplemente $\max(A[i-1], B[j-1])$ (con $-\infty$ si algún índice es negativo).

### ¿Por qué la búsqueda binaria lo encuentra?

Las condiciones se comportan de forma monótona:

- La condición $A[i-1] \le B[j]$ se cumple para índices pequeños y deja de cumplirse a partir de cierto punto. Es decir, los $i$ que la cumplen forman un prefijo.
- La condición $B[j-1] \le A[i]$ se cumple para índices grandes y deja de cumplirse hacia atrás. Los $i$ que la cumplen forman un sufijo.

La intersección de un prefijo y un sufijo es un intervalo. Como sabemos que al menos un $i$ es válido, ese intervalo no es vacío.

La búsqueda binaria mantiene siempre el intervalo $[lo, hi]$ conteniendo al menos un índice válido. En cada paso prueba $mid = \lfloor (lo+hi)/2 \rfloor$:

- Si falla la primera condición, todos los índices desde $mid$ en adelante fallan también, así que el válido está a la izquierda: $hi = mid - 1$.
- Si falla la segunda, todos los índices hasta $mid$ fallan, así que el válido está a la derecha: $lo = mid + 1$.
- Si no falla ninguna, $mid$ es válido y devolvemos.

Como el intervalo se reduce a la mitad en cada iteración, en a lo sumo $\lceil \log_2(n+1) \rceil$ pasos llegamos a un único índice, que por el invariante tiene que ser válido. Ahí termina y devuelve el valor correcto.

**Cota de información.** El $n$-ésimo menor de $2n$ elementos puede ser cualquiera de los $2n$ elementos. Un árbol de decisión que distinga entre $2n$ posibles respuestas debe tener al menos $2n$ hojas, por lo que su altura es al menos $\lceil \log_2(2n) \rceil = \Omega(\log n)$. El algoritmo logra $O(\log n)$, así que es óptimo en el modelo de comparaciones.

**Conclusión.** El algoritmo es correcto y óptimo en el modelo de comparaciones.
