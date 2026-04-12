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

* Construir la aplicación
* Desplegar automáticamente en cada push

---

## ❌ Problemas Encontrados

### 1. Error de ruta

```
App Directory Location is invalid
```

**Causa:** Ruta incorrecta en el workflow
**Solución:** Ajustar `app_location` a la ruta real del proyecto Angular

---

### 2. Error de permisos (403)

```
Permission denied
```

**Causa:** Repositorio original sin permisos
**Solución:** Realizar fork del repositorio

---

### 3. Error de token inválido

```
No matching Static Web App was found
```

**Causa:** Token desincronizado
**Solución:** Eliminar y recrear la Static Web App

---

### 4. Error de build

```
Could not detect any platform
```

**Causa:** Azure no encontraba Angular
**Solución:** Ajustar correctamente `app_location`

---

## 🔒 Implementación de Seguridad

Se creó el archivo:

```
staticwebapp.config.json
```

Con los siguientes encabezados:

* Content-Security-Policy
* Strict-Transport-Security
* X-Content-Type-Options
* X-Frame-Options
* Referrer-Policy
* Permissions-Policy

Se eliminó el uso de `unsafe-inline` para mejorar la seguridad.

---

## 📊 Resultado Final

* Aplicación desplegada correctamente ✅
* Acceso público funcional ✅
* Seguridad evaluada con A+ en securityheaders.com ✅

---

## 🎯 Conclusión

El despliegue requirió comprender:

* Estructura de proyectos Angular
* Configuración de rutas en Azure
* Automatización con GitHub Actions
* Implementación de seguridad web

Se logró un despliegue exitoso, seguro y automatizado.
