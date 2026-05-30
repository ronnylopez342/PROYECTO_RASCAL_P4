# Cambios tecnicos aplicados despues de la auditoria adversarial

## Problema 1: `Project-Name` no coincidia con carpeta raiz

Antes:

- Carpeta del ZIP: `PROYECTO_RASCAL-main`
- `META-INF/RASCAL.MF`: `Project-Name: proyecto_rascal`

Correccion:

- La raiz del ZIP final es `proyecto_rascal/`.
- `META-INF/RASCAL.MF` conserva `Project-Name: proyecto_rascal`.

## Problema 2: TypePal no cargaba

Antes, Rascal fallaba con:

```text
Could not import module analysis::typepal::TypePal
Undeclared variable: modulesTModelFromTree
```

Correccion:

- Se extrajeron desde `rascal-shell-stable.jar` los modulos TypePal necesarios y se incluyeron en:

```text
src/main/rascal/analysis/typepal/
```

Asi, `extend analysis::typepal::TypePal;` en `Checker.rsc` carga sin depender de Maven ni internet.

## Problema 3: `pom.xml` declaraba dependencias externas que Rascal reportaba como inexistentes

Antes:

```xml
<artifactId>rascal</artifactId>
<version>0.33.3</version>
<artifactId>typepal</artifactId>
<version>0.8.6</version>
```

Correccion:

- Se elimino la seccion de dependencias Rascal/TypePal del `pom.xml` portable.
- Se dejo documentado que el ZIP incluye el jar Rascal y los modulos TypePal.

## Problema 4: advertencias de TypePal por parametros no soportados

Antes, `Checker.rsc` enviaba parametros como `logTModel`, `logAttempts`, `logSolverIterations`, `logSolverSteps`, que la version incluida de TypePal ignoraba con warnings.

Correccion:

- `getModulesConfig()` conserva solo parametros soportados:

```rascal
private TypePalConfig getModulesConfig() = tconfig(
    verbose = false,
    isSubType = subtype
);
```

## Problema 5: restos del skeleton Kotlin

Antes habia referencias a `milang`, `MiLang` y TODOs de plantilla.

Correccion:

- Paquete Kotlin: `verilang`.
- `mainClass = "verilang.MainKt"`.
- `group = "verilang"`.
- `packageName = "VeriLang"`.
- `rootProject.name = "verilang-app"`.

## Problema 6: cierre de Rascal desde Kotlin

Antes `LangService.kt` enviaba `quit;`, que la REPL podia interpretar como variable no declarada.

Correccion:

- Ahora envia `:quit`.

## Problema 7: raiz del proyecto desde Kotlin

Antes el servicio solo revisaba el directorio actual y el padre inmediato.

Correccion:

- Ahora sube por los padres hasta encontrar `src/main/rascal/RunnerJson.rsc`.

