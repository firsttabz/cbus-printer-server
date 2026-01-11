# Guía de Aplicación a SignPath Foundation

Esta guía te ayudará a aplicar a **SignPath Foundation** para obtener firma de código **gratuita** para tu proyecto Open Source.

## 🎯 ¿Qué es SignPath Foundation?

SignPath Foundation ofrece **firma de código completamente gratuita** para proyectos de código abierto. Ellos:
- ✅ Emiten el certificado en su nombre
- ✅ Almacenan la clave privada en Hardware Security Module (HSM)
- ✅ Firman automáticamente tus builds desde GitHub Actions
- ✅ Eliminan las advertencias de Windows SmartScreen

## 📋 Requisitos de Elegibilidad

Tu proyecto debe cumplir:

### ✅ Requisitos Obligatorios

- **Licencia Open Source**: Debe usar una licencia aprobada por OSI
  - ✅ Tu proyecto usa **MIT** (aprobada)
- **Código 100% Open Source**: Sin código propietario
- **Activamente mantenido**: Commits recientes y desarrollo activo
- **Sin malware**: Código limpio y seguro
- **Documentación**: README con descripción clara del proyecto
- **Política de firma**: Documento público de cómo se firma el código
- **MFA habilitado**: Todos los colaboradores deben tener autenticación de dos factores

### ✅ Mejores Prácticas (Recomendadas)

- Proyecto con historial de commits
- Comunidad activa (issues, PRs)
- Documentación clara
- Tests automatizados

## 📝 Proceso de Aplicación

### Paso 1: Preparar tu Repositorio

1. **Verifica que tu repositorio sea público**
   - `https://github.com/firsttabz/cbus-printer-server` ✅

2. **Asegúrate de tener licencia MIT visible**
   - Crea archivo `LICENSE` si no existe

3. **Habilita MFA en GitHub**
   - Settings → Password and authentication → Two-factor authentication

4. **Crea política de firma de código**
   - Agrega sección en README o crea `CODE_SIGNING_POLICY.md`

### Paso 2: Aplicar a SignPath Foundation

1. **Ve al sitio de SignPath Foundation**
   - URL: `https://about.signpath.io/product/open-source`

2. **Completa el formulario de aplicación**
   - Nombre del proyecto: **CBus Printer Server**
   - URL del repositorio: `https://github.com/firsttabz/cbus-printer-server`
   - Licencia: **MIT**
   - Descripción: Servidor de impresiones para sistema CBus
   - Plataformas: **Windows**

3. **Espera la aprobación**
   - Tiempo estimado: 1-2 semanas
   - Recibirás un email con instrucciones

### Paso 3: Configurar SignPath (Después de Aprobación)

Una vez aprobado, SignPath te proporcionará:

1. **API Token**: Token de autenticación
2. **Organization ID**: ID de tu organización
3. **Project Slug**: Identificador de tu proyecto

### Paso 4: Configurar GitHub Secrets

Agrega los siguientes secretos en tu repositorio:

1. Ve a: `https://github.com/firsttabz/cbus-printer-server/settings/secrets/actions`

2. Agrega estos secretos:

   **SIGNPATH_API_TOKEN**
   - Value: El token que te proporcionó SignPath

   **SIGNPATH_ORGANIZATION_ID**
   - Value: Tu Organization ID

   **SIGNPATH_PROJECT_SLUG**
   - Value: El slug de tu proyecto (ej: `cbus-printer-server`)

### Paso 5: Configurar SignPath Project

En el portal de SignPath:

1. **Crea un Signing Policy**
   - Nombre: `release-signing`
   - Tipo: Release signing

2. **Crea un Artifact Configuration**
   - Nombre: `electron-installer`
   - Tipo: Portable Executable (PE)
   - Pattern: `*.exe`

3. **Conecta con GitHub**
   - Autoriza SignPath a acceder a tu repositorio
   - Configura el webhook

## 🚀 Primer Release Firmado

Una vez configurado todo:

```bash
# Actualiza versión en package.json
# Luego crea el tag
git tag v1.9.12
git push origin v1.9.12
```

El workflow automáticamente:
1. ✅ Compila la aplicación
2. ✅ Sube el instalador a SignPath
3. ✅ SignPath firma el instalador
4. ✅ Descarga el instalador firmado
5. ✅ Publica la release en GitHub

## 📄 Política de Firma de Código (Ejemplo)

Agrega esto a tu README o crea `CODE_SIGNING_POLICY.md`:

```markdown
## Code Signing Policy

CBus Printer Server uses code signing to ensure the authenticity and integrity of our releases.

### Signing Process

- All Windows releases are signed using SignPath Foundation
- Code signing certificates are managed by SignPath Foundation
- Private keys are stored in Hardware Security Modules (HSM)
- Only releases built from tagged commits on the main branch are signed
- All builds are performed on GitHub-hosted runners
- Signing is automated through GitHub Actions

### Verification

Users can verify the signature of our releases:

1. Download the installer from GitHub Releases
2. Right-click → Properties → Digital Signatures
3. Verify the signature is from "SignPath Foundation"

### Security

- All team members have MFA enabled
- Only authorized maintainers can create release tags
- Build process is fully automated and auditable
```

## ✅ Checklist de Aplicación

Antes de aplicar, verifica:

- [ ] Repositorio es público
- [ ] Licencia MIT visible en el repositorio
- [ ] README con descripción clara del proyecto
- [ ] MFA habilitado en tu cuenta de GitHub
- [ ] Política de firma de código documentada
- [ ] Proyecto activamente mantenido (commits recientes)
- [ ] Sin código propietario o malware
- [ ] GitHub Actions workflow configurado

## 🔗 Enlaces Útiles

- **Aplicar**: https://about.signpath.io/product/open-source
- **Documentación**: https://about.signpath.io/documentation
- **GitHub Action**: https://github.com/signpath/github-action-submit-signing-request
- **Proyectos que usan SignPath**: Git Extensions, LiteDB, Flameshot, Stellarium

## 💡 Consejos

1. **Sé paciente**: El proceso de aprobación puede tomar 1-2 semanas
2. **Documentación clara**: Asegúrate de que tu README explique bien qué hace tu app
3. **Actividad reciente**: Haz algunos commits antes de aplicar
4. **Responde rápido**: Si SignPath te pide información adicional, responde pronto

## 🆘 Si No Calificas

Si tu proyecto no califica para SignPath Foundation:

**Alternativas:**
1. **Certificado auto-firmado** (gratis, pero con advertencias)
2. **Certificado comercial** ($75-200/año)
3. **No firmar** (solo para uso interno)

## 📞 Soporte

Si tienes preguntas sobre SignPath Foundation:
- Email: foundation@signpath.io
- Documentación: https://about.signpath.io/documentation
