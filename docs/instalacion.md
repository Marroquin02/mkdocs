# Instalación de NestJS

En esta guía aprenderás a configurar NestJS en tu entorno de desarrollo.

## Requisitos Previos

Antes de instalar NestJS, necesitas tener instalados:

- **Node.js** (versión 18 o superior)
- **npm** o **yarn** o **pnpm**
- Un editor de texto (VS Code recomendado)

## Métodos de Instalación

### Usando CLI de NestJS (Recomendado)

La forma más rápida de crear un nuevo proyecto NestJS.

=== "npm"
    ```bash
    npm i -g @nestjs/cli
    nest new nombre-proyecto
    ```

=== "yarn"
    ```bash
    yarn global add @nestjs/cli
    nest new nombre-proyecto
    ```

=== "pnpm"
    ```bash
    pnpm add -g @nestjs/cli
    nest new nombre-proyecto
    ```

### Usando Git

Clona un proyecto esqueleto directamente:

```bash
git clone https://github.com/nestjs/typescript-starter.git nombre-proyecto
cd nombre-proyecto
npm install
```

## Estructura del Proyecto

Después de crear el proyecto, verás la siguiente estructura:

```
nombre-proyecto/
├── src/
│   ├── app.controller.ts
│   ├── app.controller.spec.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── test/
├── node_modules/
├── package.json
├── tsconfig.json
└── nest-cli.json
```

## Archivos Principales

| Archivo | Propósito |
|---------|-----------|
| `main.ts` | Punto de entrada, bootstraps la aplicación |
| `app.module.ts` | Módulo raíz de la aplicación |
| `app.controller.ts` | Define las rutas base |
| `app.service.ts` | Lógica de negocio |

## Ejecutar en Desarrollo

Una vez instalado, inicia el servidor de desarrollo con:

```bash
npm run start:dev
```

Esto iniciará el servidor en `http://localhost:3000`.

## Verificación

Para verificar que todo está funcionando:

1. Abre tu navegador en `http://localhost:3000`
2. Deberías ver un mensaje de bienvenida JSON

> **Warning**: Si el puerto 3000 está en uso, NestJS automáticamente intentará con el siguiente puerto disponible.

## Próximo Paso

Continúa aprendiendo en [Módulos y Controladores](modulos-controladores.md).
