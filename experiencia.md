# Experiencia en el proyecto de concurrencia

## Tabla de contenidos

- [Introducción](#introducción)
- [Desarrollo](#desarrollo)
  - [Herramientas y técnicas utilizadas](#herramientas-y-técnicas-utilizadas)
  - [Estructura lógica del proyecto](#estructura-lógica-del-proyecto)
  - [Retos y aprendizajes obtenidos](#retos-y-aprendizajes-obtenidos)
- [Conclusión](#conclusión)

## Introducción

La programación concurrente es uno de los pilares fundamentales de los sistemas modernos. Desde servidores web que manejan miles de solicitudes simultáneamente hasta simulaciones complejas, la habilidad de coordinar múltiples procesos es esencial. Inspirado por este desafío, me embarqué en el desarrollo de un simulador de un restaurante utilizando el lenguaje de Java con una arquitectura respaldada por la biblioteca de FXGL.

Este proyecto me permitió explorar la interacción entre múltiples componentes concurrentes, como la entrada y salida de clientes, preparación de órdenes, entrega de alimentos y la gestión de recursos limitados como mesas.

## Desarrollo

### Herramientas y técnicas utilizadas

A continuación se explicará las siguientes herramientas y técnicas utilizadas para el desarrollo del proyecto:

- __Java:__ Es el lenguaje principal utilizado para el desarrollo del simulador, que proporciona un entorno robusto para manejar tanto la lógica de concurrencia de los procesos como la interfaz gráfica.

- __FXGL:__ Esta es la biblioteca utilizada para el desarrollo de la interfaz gráfica que ayudó a facilitar la creación de gráficos interactivos para representar cada elemento del simulador así como sus animaciones, simplificando la representación visual de los procesos concurrentes.

- __Monitores:__ Son estructuras lógicas que permitieron manejar la sincronización de recursos compartidos como los buffers para las ordenes y alimentos, para evitar posibles condiciones de carrera al momento de acceder a estos recursos. Aquí se muestra un ejemplo de un monitor implementado:

```
public class BufferOrdenes {
    private final Queue<Orden> ordenes = new LinkedList<>();

    public synchronized void agregarOrden(Orden orden) {
        ordenes.add(orden);
        notifyAll();
    }

    public synchronized Orden tomarOrden() {
        while (ordenes.isEmpty()) {
            try {
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        return ordenes.poll();
    }
}
```

### Estructura lógica del proyecto

Para facilitar la implementación y el mantenimiento de la aplicación, se optó por una arquitectura modular. Los componentes principales del proyecto para manejar la lógica de la concurrencia se dividieron en:

- __Modelos (hilos):__ Aquí es donde se crearon las entidades principales del proyecto, así como la creación de los hilos para representar a los actores principales del simulador con sus respectivos atributos clave.

- __Monitores:__ Esta es la capa donde se implementaron los patrones de sincronización, utilizando métodos sincronizados para evitar condiciones de carrera y gestionar la concurrencia.

### Retos y aprendizajes obtenidos

Uno de los desafíos principales que he experimentado en la implementación de concurrencia en Java fue la sincronización entre los hilos. Implementar el patrón productor-consumidor fue uno de los retos más complejos para poder manejar el control de los buffers (órdenes y alimentos). Fue necesario gestionar colas de órdenes y asegurar que los cocineros no accedieran a una orden inexistente o que las órdenes no se generaran más rápido de lo que se podían procesar.

``` 
Orden orden = restaurante.getBufferOrdenes().tomarOrden();
System.out.println("Cocinero " + id + " recibió la orden " + orden.getId() + ".");

CountDownLatch latch1 = new CountDownLatch(1);
SpriteCocinero.multiPosicion(id, 0, 200, coordenadas[0], coordenadas[1], cocinaView, 2, latch1);
latch1.await();

System.out.println("Cocinero " + id + " está preparando la orden " + orden.getId() + ".");
Thread.sleep((long) (Math.random() * 2000));
    
    
orden.setEstado(Orden.EstadoOrden.LISTO);
restaurante.getBufferComidas().agregarComida(orden);
    
System.out.println("Cocinero " + id + " entregó la orden " + orden.getId() + ".");
```

Otro desafío complejo de resolver fue la instalación e implementación de la biblioteca de FXGL para el proyecto, debido al poco conocimiento sobre el uso y aplicación de esta biblioteca como dependencia, lo cual llevó una serie de pasos algo compleja para poder resolver el problema de la instalación de la biblioteca.

```
<dependencies>
    <dependency>
        <groupId>com.github.almasb</groupId>
        <artifactId>fxgl</artifactId>
        <version>17.1</version>
    </dependency>
</dependencies>
```

Además, la representación gráfica de cada elemento exigió mucha precisión para coordinar las animaciones de cada elemento como los meseros y comensales mientras interactuaban con las mesas y las órdenes.

Otro desafío significativo fue mantener el sistema escalable. A medida que aumentaba el número de comensales y cocineros, era necesario garantizar que el rendimiento se mantuviera constante.

## Conclusión

El desarrollo de este proyecto fue una experiencia enriquecedora tanto a nivel técnico como personal. Más allá de mejorar mis habilidades en programación concurrente, aprendí la importancia de planificar una arquitectura sólida antes de comenzar a codificar. La modularización fue clave para gestionar la complejidad del proyecto, permitiéndome trabajar de manera organizada y eficiente.

Los desafíos que enfrenté, como la sincronización entre hilos, el manejo de los monitores y adicionalmente algo de la representación gráfica de procesos concurrentes, me ayudaron a comprender mejor cómo se implementan y optimizan sistemas similares en el mundo real. Además, trabajar con FXGL me abrió las puertas a nuevas posibilidades para futuros proyectos interactivos.

223240 Hernández Pérez Adrián Mauricio