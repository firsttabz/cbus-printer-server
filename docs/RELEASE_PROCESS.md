# Proceso de Release

Guía rápida para crear y publicar una nueva versión de **CBus Printer Server**.

## 📦 Proceso Completo

### 1. Actualizar Versión

Edita `package.json` y actualiza el número de versión:

```json
{
  "version": "1.9.12"
}
```

### 2. Commit de Cambios

```bash
git add .
git commit -m "Release v1.9.12"
git push origin main
```

### 3. Crear Tag de Versión

```bash
# Crea el tag
git tag v1.9.12

# Publica el tag
git push origin v1.9.12
```

### 4. Monitorear GitHub Actions

1. Ve a: `https://github.com/firsttabz/cbus-printer-server/actions`
2. El workflow **"Build and Release"** se ejecutará automáticamente
3. Espera a que termine (5-10 minutos)

### 5. Verificar Release

1. Ve a: `https://github.com/firsttabz/cbus-printer-server/releases`
2. La nueva release `v1.9.12` estará disponible
3. Descarga el instalador para probar

## 🔄 Actualizaciones Automáticas

Las aplicaciones instaladas se actualizarán automáticamente:

- **Intervalo de verificación**: Cada 5 minutos
- **Notificación al usuario**: Sí
- **Descarga automática**: Sí
- **Instalación**: Requiere confirmación del usuario

## 🛠️ Comandos Útiles

### Build Local (sin firma)

```bash
npm run make
```

### Build Local (con firma)

```powershell
# Configura las variables de entorno
$env:WINDOWS_CERT_FILE = "C:\ruta\a\certificado.pfx"
$env:WINDOWS_CERT_PASSWORD = "tu-contraseña"

# Compila
npm run make
```

### Publicar Manualmente

```bash
npm run publish
```

## 📋 Checklist de Release

- [ ] Actualizar versión en `package.json`
- [ ] Probar la aplicación localmente
- [ ] Commit y push de cambios
- [ ] Crear y publicar tag
- [ ] Verificar que el workflow se ejecuta correctamente
- [ ] Descargar y probar el instalador
- [ ] Verificar que la firma es válida
- [ ] Probar actualización automática

## 🚨 Troubleshooting

### El workflow falla

1. Revisa los logs en GitHub Actions
2. Verifica que los secretos estén configurados correctamente
3. Asegúrate de que el certificado sea válido

### La aplicación no se actualiza

1. Verifica que la versión en `package.json` sea mayor que la instalada
2. Revisa los logs de la aplicación
3. Espera al menos 5 minutos después de la instalación

### Advertencias de Windows

Si Windows muestra advertencias:
1. Verifica la firma con `Get-AuthenticodeSignature`
2. Asegúrate de que el certificado sea de una CA reconocida
3. Verifica que el certificado esté vigente

## 📚 Documentación Adicional

- [Configuración de Firma de Código](./CODE_SIGNING_SETUP.md)
- [Electron Forge Documentation](https://www.electronforge.io/)
- [GitHub Releases](https://docs.github.com/en/repositories/releasing-projects-on-github)
