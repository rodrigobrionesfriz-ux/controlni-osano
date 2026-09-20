# Controles de salud infantil

App web instalable (PWA) de una sola página: peso, talla y perímetro de la cabeza con curvas OMS 2006 (niñas 0 a 5 años), reporte en tabla, enfermedades con medicamentos y registro dental. Los datos viven en Firestore (proyecto `controlninosano`) y solo se accede con Google.

El repositorio no contiene datos de la niña. El nombre, la fecha de nacimiento y los controles se guardan solo en Firestore.

## Archivos (todos en la raíz del repositorio)
| Archivo | Para qué sirve |
|---|---|
| `index.html` | La app completa |
| `sw.js` | Service worker: instalación y uso sin conexión |
| `manifest.webmanifest` | Nombre, colores e íconos de la app instalada |
| `icon.svg`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png` | Íconos |
| `firestore.rules` | Reglas de seguridad para pegar en Firebase |

## Puesta en marcha

### 1. Firebase (una sola vez)
1. Consola de Firebase, proyecto `controlninosano`, **Firestore Database**: crear la base en modo producción.
2. **Authentication**, Método de acceso: habilitar **Google**.
3. **Authentication**, Configuración, Dominios autorizados: agregar `TU-USUARIO.github.io`.
4. **Firestore**, pestaña Reglas: pegar `firestore.rules` con los correos autorizados y **Publicar**.
5. Recomendado: en Google Cloud, Credenciales, restringir la clave de API a "Referentes HTTP" con `https://TU-USUARIO.github.io/*` y `https://controlninosano.firebaseapp.com/*`.

### 2. GitHub Pages
1. Subir todos los archivos de la tabla a la raíz del repositorio (Add file, Upload files). Si ya estaba publicado, reemplaza `index.html` y agrega los demás.
2. Settings, Pages, Source: **Deploy from a branch**, rama `main`, carpeta `/ (root)`.
3. Abrir `https://TU-USUARIO.github.io/NOMBRE-REPO/`.

### 3. Instalar en el celular
- Android (Chrome): botón **Instalar app** en la pestaña Controles, o menú del navegador, "Instalar app".
- iPhone (Safari): Compartir, "Añadir a pantalla de inicio".

## Uso
- **Curvas**: modo Líneas (percentiles 3/97, 5/95, 10/90, 25/75 y 50), Bandas (P3, P15, P50, P85, P97) o Desviaciones estándar. Rangos 0-2, 0-3, 0-5 y 2-5 años.
- **Controles, vista Tabla**: resumen con percentil por dato y tablas por indicador (percentil, z, estado y cambio). Exporta CSV y genera un reporte imprimible con tabla y gráficos ("Imprimir o guardar PDF").
- **Actualizaciones**: al publicar una versión nueva, la app la toma al abrirla con conexión. Si no cambia, cierra y vuelve a abrir la app.

## Estructura de datos en Firestore
```
ninos/principal                      perfil (name, birth, sex)
ninos/principal/measurements/{id}    controles de peso, talla y cabeza
ninos/principal/illnesses/{id}       enfermedades con medicamentos
ninos/principal/visits/{id}          visitas al dentista
ninos/principal/teeth/{codigo}          un documento por diente de leche
```

## Notas
- Curvas: patrones de crecimiento infantil de la OMS 2006 (parámetros LMS), niñas de 0 a 5 años. Percentil y z se calculan en el navegador.
- Sin conexión se puede consultar y registrar: Firestore guarda en caché y sincroniza al volver la red. El inicio de sesión sí requiere internet la primera vez.
- Si el SDK de Firebase no carga, la app pasa a modo local (datos solo en el navegador).
- Es una guía de seguimiento y no reemplaza el control con el pediatra.
