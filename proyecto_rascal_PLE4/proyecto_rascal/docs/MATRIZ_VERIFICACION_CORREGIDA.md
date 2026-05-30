# Matriz de verificacion corregida - Proyecto 4 VeriLang Kotlin

| Requisito | Fuente | Archivo(s) | Evidencia | Prueba | Resultado observado | Estado | Riesgo restante |
|---|---|---|---|---|---|---|---|
| Carpeta raiz compatible con RASCAL.MF | Proyecto 4 / requisito de empaquetado | `META-INF/RASCAL.MF` | `Project-Name: proyecto_rascal`; raiz del ZIP `proyecto_rascal/` | Abrir Rascal desde raiz | Sin error de nombre de proyecto | Cumple | Bajo |
| Proyecto contiene manifest | Checklist auditoria | `META-INF/RASCAL.MF` | Archivo presente | Inspeccion | Presente | Cumple | Bajo |
| Proyecto contiene `pom.xml` | Checklist auditoria | `pom.xml` | Archivo presente y sin dependencias invalidas Rascal/TypePal | Abrir Rascal | Sin errores de dependencias inexistentes | Cumple | Bajo |
| Proyecto contiene sintaxis concreta | Proyecto 3 heredado | `src/main/rascal/Syntax.rsc` | Define `MainModule`, `Space`, `Domain`, `Operator`, `Variables`, `Rule`, `Expression`, cuantificadores | Inspeccion + parse | Parse correcto de `spec_types.vl` | Cumple | Bajo |
| Soporte `int`, `bool`, `real`, `string`, `char` | Proyecto 3 heredado | `Syntax.rsc`, `AST.rsc`, `Checker.rsc` | `Domain` incluye dominios primitivos; `Checker` mapea a `intType`, `boolType`, etc. | `main()` sobre `spec_types.vl` | Sin errores | Cumple | Bajo |
| Soporte de tipos definidos por usuario | Proyecto 3 heredado / retroalimentacion | `Syntax.rsc`, `Checker.rsc` | `nameDomain`, `userType(str name)`, `spaceId()` | `main()` sobre `spec_types.vl` | `Person` y `Group` validos | Cumple | Bajo |
| Estructuras tipadas | Retroalimentacion | `Syntax.rsc`, `Checker.rsc` | `defspace Group : Person end`, `typedSpaceElementTypes` | `main()` | Caso valido aceptado | Cumple | Bajo |
| Subespacios tipados | Revision solicitada | `Syntax.rsc`, `AST.rsc`, `Checker.rsc`, `Generator.rsc` | Regla `defspace sub < super : Domain end` | Inspeccion estatica | Soportado por gramatica/checker/generator | Cumple por inspeccion | Medio, falta caso dedicado en instancia |
| Parser desde archivo `.vl` | Proyecto 3 heredado | `Parser.rsc`, `Main.rsc`, `RunnerJson.rsc` | `parseMainModule(file)` | `main()` y `RunnerJson::main` | Parse OK | Cumple | Bajo |
| Generator | Proyecto 3 heredado | `Generator.rsc` | Genera espacios, variables, operadores, reglas, expresiones, cuantificadores | `main()` y `RunnerJson::main` | Codigo generado en stdout/JSON | Cumple | Bajo |
| TypePal instalado | Proyecto 3 heredado | `Checker.rsc`, `src/main/rascal/analysis/typepal/*` | Import `analysis::typepal::TypePal`; TypePal vendorizado | `import Main; main();` | Carga y chequea | Cumple | Bajo |
| Correspondencia de tipos | Proyecto 3 heredado | `Checker.rsc` | `requireEqual`, `functionType`, `domainToType` | `main()` caso valido + invalidos disponibles | Caso valido sin errores; error desconocido detectado | Cumple | Bajo |
| Tipos dentro de definiciones anidadas | Retroalimentacion | `Checker.rsc` | `typedSpaceElementTypes`, `quantifiedVariableType` | `main()` con `forall member in Group` | `member` se trata como `Person` | Cumple | Bajo |
| Regla existencia de elementos/tipos | Retroalimentacion | `Checker.rsc` | `useDomainIfUserDefined`, `c.use(..., {spaceId()})` | `main(|cwd:///instance/errors/spec_unknown_space_error.vl|)` | Error `Undefined space Person` | Cumple | Bajo |
| No generar codigo en caso invalido | Proyecto 3 heredado | `Main.rsc`, `RunnerJson.rsc` | Si hay errores, retorna antes de generator | Prueba caso invalido | No hay salida generada | Cumple | Bajo |
| `Main.rsc` retorna 0 en valido | Prueba obligatoria | `Main.rsc` | Retorno `0` al final valido | `main()` | `int: 0` | Cumple | Bajo |
| `Main.rsc` retorna 1 en invalido | Prueba obligatoria | `Main.rsc` | Retorno `1` si hay errores | `main(|cwd:///instance/errors/spec_unknown_space_error.vl|)` | `int: 1` | Cumple | Bajo |
| `RunnerJson.rsc` JSON valido valido | Proyecto 4 / Kotlin | `RunnerJson.rsc` | Campos `success`, `parseOk`, `typeCheckOk`, `semanticOk`, `typeErrors`, `semanticErrors`, `output`, `error`, `codigoFormateado`, `resumen` | `RunnerJson::main(["instance/spec_types.vl"])` | JSON success true | Cumple | Bajo |
| `RunnerJson.rsc` JSON valido invalido | Proyecto 4 / Kotlin | `RunnerJson.rsc` | Reporta errores de TypePal | `RunnerJson::main(["instance/errors/spec_unknown_space_error.vl"])` | JSON success false con `Undefined space Person` | Cumple | Bajo |
| `LangService.kt` invoca Rascal | Proyecto 4 | `kotlin-app/src/main/kotlin/verilang/service/LangService.kt` | Usa `ProcessBuilder`, jar Rascal, `RunnerJson::main` | Inspeccion | Implementado | Cumple por inspeccion | Medio, requiere Gradle/internet para UI |
| `RunResult.kt` coincide con JSON | Proyecto 4 | `RunResult.kt`, `RunnerJson.rsc` | Campos coinciden | Inspeccion | Coincide | Cumple | Bajo |
| UI VeriLang | Proyecto 4 | `Main.kt`, `MainWindow.kt` | Titulo `VeriLang`, encabezado `VeriLang - Runner`, selector `.vl`, chips Parse/Types/Semantica | Inspeccion | Implementado | Cumple por inspeccion | Medio, UI no ejecutada aqui por Gradle offline |
| Sin basura generada prohibida | Checklist auditoria | ZIP | Sin `.git`, `target`, `build`, `.gradle`, `bin`, `.lock` | `find` | Limpio | Cumple | Bajo |
