# 🚀 Proceso de Despliegue - PokeDex

## 📌 Preparación del Entorno

1. Se realizó fork del repositorio original.

2. Se clonó el proyecto en local:

   ```bash
   git clone <url>
   ```

3. Se accedió a la carpeta del proyecto Angular:

   ```bash
   cd sistemas-distribuidos/poke-dex-lab/source/pokedex-angular
   ```

4. Instalación de dependencias:

   ```bash
   npm install
   ```

5. Generación de build:

   ```bash
   ng build
   ```

---

## ☁️ Configuración en Azure

1. Se creó una Static Web App.
2. Se conectó con GitHub.
3. Se configuraron las rutas:

```yaml
app_location: /sistemas-distribuidos/poke-dex-lab/source/pokedex-angular
output_location: dist/pokedex-angular
```

---

## ⚙️ CI/CD con GitHub Actions

Azure generó automáticamente un workflow que permite:

- Construir la aplicación
- Desplegar automáticamente en cada push

---

## ❌ Errores Encontrados y Soluciones

### 🔴 1. Error de permisos en GitHub (403)

**Error:**

```bash
Permission denied to repository
fatal: unable to access ... 403
```

**Causa:** El repositorio original no permitía hacer `push` porque no era de mi propiedad.

**Solución:** Se realizó un **fork** del repositorio en GitHub para tener control total y poder subir cambios.

---

### 🔴 2. Error: `remote origin already exists`

**Error:**

```bash
error: remote origin already exists
```

**Causa:** El repositorio ya tenía configurado un origen remoto.

**Solución:** Se decidió trabajar directamente sobre el fork (origen ya existente) en lugar de modificar el remoto original.

---

### 🔴 3. Error de ruta en Azure

**Error:**

```text
App Directory Location is invalid
```

**Causa:** La ruta configurada en Azure no coincidía con la estructura real del proyecto Angular.

**Solución:** Se identificó la ruta correcta del proyecto:

```yaml
app_location: /sistemas-distribuidos/poke-dex-lab/source/pokedex-angular
output_location: dist/pokedex-angular
```

y se actualizó el workflow de Azure Static Web Apps.

---

### 🔴 4. Error de token en Azure

**Error:**

```text
No matching Static Web App was found or the api key was invalid
```

**Causa:** El token de autenticación de Azure estaba desincronizado o mal configurado.

**Solución:** Se eliminó la Static Web App y se volvió a crear, generando un nuevo workflow y un token válido.

---

### 🔴 5. Azure no detectaba Angular

**Error:**

```text
Could not detect any platform in the source directory
```

**Causa:** Azure no encontraba el proyecto Angular porque la ruta configurada no apuntaba a la carpeta que contiene `package.json`.

**Solución:** Se corrigió `app_location` para apuntar directamente a la carpeta del proyecto Angular (`sistemas-distribuidos/poke-dex-lab/source/pokedex-angular`).

---

### 🔴 6. Error de build (no existía `dist`)

**Problema:** El proyecto no tenía generada la carpeta `dist` y el workflow fallaba al desplegar.

**Causa:** Angular aún no había sido compilado en local.

**Solución:** Se ejecutaron los comandos:

```bash
npm install
ng build --configuration production
```

para generar los archivos de producción en `dist/pokedex-angular`.

---

### 🔴 7. Baja calificación de seguridad (CSP)

**Problema:** La aplicación obtuvo inicialmente una calificación baja (F/D) en securityheaders.com.

**Causa:** No se habían configurado encabezados HTTP de seguridad.

**Solución:** Se creó el archivo `staticwebapp.config.json` y se añadieron headers como:

- `Content-Security-Policy`
- `Strict-Transport-Security`
- `X-Content-Type-Options`
- `X-Frame-Options`
- `Referrer-Policy`
- `Permissions-Policy`

Posteriormente se fue afinando la CSP hasta obtener una calificación **A+**.

---

### 🔴 8. CSP bloqueando scripts / handlers inline

**Error:**

```text
Executing inline event handler violates the following Content Security Policy directive 'script-src 'self' https:'
```

**Causa:** Se eliminó `unsafe-inline` de `script-src`, pero el proyecto tenía scripts inline:

- Scripts en `index.html` y `404.html` (heredados de GitHub Pages).
- Un handler `onload="this.media='all'"` generado por la optimización de CSS del build de Angular.

**Solución:**

- Se eliminaron los scripts inline de `index.html` y `404.html`.
- Se configuró Angular (`angular.json`) para desactivar `inlineCritical` en producción, evitando que se genere el `onload` inline en el `<link rel="stylesheet">`.

De esta forma se mantuvo una CSP estricta sin `unsafe-inline` y la app siguió funcionando correctamente.

---

### 🔴 9. Imágenes de Pokémon no cargaban en producción

**Error (en producción):**

```text
GET /pokedex-angular/assets/images/... 404 (Not Found)
```

**Causa:** En `environment.prod.ts` la variable `imagesPath` apuntaba a `/pokedex-angular/assets/images`, ruta pensada para GitHub Pages. En Azure Static Web Apps la app cuelga de `/`, por lo que las imágenes reales están en `/assets/images`.

**Solución:** Se actualizó `imagesPath` en `environment.prod.ts` a:

```ts
imagesPath: '/assets/images',
```

con lo que las URLs generadas para los sprites quedaron correctas y las imágenes se cargan tanto en la lista como en el detalle.

---

### 🔴 10. Estilos y dropdowns afectados por CSP

**Problema:** Algunos estilos (por ejemplo, comportamientos de dropdown) no se aplicaban correctamente al endurecer la CSP.

**Causa:** La CSP bloqueaba ciertas técnicas automáticas del build (como CSS crítico inline y handlers generados) hasta que se ajustó la configuración.

**Solución:**

- Se mantuvo una CSP estricta (`script-src 'self' https:;`).
- Se adaptó el proyecto Angular para no depender de scripts/handlers inline.
- Se permitió `style-src 'self' https: 'unsafe-inline';` para ser compatible con estilos inline generados por Angular, sin afectar la calificación **A+** en securityheaders.com.

Con estos ajustes, la aplicación conserva todos los estilos y funcionalidades en producción sin sacrificar seguridad.

---

## 🔒 Implementación de Seguridad

Se creó el archivo:

```
staticwebapp.config.json
```

Con los siguientes encabezados:

- Content-Security-Policy
- Strict-Transport-Security
- X-Content-Type-Options
- X-Frame-Options
- Referrer-Policy
- Permissions-Policy

Se eliminó el uso de `unsafe-inline` para mejorar la seguridad.

---

## 📊 Resultado Final

- Aplicación desplegada correctamente ✅
- Acceso público funcional ✅
- Seguridad evaluada con A+ en securityheaders.com ✅

---

## 🎯 Conclusión

El despliegue requirió comprender:

- Estructura de proyectos Angular
- Configuración de rutas en Azure
- Automatización con GitHub Actions
- Implementación de seguridad web

Se logró un despliegue exitoso, seguro y automatizado.
