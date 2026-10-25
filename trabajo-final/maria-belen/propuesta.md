# 01 · Propuesta

**Proyecto:** Copiloto laboral para Comunicación *(nombre provisorio)*
**Tipo:** aplicación web con IA (trabajo final del seminario)

---

## 1. Resumen

Una aplicación web que ayuda a estudiantes avanzados y graduados de Comunicación a **encontrar y priorizar puestos acordes a su perfil**, **adaptar su CV y su carta de presentación a cada oferta** y **llevar el seguimiento de sus postulaciones**. Usa IA para interpretar ofertas y CV, pero siempre trabaja con la experiencia real de la persona: reformula y ordena, no inventa.

## 2. Problema

Quienes salen de la carrera de Comunicación se encuentran con dificultades concretas al buscar trabajo:

- **Perfiles y títulos difusos.** El campo incluye roles con nombres distintos y límites borrosos (community manager, redacción de contenidos, comunicación institucional o interna, prensa, producción audiovisual, estrategia de contenidos, entre otros). Cuesta saber cuáles encajan con la formación y la experiencia propias.
- **CV genérico.** Un mismo CV se envía a todas las ofertas, sin resaltar lo que cada una pide ni usar su vocabulario.
- **Brecha de herramientas.** Las ofertas piden herramientas y habilidades específicas (analítica, edición, gestión de redes, automatización) y no siempre es claro cuáles faltan ni cuáles conviene aprender primero.
- **Seguimiento desordenado.** Las postulaciones se pierden entre mails, portales y planillas, y se olvida hacer seguimiento.

## 3. Destinatarios

Personas usuarias: **alumnos y graduados de la carrera de Comunicación** que quieren insertarse en el mercado laboral o cambiar de puesto.

| Perfil | Situación | Qué necesita de la herramienta |
|---|---|---|
| Estudiante avanzado | Busca primera experiencia, pasantía o trabajo junior | Traducir materias, proyectos y trabajos prácticos a lenguaje de oferta; saber qué puestos probar |
| Graduado reciente | Poca experiencia formal, perfil amplio | Priorizar entre muchos tipos de puesto; detectar qué herramientas aprender |
| Profesional que cambia de puesto | Experiencia previa, quiere moverse de rol o rubro | Reubicar su experiencia hacia otro puesto; saber qué brechas tiene |

## 4. Solución propuesta

Tres módulos integrados en una sola aplicación:

1. **Adaptador de CV y carta.** La persona pega una oferta y carga su CV base. La herramienta indica qué palabras clave de la oferta faltan en el CV, propone una versión reordenada que destaca lo más relevante y redacta una carta de presentación a medida.
2. **Sugeridor de puestos.** La persona sube su CV. La herramienta lo compara con una base de puestos del rubro (archivos `.md`), los ordena según el nivel de coincidencia y explica el puntaje. Además permite elegir otro puesto y consultar qué herramientas o habilidades le faltan.
3. **Tracker de postulaciones.** Registro de cada postulación con estado, fechas y notas, que señala cuáles necesitan seguimiento.

## 5. Qué no es (límites del proyecto)

- **No envía postulaciones** ni completa formularios en portales de empleo.
- **No inventa** experiencia, títulos, herramientas ni logros: solo reformula lo que figura en el CV.
- **No rastrea portales de empleo** de forma automática; la base de puestos se arma y mantiene a mano.
- **No promete resultados**: orienta y ahorra trabajo, pero la decisión y la revisión final son de la persona.

## 6. Valor y diferencial

- **Especializado en Comunicación:** la base de puestos y herramientas refleja el rubro, a diferencia de los adaptadores de CV genéricos.
- **Honestidad por diseño:** la salida se verifica contra el CV original para evitar que la IA agregue información falsa.
- **Brechas accionables:** no solo dice "te falta X", sino qué conviene aprender según la cantidad de puestos que lo piden.
- **Todo en un lugar:** del descubrimiento del puesto al seguimiento de la postulación.

## 7. Dónde interviene la IA y por qué

| Tarea | Por qué IA |
|---|---|
| Extraer requisitos, herramientas y palabras clave de una oferta en texto libre | Las ofertas no tienen formato común |
| Comparar un CV con un puesto y justificar el puntaje | Requiere interpretar experiencia expresada de muchas formas |
| Reformular el CV y redactar la carta con el tono adecuado | Generación de texto condicionada a datos reales |
| Responder "qué me falta para este puesto" | Razonamiento sobre brechas entre perfil y requisitos |

## 8. Alcance y supuestos

- Aplicación web de uso individual, sin cuentas de usuario ni base de datos compartida.
- Base inicial de **30 a 60 puestos** de Comunicación, armada a mano a partir de ofertas públicas y resumida con palabras propias.
- Idioma: español.
- Supuesto a confirmar: se desarrolla en Python con una interfaz web liviana (ver documento de diseño).

## 9. Validación prevista

- Probar la herramienta con un grupo reducido de alumnos o graduados (3 a 5 personas) usando CV reales con su consentimiento, o CV de ejemplo.
- Comparar el ranking de puestos de la herramienta con el criterio de personas del rubro.
- Revisar de forma sistemática que las cartas y CV generados no contengan datos inventados.

## 10. Riesgos principales

| Riesgo | Mitigación |
|---|---|
| La IA inventa experiencia o datos | Reglas estrictas en el prompt y verificación automática contra el CV |
| Base de puestos chica o sesgada | Cubrir variedad de roles y niveles; documentar sus límites |
| Datos personales del CV | Procesamiento local, envío mínimo a la API, aviso de privacidad, sin CV reales en el repositorio |
| Puntajes poco confiables | Mostrar siempre el motivo y las brechas; evaluar contra rankings humanos |
| Alcance excesivo | Priorizar el adaptador de CV y carta; el tracker va último |

## 11. Criterios de éxito (nivel alto)

- La persona obtiene, a partir de una oferta y su CV, un CV adaptado y una carta utilizables sin editarlos desde cero.
- El ranking de puestos resulta razonable para quienes lo prueban.
- Ninguna salida de prueba contiene información que no esté en el CV.
- La persona puede registrar y retomar sus postulaciones sin recurrir a otra herramienta.

El detalle medible de estos criterios está en `02-requisitos.md`.
