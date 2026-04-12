# 🧩 PokeDex - Despliegue en la Nube

## 📌 Descripción del Proyecto

Este proyecto consiste en el despliegue de una aplicación web llamada **PokeDex**, la cual permite explorar diferentes especies de Pokémon mostrando información detallada.

La aplicación fue desarrollada previamente (HTML, CSS y JavaScript con Angular) y en esta actividad se realizó su despliegue en la nube utilizando buenas prácticas de DevOps y seguridad.

---

## ☁️ Creación de Cuenta en Azure

Para realizar el despliegue se utilizó **Azure for Students**, siguiendo estos pasos:

1. Ingresar a la página oficial de Azure for Students.
2. Hacer clic en **"Empezar gratis"**.
3. Iniciar sesión con el correo institucional.
4. Verificar la cuenta estudiantil.
5. Activar los beneficios gratuitos (créditos de Azure).

---

## 🚀 Creación del Repositorio

1. Se creó un repositorio en GitHub llamado `pokedex`.
2. Se realizó un fork del repositorio original del proyecto.
3. Se clonó el repositorio en local:

   ```bash
   git clone <url-del-repositorio>
   ```
4. Se instalaron dependencias del proyecto Angular:

   ```bash
   npm install
   ```
5. Se generó la carpeta de producción:

   ```bash
   ng build
   ```

---

## 🔗 Integración con Azure

1. Se creó un recurso de **Static Web Apps** en Azure.

2. Se conectó el repositorio de GitHub.

3. Se configuraron las rutas del proyecto:

   * App location:

     ```
     /sistemas-distribuidos/poke-dex-lab/source/pokedex-angular
     ```
   * Output location:

     ```
     dist/pokedex-angular
     ```

4. Azure generó automáticamente un workflow de CI/CD.

---

## 🌐 Resultado

La aplicación fue desplegada exitosamente y se encuentra accesible mediante una URL pública proporcionada por Azure.

---

## 🔒 Seguridad Web

Se implementaron encabezados HTTP de seguridad mediante un archivo:

```
staticwebapp.config.json
```

Esto permitió mejorar la calificación en seguridad hasta **A+** en el escaneo.

---

## 🧠 Reflexión Técnica

* Los encabezados de seguridad ayudan a prevenir ataques como XSS y clickjacking.
* El despliegue en la nube requiere comprender rutas, build y automatización.
* Uno de los mayores desafíos fue configurar correctamente las rutas del proyecto Angular dentro de Azure.

---

## 📌 Conclusión

El proyecto permitió aplicar conocimientos de despliegue, integración continua y seguridad web, logrando una aplicación funcional y protegida en la nube.
