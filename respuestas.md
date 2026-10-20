# Respuestas – Laboratorio de pruebas automatizadas en Express.js

**Taller de Lenguajes de Programación IV · Unidad 4**

## Sección 2 – Conocer la aplicación

- **¿Qué archivo contiene la regla de cálculo?** `src/services/precios.ts`.
- **¿Qué archivo define la ruta HTTP?** `src/app.ts`.
- **¿En qué se diferencian?** `precios.ts` contiene la lógica de negocio (valida y calcula, sin saber nada de HTTP). `app.ts` recibe la petición, llama al servicio y devuelve la respuesta con su código (200, 400 o 500).

## Sección 4 – Interpretar la falla

1. **¿Qué total devuelve un 150 % sobre $1.000?** −500.
2. **¿Por qué no cumple la regla de negocio?** Un precio final no puede ser negativo; el descuento debe estar entre 0 y 100 %.
3. **¿Por qué la prueba falla si la función terminó?** Porque la prueba espera que se lance un `RangeError`, y la función devolvió un valor sin lanzar ningún error.

## Sección 6 – Desafío NaN

**¿La validación rechaza `NaN`?** Sí. `Number.isFinite(NaN)` devuelve `false`, por lo que se lanza `RangeError`.

## Sección 7 – Prueba de integración

**¿Por qué es de integración?** Porque envía una petición HTTP real y comprueba varias partes juntas: Express, la lectura del JSON, la ruta, el servicio y la conversión del error en un 400. Una prueba unitaria solo comprueba la función aislada.

## Sección 8 – Tipos y pruebas

**¿Por qué usar `typecheck` y `npm test`?** Porque detectan errores distintos. `typecheck` revisa los tipos sin ejecutar el código; `npm test` ejecuta el código y verifica que se comporte como se espera. Ninguno reemplaza al otro.

## Sección 10 – Registro de resultados

| Comprobación | Resultado | Observación |
| --- | --- | --- |
| Respuesta de `GET /healthy` | ✅ `{"estado":"ok"}` | Ruta cambiada de `/salud` a `/healthy`. |
| Primera ejecución de `npm test` | 4 aprobadas, 1 fallida | Falló la del 150 %: devolvía −500. |
| Corrección del servicio | ✅ 5 aprobadas | Se agregó la validación con `RangeError`. |
| Nuevas pruebas unitarias | ✅ 9 aprobadas | 100 %, precio negativo, descuento negativo y `NaN`. |
| Prueba de integración HTTP 400 | ✅ 10 aprobadas | Devuelve 400 con la propiedad `error`. |
| Verificación `npm run typecheck` | ✅ Sin errores | — |

## Sección 11 – Preguntas de cierre

1. **¿Qué diferencia hay entre comprobar manualmente una respuesta y ejecutar una prueba automatizada?** La comprobación manual depende de una persona y hay que repetirla a mano. La automatizada deja escrito el resultado esperado y se puede repetir siempre igual, en segundos.
2. **¿Cuál fue la causa de la primera prueba fallida?** La función no validaba las entradas y aceptaba un descuento del 150 %.
3. **¿Por qué es útil incluir casos inválidos?** Porque los errores suelen estar en los límites y en los datos inesperados. Los casos habituales pasaban aunque la función tuviera el defecto.
4. **¿Qué parte comprueba cada archivo de pruebas?** `tests/precios.test.ts` comprueba solo el servicio (pruebas unitarias). `tests/api.test.ts` comprueba la app Express junto con el servicio (pruebas de integración).
5. **¿Qué ventajas tiene ejecutar las pruebas después de cada modificación?** Los errores se detectan enseguida, se evitan regresiones (romper algo que antes funcionaba) y se puede modificar el código con más confianza.
