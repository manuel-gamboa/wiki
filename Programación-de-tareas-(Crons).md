El primer paso para tener una tarea "programada" es contar con una clase usualmente un `Singleton` la cual deberá de administrar toda la lógica de la tarea a programar. Posteriormente dicha clase deberá tener un método el cual será llamado cada vez que se quiera ejecutar la tarea, el nombre de la función, por convención, deberá de terminar en la palabra `Cron`.

### Ejemplo

1) Creamos una clase llamada `TStatsUploader` que administra toda la lógica para subir estadísticas, etc.

2) Definimos el método de la tarea programada sin parámetros y que "por convención" debe terminar con la palabra Cron. Por ejemplo:

```pascal
procedure TStatsUploader.SubirEstadisticasCron;
begin
   // NOTA: La siguiente función debe ejecutarse en un hilo 
   SubimosEstadisticasAServidorAsync;
end;
````

3) Programamos, cuando sea pertinente (método `Configurar`, `AfterConstruction`, etc.) el envio usando el controlador `ICronTab` del cual la clase `TStatsUploader` deberá tener una instancia, por ejemplo:

```pascal
procedure TStatsUploader.Configurar(const aParametro: String);
begin
     Assert(fCronTab <> nil);
     // Programamos que se suba cada 5 minutos los stats
     fCronTab.ProgramarTareaCada('NombreUnicoDelCron',
                                 SubirEstadisticasCron, // Este metodo se llamara cada 5 minutos
                                 5);  // Parametro de minutos
end;
```

### Consideraciones importantes
El método de cron deberá de encargarse de ejecutar su código en un hilo, de lo contrario el hilo principal y el programa puede bloquearse a nivel gráfico en lo que la tarea se termina de ejecutar.