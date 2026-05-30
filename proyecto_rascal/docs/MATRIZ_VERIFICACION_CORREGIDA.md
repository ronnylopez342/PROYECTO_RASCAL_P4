# Matriz de verificación corregida

| Requisito | Archivo(s) | Evidencia | Prueba realizada | Resultado observado | Estado | Riesgo restante |
|---|---|---|---|---|---|---|
| Carpeta raíz compatible con Rascal | `META-INF/RASCAL.MF` | `Project-Name: proyecto_rascal`; ZIP raíz `proyecto_rascal/` | Ejecución desde carpeta limpia | Rascal no reporta mismatch de nombre | Cumple | Bajo |
| Archivos Rascal principales | `src/main/rascal/*.rsc` | Están `Syntax`, `AST`, `Parser`, `Implode`, `Generator`, `Checker`, `Main`, `RunnerJson` | Inspección de estructura | Todos presentes | Cumple | Bajo |
| TypePal resoluble | `src/main/rascal/analysis/typepal/` | TypePal vendorizado | `import Main;` | Importa y ejecuta checker | Cumple | Bajo |
| Parser `.vl` | `Parser.rsc`, `Syntax.rsc` | `parseMainModule` usado por `Main` y `RunnerJson` | `main();` | Parser OK | Cumple | Bajo |
| Generator | `Generator.rsc` | Genera espacios, espacios tipados, variables, operadores, reglas, expresiones y cuantificadores | `main();` | Código generado en consola | Cumple | Bajo |
| Tipos primitivos | `Syntax.rsc`, `AST.rsc`, `Checker.rsc` | `int`, `bool`, `real`, `string`, `char` | `instance/spec_types.vl` | Retorna `int: 0` | Cumple | Bajo |
| Tipos definidos por usuario | `Checker.rsc` | `userType(str name)` y rol `spaceId()` | `instance/spec_types.vl`; errores de espacios desconocidos | Válidos pasan; desconocidos fallan | Cumple | Bajo |
| Estructuras tipadas | `Syntax.rsc`, `AST.rsc`, `Checker.rsc`, `Generator.rsc` | `defspace Group : Person end` | `instance/spec_typed_space_good.vl` y `spec_types.vl` | Retornan `int: 0` | Cumple | Bajo |
| Tipos en definiciones anidadas/cuántificadores | `Checker.rsc` | `typedSpaceElementTypes` y `quantifiedVariableType` | `spec_typed_space_quantifier_good.vl` y error correspondiente | Caso bueno pasa; caso malo falla | Cumple | Bajo |
| Regla de existencia de elementos/tipos | `Checker.rsc` | `useDomainIfUserDefined` usa `spaceId()` | `spec_unknown_space_error.vl` | `Undefined space Person`, `int: 1` | Cumple | Bajo |
| `Main.rsc` válido | `Main.rsc` | No genera si hay errores; genera si no hay | `main();` | Retorna `int: 0` | Cumple | Bajo |
| `Main.rsc` inválido | `Main.rsc` | Reporta errores y retorna `1` | `main(|cwd:///instance/errors/spec_unknown_space_error.vl|);` | `Undefined space Person`, `int: 1` | Cumple | Bajo |
| `RunnerJson.rsc` válido | `RunnerJson.rsc` | JSON con campos esperados | `RunnerJson::main(["instance/spec_types.vl"]);` | `success:true`, checks true | Cumple | Bajo |
| `RunnerJson.rsc` inválido | `RunnerJson.rsc` | JSON de error con listas | `RunnerJson::main(["instance/errors/spec_unknown_space_error.vl"]);` | `success:false`, `Undefined space Person` | Cumple | Bajo |
| Integración Kotlin estática | `LangService.kt`, `RunResult.kt`, `MainWindow.kt` | Invoca Rascal, ejecuta RunnerJson, extrae JSON, muestra UI VeriLang | Inspección | Implementado, sin TODOs `milang` | Cumple estático | No se pudo abrir UI en este entorno por falta de internet para Gradle |
| Kotlin ejecución visual | `kotlin-app/` | Gradle wrapper incluido | `./gradlew --version` | Falla descarga por `UnknownHostException` del entorno | No verificable aquí | Requiere internet/Gradle local |
| Sin basura generada | Todo el ZIP | No `.git`, `.vscode`, `target`, `build`, `.gradle`, `bin`, `.lock`, `instance/output` | `find` | Limpio | Cumple | Bajo |
