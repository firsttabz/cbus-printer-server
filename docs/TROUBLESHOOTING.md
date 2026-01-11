# Troubleshooting - CBus Printer Server

Guía de solución de problemas para la configuración de firma de código y releases.

## 🔐 Problemas de Firma de Código

### Error: "Certificate file not found"

**Síntomas:**
- El workflow de GitHub Actions falla en el paso "Build and publish"
- Mensaje de error: "Certificate file not found" o similar

**Causas posibles:**
1. El secreto `WINDOWS_CERTIFICATE` no está configurado
2. El contenido Base64 está incompleto o mal formateado
3. El secreto tiene espacios adicionales

**Soluciones:**

```powershell
# 1. Regenera el Base64 correctamente
$certBytes = [System.IO.File]::ReadAllBytes("tu-certificado.pfx")
$certBase64 = [System.Convert]::ToBase64String($certBytes)
$certBase64 | Out-File -FilePath "cert-base64.txt" -Encoding ASCII -NoNewline

# 2. Verifica que el archivo no tenga saltos de línea
Get-Content "cert-base64.txt" -Raw
```

3. Actualiza el secreto en GitHub:
   - Settings → Secrets → Actions
   - Elimina `WINDOWS_CERTIFICATE`
   - Crea uno nuevo con el contenido correcto

---

### Error: "Invalid password"

**Síntomas:**
- Error al intentar usar el certificado
- Mensaje: "Invalid password" o "Cannot decrypt certificate"

**Soluciones:**

1. Verifica la contraseña del certificado:
   ```powershell
   # Intenta abrir el certificado localmente
   $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
   $cert.Import("tu-certificado.pfx", "tu-contraseña", "DefaultKeySet")
   ```

2. Actualiza el secreto `WINDOWS_CERTIFICATE_PASSWORD` en GitHub

---

### Windows muestra advertencias de seguridad

**Síntomas:**
- Al ejecutar el instalador, Windows muestra "Windows protected your PC"
- SmartScreen bloquea la aplicación

**Causas posibles:**
1. El certificado no es de una CA reconocida
2. El certificado está vencido
3. La firma no se aplicó correctamente
4. El certificado es nuevo (SmartScreen necesita "reputación")

**Soluciones:**

```powershell
# Verifica la firma del instalador
Get-AuthenticodeSignature "cbusprinterserver-Setup-1.9.12.exe" | Format-List

# Resultado esperado:
# Status: Valid
# SignerCertificate: [Tu certificado]
```

**Si Status es "Valid" pero sigue mostrando advertencias:**
- Los certificados nuevos necesitan tiempo para ganar "reputación" en SmartScreen
- Esto puede tomar semanas o meses de descargas
- Considera comprar un certificado EV (Extended Validation) para reputación inmediata

---

## 🚀 Problemas de GitHub Actions

### El workflow no se ejecuta

**Síntomas:**
- Haces push del tag pero no aparece en Actions

**Soluciones:**

1. Verifica que el tag tenga el formato correcto:
   ```bash
   # Correcto
   git tag v1.9.12
   
   # Incorrecto
   git tag 1.9.12
   git tag release-1.9.12
   ```

2. Asegúrate de hacer push del tag:
   ```bash
   git push origin v1.9.12
   ```

3. Verifica que el workflow existe:
   - Ve a `.github/workflows/release.yml`
   - Asegúrate de que el archivo esté en la rama `main`

---

### El workflow falla en "Install dependencies"

**Síntomas:**
- Error: "npm ERR! code ENOTFOUND"
- Falla al instalar dependencias

**Soluciones:**

1. Verifica que `package-lock.json` esté committeado
2. Asegúrate de que todas las dependencias sean públicas
3. Revisa los logs para identificar qué paquete falla

---

### El workflow falla en "Build and publish"

**Síntomas:**
- Error durante la compilación
- Falla al crear el instalador

**Soluciones:**

1. Prueba compilar localmente:
   ```bash
   npm ci
   npm run make
   ```

2. Revisa los logs del workflow en GitHub Actions

3. Verifica que `forge.config.js` sea válido:
   ```bash
   node -c forge.config.js
   ```

---

## 📦 Problemas de Actualizaciones Automáticas

### La aplicación no se actualiza

**Síntomas:**
- Hay una nueva versión disponible pero la app no se actualiza
- No aparece notificación de actualización

**Soluciones:**

1. Verifica que la versión en `package.json` sea mayor:
   ```json
   // Versión instalada: 1.9.11
   // Nueva versión debe ser: 1.9.12 o superior
   ```

2. Espera al menos 5 minutos después de instalar

3. Revisa los logs de la aplicación:
   - Windows: `%APPDATA%\cbusprinterserver\logs`

4. Verifica que `update-electron-app` esté configurado:
   ```javascript
   updateElectronApp({
     repo: 'firsttabz/cbus-printer-server',
     notifyUser: true,
     updateInterval: '5 minutes',
   });
   ```

---

### Error: "Cannot find update"

**Síntomas:**
- La app busca actualizaciones pero no las encuentra

**Soluciones:**

1. Verifica que la release esté publicada en GitHub:
   - Ve a `https://github.com/firsttabz/cbus-printer-server/releases`
   - Asegúrate de que NO sea un draft

2. Verifica que el instalador esté en la release:
   - Debe haber un archivo `.exe` adjunto

3. Verifica la configuración del repositorio en `main.js`

---

## 🛠️ Problemas de Compilación Local

### Error: "Cannot find module"

**Síntomas:**
- Error al ejecutar `npm run make`
- Falta un módulo

**Soluciones:**

```bash
# Limpia e reinstala dependencias
rm -rf node_modules package-lock.json
npm install
```

---

### Error: "ENOENT: no such file or directory"

**Síntomas:**
- Falta un archivo durante la compilación

**Soluciones:**

1. Verifica que todos los archivos referenciados existan:
   - `icon.ico`
   - `src/icon.ico`
   - Archivos en `src/`

2. Verifica las rutas en `forge.config.js`

---

## 📞 Obtener Ayuda

Si ninguna de estas soluciones funciona:

1. **Revisa los logs completos**:
   - GitHub Actions: Ve a la pestaña "Actions" y abre el workflow fallido
   - Local: Ejecuta con `--verbose`: `npm run make -- --verbose`

2. **Busca en los issues del repositorio**:
   - `https://github.com/firsttabz/cbus-printer-server/issues`

3. **Crea un nuevo issue** con:
   - Descripción del problema
   - Logs completos
   - Pasos para reproducir
   - Versión de Node.js y npm

## 🔍 Comandos de Diagnóstico

```bash
# Verifica versiones
node --version
npm --version

# Verifica la configuración de Forge
npx electron-forge config

# Prueba la compilación sin publicar
npm run make

# Verifica la sintaxis de archivos
node -c forge.config.js
node -c src/main.js
```

```powershell
# Verifica la firma de un instalador (Windows)
Get-AuthenticodeSignature "instalador.exe" | Format-List

# Verifica el certificado
$cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$cert.Import("certificado.pfx", "contraseña", "DefaultKeySet")
$cert | Format-List
```
