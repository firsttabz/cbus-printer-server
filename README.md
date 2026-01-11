# CBus Printer Server

[![Code Signing](https://img.shields.io/badge/code%20signing-SignPath%20Foundation-blue)](CODE_SIGNING_POLICY.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub Release](https://img.shields.io/github/v/release/firsttabz/cbus-printer-server)](https://github.com/firsttabz/cbus-printer-server/releases)

Servidor de impresiones para el sistema CBus. Aplicación Electron que permite imprimir tickets y documentos desde el sistema web CBus.

> **🔐 Code Signing**: This project uses [SignPath Foundation](https://signpath.org) for free code signing. All Windows releases are digitally signed to ensure authenticity and eliminate security warnings.

## 🚀 Características

- ✅ Impresión directa USB (ESC/POS)
- ✅ Impresión gráfica (imágenes)
- ✅ Impresión estructurada (JSON)
- ✅ API REST para integración
- ✅ Actualizaciones automáticas
- ✅ Inicio automático con Windows
- ✅ Minimización a bandeja del sistema

## 📦 Instalación

### Para Usuarios

1. Descarga el instalador desde [GitHub Releases](https://github.com/firsttabz/cbus-printer-server/releases)
2. Ejecuta el instalador `.exe`
3. La aplicación se iniciará automáticamente
4. Configura tu impresora desde la interfaz

### Para Desarrolladores

```bash
# Clona el repositorio
git clone https://github.com/firsttabz/cbus-printer-server.git
cd cbus-printer-server

# Instala dependencias
npm install

# Ejecuta en modo desarrollo
npm start

# Compila la aplicación
npm run make
```

## 🔧 Configuración

### Selección de Impresora

1. Abre la aplicación
2. Selecciona tu impresora de la lista
3. Elige el modo de impresión (USB directo o gráfico)
4. Configura el puerto del servidor (por defecto: 3090)

### API Endpoints

La aplicación expone los siguientes endpoints:

#### `POST /print-escpos`
Impresión directa USB usando comandos ESC/POS.

```json
{
  "clave": "ABC123",
  "unidad": "101",
  "vendedor": "Juan",
  "costo_boleto": "250.00"
}
```

#### `POST /print-server`
Impresión gráfica de imágenes (tickets en formato JPG).

```json
{
  "ticket": "data:image/jpeg;base64,...",
  "ref": "ticket_123"
}
```

#### `POST /print-json`
Impresión estructurada usando formato JSON.

```json
{
  "type": "text",
  "value": "Contenido del ticket",
  "style": "bold"
}
```

## 🔄 Actualizaciones Automáticas

La aplicación se actualiza automáticamente:
- Verifica nuevas versiones cada 5 minutos
- Descarga e instala actualizaciones en segundo plano
- Notifica al usuario cuando hay una actualización disponible

## 📚 Documentación

- [Code Signing Policy](./CODE_SIGNING_POLICY.md) - Política de firma de código
- [Configuración de SignPath Foundation](./docs/CODE_SIGNING_SETUP.md) - Guía de aplicación
- [Proceso de Release](./docs/RELEASE_PROCESS.md)
- [Troubleshooting](./docs/TROUBLESHOOTING.md)
- [Documentación de API](./src/api_docs.html)

## 🛠️ Desarrollo

### Estructura del Proyecto

```
cbus-printer-server/
├── .github/
│   └── workflows/
│       └── release.yml          # GitHub Actions workflow
├── docs/                        # Documentación
├── src/
│   ├── main.js                  # Proceso principal de Electron
│   ├── preload.js               # Script de preload
│   ├── renderer.js              # Lógica del frontend
│   ├── index.html               # Interfaz principal
│   ├── logs.html                # Ventana de logs
│   └── api_docs.html            # Documentación de API
├── forge.config.js              # Configuración de Electron Forge
└── package.json
```

### Scripts Disponibles

```bash
npm start          # Inicia la aplicación en modo desarrollo
npm run package    # Empaqueta la aplicación
npm run make       # Crea instaladores
npm run publish    # Publica a GitHub Releases
```

### Tecnologías Utilizadas

- **Electron** - Framework para aplicaciones de escritorio
- **Express** - Servidor HTTP para la API
- **electron-pos-printer** - Impresión de tickets
- **escpos-win** - Comandos ESC/POS para impresoras USB
- **update-electron-app** - Actualizaciones automáticas

## 🚀 Release y Publicación

Para crear una nueva versión:

1. Actualiza la versión en `package.json`
2. Crea un tag de versión:
   ```bash
   git tag v1.9.12
   git push origin v1.9.12
   ```
3. GitHub Actions compilará y publicará automáticamente

Ver [Proceso de Release](./docs/RELEASE_PROCESS.md) para más detalles.

## 🔐 Firma de Código

La aplicación está firmada con un certificado de código válido para evitar advertencias de Windows SmartScreen.

Ver [Configuración de Firma de Código](./docs/CODE_SIGNING_SETUP.md) para configurar el certificado.

## 🐛 Troubleshooting

Si encuentras problemas, consulta la [Guía de Troubleshooting](./docs/TROUBLESHOOTING.md).

## 📄 Licencia

MIT

## 👤 Autor

**Alejandro** - CBus

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Haz fork del proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📞 Soporte

Para soporte, abre un [issue](https://github.com/firsttabz/cbus-printer-server/issues) en GitHub.
