# ISIS-2111 PLE - Proyecto 4 - VeriLang en Kotlin

Esta carpeta es la entrega completa corregida del Proyecto 4.

## Estructura principal

- `META-INF/RASCAL.MF`: manifiesto Rascal. El `Project-Name` es `proyecto_rascal`, por eso esta carpeta debe llamarse exactamente `proyecto_rascal` al ejecutar Rascal.
- `src/main/rascal/`: implementación de VeriLang.
  - `Syntax.rsc`: gramática concreta.
  - `AST.rsc`: sintaxis abstracta.
  - `Parser.rsc`: parser de archivos `.vl`.
  - `Implode.rsc`: conversión de parse tree a AST.
  - `Generator.rsc`: generación/pretty-printer de VeriLang.
  - `Checker.rsc`: chequeo semántico y de tipos con TypePal.
  - `Main.rsc`: flujo principal Rascal.
  - `RunnerJson.rsc`: puente JSON para la app Kotlin.
  - `analysis/typepal/`: fuentes de TypePal vendorizadas desde `rascal-shell-stable.jar` para que el ZIP sea autocontenido.
- `instance/`: casos válidos e inválidos.
- `kotlin-app/`: aplicación Kotlin/Compose que ejecuta Rascal y muestra los resultados.
- `rascal-shell-stable.jar`: runtime Rascal usado por Kotlin.
- `Proyecto4_Verilang_Kotlin_Final_correcciones_ampliadas_v2.pdf`: documento final.
- `docs/`: matriz, cambios aplicados, estructura final y logs de pruebas.

## Cómo ejecutar pruebas Rascal

Desde esta carpeta (`proyecto_rascal`):

```bash
java -Dfile.encoding=UTF-8 -jar rascal-shell-stable.jar
```

Luego en la consola Rascal:

```rascal
import Main;
main();
main(|cwd:///instance/errors/spec_unknown_space_error.vl|);

import RunnerJson;
RunnerJson::main(["instance/spec_types.vl"]);
RunnerJson::main(["instance/errors/spec_unknown_space_error.vl"]);
```

Resultados esperados:

- `main();` retorna `int: 0`.
- `main(|cwd:///instance/errors/spec_unknown_space_error.vl|);` reporta `Undefined space Person` y retorna `int: 1`.
- `RunnerJson::main(["instance/spec_types.vl"]);` produce JSON con `success:true`, `parseOk:true`, `typeCheckOk:true`, `semanticOk:true`.
- `RunnerJson::main(["instance/errors/spec_unknown_space_error.vl"]);` produce JSON con `success:false`, `parseOk:true`, `typeCheckOk:false`, `semanticOk:false` y `Undefined space Person`.

Los logs reales están en:

- `docs/rascal_pruebas_obligatorias.log`
- `docs/rascal_todas_las_instancias.log`

## Cómo ejecutar Kotlin

Desde `kotlin-app`:

```bash
./gradlew run
```

o en Windows:

```bat
gradlew.bat run
```

La app abre una ventana titulada `VeriLang`. Desde ahí se seleccionan archivos `.vl`. La UI muestra Parse, Types, Semántica, errores y código generado.

## Nota sobre GitHub

No suba solamente el ZIP dentro del repositorio. El evaluador necesita la estructura real de archivos. Si el repositorio no se llama `proyecto_rascal`, mantenga esta carpeta `proyecto_rascal/` como carpeta raíz del proyecto Rascal y ejecute desde dentro de ella.

El archivo `rascal-shell-stable.jar` pesa alrededor de 79 MB. GitHub web suele fallar con archivos grandes; para subir completo use GitHub Desktop o Git Bash/terminal.
