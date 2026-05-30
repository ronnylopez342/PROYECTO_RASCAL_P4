# Cambios técnicos aplicados

## Empaquetado

- Se corrigió la carpeta raíz para que sea `proyecto_rascal`, compatible con `META-INF/RASCAL.MF`.
- Se removió basura generada o de entorno: `.vscode`, `target`, `build`, `.gradle`, `bin`, archivos `.lock` e `instance/output`.
- Se mantuvo `rascal-shell-stable.jar` dentro de la entrega para que Kotlin pueda invocar Rascal.

## Dependencias Rascal / TypePal

- Se removieron dependencias Maven obsoletas/inexistentes del `pom.xml` original.
- Se vendorizó TypePal en `src/main/rascal/analysis/typepal/` a partir del `rascal-shell-stable.jar` incluido.
- Con esto `import analysis::typepal::TypePal;` queda resoluble desde un ZIP limpio sin depender de Maven externo.

## Checker

- `Checker.rsc` mantiene soporte para tipos primitivos: `int`, `bool`, `real`, `string`, `char`.
- `Checker.rsc` mantiene soporte para `userType(str name)`.
- Se validan espacios tipados como `defspace Group : Person end`.
- Se valida existencia de tipos definidos por usuario usados como dominios.
- Se valida el tipo de variables cuantificadas dentro de espacios tipados.
- Se validan operadores, reglas, expresiones lógicas y cuantificadores.
- Se eliminaron argumentos de configuración TypePal no soportados por la versión incluida para evitar warnings.

## Kotlin

- Se renombró el paquete Kotlin de `milang` a `verilang`.
- Se limpió el skeleton: sin TODOs ni textos “Mi Lenguaje”.
- `Main.kt` abre una ventana titulada `VeriLang`.
- `MainWindow.kt` permite seleccionar archivos `.vl` y muestra Parse, Types, Semántica, errores y salida.
- `LangService.kt` invoca `rascal-shell-stable.jar`, importa `RunnerJson`, ejecuta `RunnerJson::main`, extrae JSON y decodifica a `RunResult`.
- `RunResult.kt` coincide con los campos emitidos por `RunnerJson.rsc`.
- El wrapper Gradle queda configurado con `gradle-8.7-bin.zip`.
