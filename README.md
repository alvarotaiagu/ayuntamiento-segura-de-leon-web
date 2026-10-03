# Web municipal «Puerta abierta» · Segura de León

Maqueta de la web del **Ayuntamiento de Segura de León** (Badajoz, 1.758 habitantes), hecha con la plantilla `plantilla-ayuntamiento-puerta-abierta-web` y con sus datos reales. Es una **propuesta**:
- en todas las páginas lleva la banda «Propuesta de diseño… no es la web oficial» y `noindex, nofollow`;
- está publicada en <https://alvarotaiagu.github.io/ayuntamiento-segura-de-leon-web/> (con `?revision`, el mando de la reunión).

```bash
npm install                        # Playwright y axe-core (solo para los scripts)
node scripts/aplicar.mjs           # genera la web desde los datos
node scripts/servir.mjs            # http://127.0.0.1:4192/  ·  con ?revision, el mando
node scripts/verificar.mjs         # todas las comprobaciones
```

Los datos, con su fuente y fecha, están en `../ayuntamiento-segura-de-leon-bocetos/`:
- `DATOS.md`: cada dato con su fuente;
- `INVENTARIO.md`: las 475 páginas y documentos de seguradeleon.es, con su destino;
- `ERRORES.md`: lo que falla en la web actual.

`municipio.json` se genera con `../ayuntamiento-segura-de-leon-bocetos/_scripts/construir_municipio.py`.

---

## El concepto

Es el de la plantilla: **la puerta del Ayuntamiento, abierta todo el día**. Un arco de medio punto encalado enmarca el castillo y da paso al panel «Hoy en Segura», que dice si el Ayuntamiento está abierto, qué viene en la agenda y cuál es el último aviso.

Por qué le va bien a Segura:
- **Su web está parada y su Bandomóvil, vivo.** La última noticia de seguradeleon.es es de 2022 y la agenda está vacía desde 2013. Mientras tanto, el Ayuntamiento publica cada día en Bandomóvil (203 comunicados desde junio). La propuesta pone esos avisos a la vista y enlaza el canal para recibirlos en el móvil.
- **Lo importante de su web está en imágenes**: los teléfonos, los horarios, las fiestas y la corporación. Aquí todo es texto, se lee con lector de pantalla y se encuentra con el buscador.
- **Es un pueblo de visita**: el castillo, el Museo de la Capea y las Capeas. Por eso «El pueblo» tiene «Para visitar», con horario, entrada y teléfono, y «Dónde comer y dormir».

## Qué se añadió a la plantilla para este pueblo

Su web tenía contenido útil que la plantilla no sabía enseñar. Se añadió **a la plantilla**, como sección opcional y genérica, nunca como parche:

| Sección | Campo | Origen |
|---|---|---|
| Para visitar | `pueblo.visitas` | La añadió la sesión de otro municipio el mismo día; aquí se usa para el castillo, el museo, la exposición Aradillas y la Oficina de Turismo |
| Normativa y documentos | `documentos` | Ídem; aquí se usa para 44 ordenanzas, las actas y las guías turísticas |
| Instalaciones municipales | `instalaciones` | La añadió la sesión de Usagre el mismo día; aquí se usa para el pabellón, la sala de cardio, la piscina, los parques, el Punto Limpio y el área de autocaravanas |
| Dónde comer y dormir | `pueblo.establecimientos` | Añadida para Segura (commit `c232384` de la plantilla) |
| Reciba los avisos en el móvil | `canal_avisos` | Añadida para Segura (ídem): Bandomóvil y Telegram |
| Tablón sin expropiaciones | patrón en `scripts/lib/tablon.mjs` | Añadido para Segura (commit `e5541f0`): la relación de afectados del Monte de los Silos nombraba a propietarios |

Además, la prueba de reskin de `verificar.mjs` busca ahora un municipio de `pruebas/` distinto del propio. Aquí se prueba con **Ribera del Fresno** (`pruebas/ribera-del-fresno/`), porque el de la plantilla era justo Segura.

## Decisiones que conviene saber

- **El color de marca es el púrpura del león, elegido a mano** (`marca/marca.json`, `principal_motivo`).
  - Por área ganaría el **gules** del cuartel (31 %), pero el rojo es el de las alertas: un aviso urgente no puede parecer un botón.
  - La regla pasaría entonces al **sinople** de la punta, que solo ocupa un 5 % del escudo y pinta una bellota, no al pueblo.
  - El **león púrpura** es la pieza que explica el nombre («de León»: la villa fue capital de la Encomienda Mayor de León de la Orden de Santiago), y su web actual ya es malva.
  - `aplicar.mjs` lo oscurece hasta AA sin cambiarle el matiz; la tabla está en `marca/_contraste.txt`.
  - El mando de la reunión ofrece otras dos paletas, giradas ±100°.
- **Horario del Ayuntamiento: de ejemplo.** No está publicado en ningún sitio. Sale «Lunes a viernes, de 8:00 a 14:00», con la etiqueta «Ejemplo»: es el que dan dos comunicados de Bandomóvil para inscribirse en las oficinas.
- **Corporación**: la del BOP de 2023, contrastada con el **acta del Pleno de 28/09/2026**. La Diputación y el Ministerio aún ponen a Antonio Manuel Lozano Blanco (PP); el acta ya pone a M.ª Ángeles Miranda Santana. Las áreas de la alcaldesa salen de la imagen de su web, porque el BOP solo publica las delegaciones de los demás.
- **Tablón**: una sola lectura del 03/10/2026, guardada en los bocetos. De sus 5 anuncios salen 2. Quedan fuera:
  - el acta del Pleno, que no está disociada (la noticia del Pleno la resume);
  - la citación y la relación de afectados de la expropiación del Monte de los Silos, que nombran a propietarios.
  
  La lectura automática solo se activa con la autorización del Ayuntamiento (`tablon_autorizado`): el `robots.txt` de la sede la prohíbe.
- **Trámites: 112, todos de la sede**, comprobados uno a uno el 03/10/2026. Los temas y momentos en lenguaje claro son los de la plantilla (los uuid de Gestiona son comunes), más «Trabajo y empleo» y «Busco trabajo en el Ayuntamiento».
- **Documentos de su web**: las ordenanzas enlazan su publicación más reciente en el BOP. Las guías turísticas, las rutas en PDF y las audioguías se enlazan **en su servidor actual**: al cambiar de web hay que migrarlas (ver pendientes).
- **Teléfonos**: los 25 de la imagen de su web, contrastados uno a uno con la fuente oficial de cada servicio.
  - El instituto da 924 290 700 en su web oficial y 924 280 700 en la del Ayuntamiento: se usa el del centro.
  - No se publican el 085 de bomberos (en Extremadura las emergencias van al 112) ni el móvil personal del monitor del gimnasio.

### Erratas de su web, corregidas al usar sus textos
- «Monumento del **Sacrado** Corazón» → Sagrado.
- «Ntra. **Sr.a** de las Angustias» → Sra.
- «Beatriz **Nuñez**» → Núñez.
- «Ildefonso **serrano**» → Serrano.
- «tasa por **restación** del servicio» → prestación; «**incluídos**» → incluidos; «**resíduos**» → residuos (ordenanzas).
- Teléfono cortado «924 70 30 **6**» → 924 703 061 (Diputación). En la web nueva sale el principal, 924 703 011.
- Fuera de su web: en Bandomóvil, el cartel de la Noche en Blanco dice «NOCHE EN **BLANO**», y el comunicado del pregón pone «jueves 11 de septiembre» cuando el jueves era el 10.

### Datos que se contradicen
- **Órgano de la parroquia**: 1782 o 1783 según la página. Se usa 1782 (`parroquia` e `interior`).
- **Convento de la Limpia Concepción**: 1571 o 1573. Se usa «hacia 1571-1573».
- **Castillo**: Turismo de Extremadura tiene los horarios de verano e invierno invertidos y otra tarifa. Se usa el comunicado del Ayuntamiento del 01/10/2026 y la tarifa de su web. Los precios de las visitas guiadas y de los grupos tienen tres versiones distintas: **no se publican**.
- **Consultorio**: 924 280 729 en la web del SES y en la del Ayuntamiento; 924 703 326 en el catálogo del SES de 2017. Se usa el primero.
- **Fecha del Pleno de septiembre**: el tablón dice el 30 y el acta, el 28. Se usa el 28.
- **Punto Limpio**: los viernes abre a las 8:00 según su web y a las 8:30 según Bandomóvil (posterior). Se usa Bandomóvil.

## Créditos de las fotos

| Foto | Autor | Licencia |
|---|---|---|
| El castillo sobre el cerro (portada) | muffinn (Wikimedia Commons) | CC BY 2.0 |
| El castillo sobre el caserío, iglesia de la Asunción, ermita de los Remedios, calles y tejados, Capeas de 2013 y 2014 | Ana Rey (Wikimedia Commons) | CC BY-SA 2.0 |
| Convento de San Benito y Casa Consistorial | web del Ayuntamiento | **Solo para la propuesta**: su aviso legal reserva los derechos |
| Escudo | Mfarinias, diseño de Antonio Alfaro de Prado (Wikimedia Commons) | CC BY-SA 4.0 |

Todas tienen enlace a su ficha en `media/creditos.json` y en «El pueblo». En las de las Capeas salen adultos en un acto público, a tamaño medio.

## Pendientes para el Ayuntamiento

- [ ] **Horario de atención** al público. Ahora sale el de ejemplo.
- [ ] **Fotos propias** (el Museo de la Capea, las calles, las fiestas) y **autorización** para usar las de su web.
- [ ] **Policía Local**: si hay servicio operativo (el Ayuntamiento la cita «en situación de segunda actividad») y si el 649 562 513 sigue valiendo.
- [ ] La **fecha del relevo** de Lozano Blanco por Miranda Santana, y los portavoces de los grupos.
- [ ] El **saluda** de la alcaldesa: el texto actual es de ejemplo.
- [ ] **Precios de las visitas guiadas y de los paquetes** turísticos: hay tres versiones.
- [ ] **Migrar** a la web nueva los 112 audios de las audioguías, las guías y las rutas en PDF, y las actas de 2003 a 2018, que ahora están en su servidor.
- [ ] **Autorización para leer su tablón** de la sede (`robots.txt` lo prohíbe a los robots), y para leer su Bandomóvil si se quiere que los avisos entren solos.
- [ ] La referencia del **DOE del escudo** (2008).
- [ ] Dirección del **área de autocaravanas** y del **Museo de la Capea**, que su web no da (OpenStreetMap los sitúa).
- [ ] Teléfono de la **residencia** y horarios del **cementerio** y del **mercado de abastos**.

## Verificación

`node scripts/verificar.mjs --capturas`: ver el resultado de la última pasada en el informe de entrega. Comprueba axe (WCAG 2.1 AA) en todas las páginas con las dos densidades y las tres paletas, el desborde de 320 a 1440 px y al 200 %, el teclado, la cortina, el «abierto ahora», el tablón, el reskin a Ribera del Fresno sin restos de Segura, las secciones opcionales y que «Ejemplo» salga justo en los datos marcados.
