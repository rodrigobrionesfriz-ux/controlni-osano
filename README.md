# Controles de salud infantil

App web de una sola página: peso, talla y perímetro de la cabeza con curvas OMS 2006 (niñas 0 a 5 años), enfermedades con medicamentos y registro dental. Los datos viven en Firestore (proyecto `controlninosano`) y solo se accede con Google.

El repositorio no contiene datos de la niña. El nombre, la fecha de nacimiento y los controles se guardan solo en Firestore.

## Puesta en marcha

### 1. Firebase (una sola vez)
1. Consola de Firebase, proyecto `controlninosano`, **Firestore Database**: crear la base en modo producción (región sugerida: `southamerica-west1`, Santiago).
2. **Authentication**, pestaña Método de acceso: habilitar **Google**.
3. **Authentication**, Configuración, Dominios autorizados: agregar `TU-USUARIO.github.io`.
4. **Firestore**, pestaña Reglas: pegar el contenido de `firestore.rules`, cambiar los correos por los reales y **Publicar**.
5. Recomendado: en Google Cloud, Credenciales, restringir la clave de API a "Referentes HTTP" con `https://TU-USUARIO.github.io/*` y `https://controlninosano.firebaseapp.com/*`.

### 2. GitHub Pages
1. Crear un repositorio nuevo (por ejemplo `controles-salud`).
2. Subir `index.html`, `firestore.rules`, `README.md` y `.gitignore` (Add file, Upload files).
3. Settings, Pages, Source: **Deploy from a branch**, rama `main`, carpeta `/ (root)`, Save.
4. En un par de minutos queda en `https://TU-USUARIO.github.io/controles-salud/`.

### 3. Primer uso
1. Abrir la URL y entrar con Google.
2. En la primera pantalla, tocar **Importar CSV** y elegir el archivo exportado de la otra app. Se cargan nombre, fecha de nacimiento y todos los controles.
3. Agregar la app a la pantalla de inicio del celular.

## Estructura de datos en Firestore
```
ninos/principal                      perfil (name, birth, sex)
ninos/principal/measurements/{id}    controles de peso, talla y cabeza
ninos/principal/illnesses/{id}       enfermedades con medicamentos
ninos/principal/visits/{id}          visitas al dentista
ninos/principal/extras/teeth         dientes de leche
```

## Notas
- Curvas: patrones de crecimiento infantil de la OMS 2006 (parámetros LMS), niñas de 0 a 5 años. Percentil y z se calculan en el navegador.
- Funciona sin conexión gracias a la caché de Firestore y se sincroniza al volver la red.
- Si el SDK de Firebase no carga, la app pasa a modo local (datos solo en el navegador).
- Es una guía de seguimiento y no reemplaza el control con el pediatra.
