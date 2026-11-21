# Laboratorio de integración continua

<!-- Al terminar, este archivo va al repositorio como docs/diagnostico-ci.md (guía de laboratorio del Tema 3, Parte 3). -->

> **Cómo completar esta plantilla**
>
> Cada texto que está entre los signos **menor que** y **mayor que** se reemplaza por lo que se pide,
> incluidos los dos signos. El resto queda como está.
>
> | Signo | Nombre |
> |:---:|---|
> | `<` | menor que |
> | `>` | mayor que |
>
> Ejemplo, con `mi-usuario` en lugar del usuario de GitHub:
>
> | En la plantilla | Ya completado |
> |---|---|
> | `https://github.com/<usuario>/api-tareas/tree/lab-ci` | `https://github.com/FelipePerezOviedo93/api-tareas-clase` |
>
> En la vista previa, algunos de esos textos no se muestran, porque se leen como código HTML. En el editor
> siempre se ven.

## 1. Enlaces

| Qué | Enlace |
|---|---|
| Repositorio en el tag `lab-ci` | https://github.com/<usuario>/api-tareas/tree/lab-ci |
| Pull request que el check bloqueó (prueba rota a propósito) | <https://github.com/FelipePerezOviedo93/api-tareas-clase/pull/4> |

## 2. Diagnóstico de los tres pipelines rotos

### Caso 1

**Síntoma** Falló en el paso *Pruebas* y en el paso *Publicar Pruebas* 
```text
* Run actions/upload-artifact@v7
Error: No files were found with the provided path: reportes/. No artifacts will be uploaded.
```
```
Run bash scripts/pruebas.sh
/opt/hostedtoolcache/Python/3.12.16/x64/bin/python: No module named pytest
```

**Causa:** Se está escribiendo las pruebas en un archivo que no es el correcto, además pytest no está instalado y no se hace ejecución del archivo con las dependencias del desarrollador

**Corrección:** Se hizo un cambio en el scrit, para que las ruebas sean escritas en el archivo correcto, en reportes
Además se agregó el archivo de requirmiento dev, que contiene pytest 
* [PR](https://github.com/FelipePerezOviedo93/api-tareas-clase/pull/11)

**Prevención:** Escribir una prueba unitaria, que compruebe donde se van a escribir las pruebas y confirmar que sea la direccion correcta, ademas de uno que cmpruebe las dependencias de desarro se agregaron en caso como este donde se hacen pruebas

### Caso 2

**Síntoma** Se cometio un error de sintaxis al momento de escribir la prueba, se escribe en un archivo llamado reporte, y el scrip crea la carpeta y lo hace para reportes

```text
Run actions/upload-artifact@v7
Error: No files were found with the provided path: reporte/. No artifacts will be uploaded.
```

**Causa:** pruebas.sh escribe sobre un archivo diferente al que esta definido en el ci.yml

**Corrección:** corregir el archivo ci.yml, para que se escriba sobre el mismo archivo
* [PR](https://github.com/FelipePerezOviedo93/api-tareas-clase/pull/12)

**Prevención:** Revisar que las pruebas sean escritas en los archivos correctos, aplicar pruebas unitarias

### Caso 3

**Síntoma** Falla en la parte de Pruebas, no se esta encontrando el archivo tareas_ejemplo.json

```text
/opt/hostedtoolcache/Python/3.12.16/x64/lib/python3.12/pathlib.py:1013: FileNotFoundError
- generated xml file: /home/runner/work/api-tareas-clase/api-tareas-clase/reportes/junit.xml -
=========================== short test summary info ============================
ERROR tests/test_datos_ejemplo.py::test_carga_las_tareas_de_ejemplo - FileNotFoundError: [Errno 2] No such file or directory: '/home/runner/work/api-tareas-clase/api-tareas-clase/tests/datos/tareas_ejemplo.json'
========================= 22 passed, 1 error in 0.25s ==========================
Error: Process completed with exit code 1.
```

**Causa:** No se esta encontrando el archivo tareas_ejemplo.json, ya que existe una regla en .gitignore, que todo archivo con la extension .json, no sea incluido dentro del commit

**Corrección:** Se elimina la regla dentro del gitignore, que no permitia agregar archivos .json
* [PR](https://github.com/FelipePerezOviedo93/api-tareas-clase/pull/13)
**Prevención:** Revisar las reglas en gitignore, especiamente si se prohibe algun tipo de extension

## 3. Uso de IA

<No se utilizo IA para este ejercicio>
