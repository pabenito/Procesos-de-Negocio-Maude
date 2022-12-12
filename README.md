# Práctica 4 - Procesos de Negocio

Nota: La restricción de tiempo a 100 unidades se ha implementado como una condición maś en la regla tick.

'''
crl [tick] :< O : Process | gtime: T, tokens: TokenSet, Atts >
    => < O : Process | gtime: T plus T', tokens: delta(TokenSet, T'), Atts >
	if T' := mte(TokenSet) /\ 0 lt T' 
    /\ (T plus T') le 100 .
'''

## Utiliza el comando search para buscar estados del proceso a lo largo de su ejecución limitada a 100 unidades de tiempo en que no haya ningún token en el conjunto del atributo tokens. Explica el resultado.

**Comando**: `search PROCESS =>* < o : Process | tokens: empty, Atts:AttributeSet > .`

Como acabía esperar los estados sin tokens son estados de bloqueo. 

Solución    Patrón
9-8        2 2 4
3-2        2 6
2-1        8

Clases de equivalencia
1-3 loops 

Primer loop son 8 ticks.
Solición 4 feedback
1-3:         Primeras soluciones son dar vueltas al primer loop (antes del "Make an order").
4-10:         Va hacía el nodo n08, incrementa en gtime 2 de 3
Pattern:     Cada 8 gtime desde el time 14 (es decir, 14, 24, 32...) tenemos bucles en el "searched products", es decir, gtime 14 sin bucle, gtime 24 hace un bucle, gtime 32 hace dos bucles...
        La trayectoria completas se hace en la solución 11, con gtime 58

## Utiliza el comando search para verificar si hay situaciones de bloqueo para ejecuciones del proceso antes del transcurso de 100 unidades de tiempo. Explica el resultado.
