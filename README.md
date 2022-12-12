# Práctica 4 - Procesos de Negocio

**Autores**: Pedro Antonio Benito Rojano & Tom van Greevenbroek

**Nota**: La restricción de tiempo a 100 unidades se ha implementado como una condición maś en la regla tick.

```
crl [tick] :< O : Process | gtime: T, tokens: TokenSet, Atts >
=> < O : Process | gtime: T plus T', tokens: delta(TokenSet, T'), Atts >
if T' := mte(TokenSet) /\ 0 lt T'
/\ (T plus T') le 100 .
```

## Utiliza el comando search para buscar estados del proceso a lo largo de su ejecución limitada a 100 unidades de tiempo en que no haya ningún token en el conjunto del atributo tokens. Explica el resultado.

**Comando**: `search PROCESS =>* < o : Process | tokens: empty, Atts:AttributeSet > .`

Hay un patrón en las soluciones, esto se debe a los bucles que pueden hacerse. Hemos tomado nota del siguiente patrón:

- En la solución 1 llegamos al nodo _end n05_ con un _gtime:=14_.
- Desde la solución 2 y 3 estamos haciendo bucles sobre la tarea "search products" hasta acabar en el nodo _end n05_.
- En la solución 4 llegamos por primera vez al nodo _end n08_ con un _gtime:=32_.
- Desde la 5 hasta la 10 empezamos a intercalar los nodos _end n05_ y _n08_ iterando sobre la tarea "search products".
- Al llegar a la solución 11, con _gtime:=58_ llegamos al nodo _end n25_.
- Desde la 12 hasta las soluciones restante intercalamos los tres posibles nodos finales iterando sobre la tarea "search prodcut".

## Utiliza el comando search para verificar si hay situaciones de bloqueo para ejecuciones del proceso antes del transcurso de 100 unidades de tiempo. Explica el resultado.

**Comando**: `search PROCESS =>! < o : Process | Atts:AttributeSet > .`

**Resultado**: 47 estados

**Comando**: `search PROCESS =>! < o : Process | Atts:AttributeSet, gtime: 100 > .`

**Resultado**: 21 estados

**Comando**: `search PROCESS =>! < o : Process | tokens: empty, Atts:AttributeSet > .`

**Resultado**: 26 estados

Con esto podemos demostrar que tenemos un total de 47 estados de bloqueo, de ellas 21 se bloquean por agotarles el tiempo, mientras que los 26 restantes llegan a un nodo final.

Además hemos podido comprobar que todos nuestros estados sin tokens son estados de bloqueo, ya que los siguientes comandos son equivalentes.

```
search PROCESS =>! < o : Process | tokens: empty, Atts:AttributeSet > .
search PROCESS =>* < o : Process | tokens: empty, Atts:AttributeSet > .
```

**Resultado**: 26 estados
