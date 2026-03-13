description: Transforma bases de código heredadas en estructuras modernas y mantenibles mediante la refactorización de patrones obsoletos, la actualización de dependencias y la aplicación de estándares de codificación contemporáneos.
tags: [code, refactoring, modernization, architecture]
author: Kilo
version: 1.0.0
dependencies:
  - Node.js (v16+ para proyectos JavaScript/TypeScript)
  - Python (v3.8+ para proyectos Python)
  - Git (v2.30+ para control de versiones)
  - ESLint/Prettier para linting de JS/TS
  - Black/Flake8 para formateo de Python
  - Docker (opcional, para builds contenerizados)
environment_variables:
  - CODE_MORPHER_TARGET_DIR: Ruta absoluta al directorio de la base de código (predeterminado: directorio de trabajo actual)
  - CODE_MORPHER_BACKUP_ENABLED: Establecer en 'true' para crear copias de seguridad automáticas antes de las transformaciones (predeterminado: true)
  - CODE_MORPHER_LINT_LEVEL: Establecer en 'strict', 'moderate' o 'lenient' para aplicación de linting (predeterminado: moderate)
last_updated: 2026-03-13
---

# Habilidad Code Morpher

## Propósito

Code Morpher está diseñado para modernizar y refactorizar bases de código heredadas, abordando problemas comunes como sintaxis obsoleta, patrones ineficientes, dependencias obsoletas y baja mantenibilidad. Transforma el código en estándares contemporáneos mientras preserva la funcionalidad y mejora el rendimiento.

### Casos de Uso Reales

- **Migración de JavaScript Heredado**: Convertir código ES5 a módulos ES6+, reemplazar var con let/const e implementar funciones de flecha en una aplicación Node.js de 10 años.
- **Actualización de Python 2 a 3**: Automatizar la conversión de scripts Python 2.x a Python 3.x, manejando declaraciones print, manejo de unicode y cambios de importaciones en un pipeline de procesamiento de datos.
- **Refactorización de Componentes de Clase React a Funcionales**: Refactorizar componentes React basados en clases a hooks funcionales, actualizando métodos de ciclo de vida a useEffect y gestión de estado a useState en una aplicación web a gran escala.
- **Optimización de Consultas de Base de Datos**: Modernizar consultas SQL crudas para usar patrones ORM (por ejemplo, Sequelize para Node.js o SQLAlchemy para Python), agregando manejo adecuado de errores y agrupamiento de conexiones.
- **Refactorización de Endpoints de API**: Convertir endpoints RESTful a resolvedores GraphQL o actualizar a estándares OpenAPI 3.0, incluyendo integración de middleware de autenticación.

## Alcance

Code Morpher opera en los siguientes comandos y transformaciones específicas:

- **transform-js**: Refactoriza archivos JavaScript/TypeScript (flags: --target=es6, --target=es2022, --preserve-comments=true)
- **transform-py**: Moderniza código Python (flags: --from-py2, --from-py3.6, --add-type-hints)
- **refactor-components**: Convierte componentes de UI a patrones modernos (flags: --framework=react, --framework=vue, --add-tests=true)
- **upgrade-deps**: Actualiza dependencias de package.json o requirements.txt (flags: --major-only=false, --check-vulnerabilities=true)
- **optimize-queries**: Refactoriza interacciones de base de datos (flags: --orm=sequelize, --orm=sqlalchemy, --add-migrations=true)
- **lint-and-fix**: Aplica correcciones automáticas de linting y formateo (flags: --tool=eslint, --tool=black, --fix-only=true)

## Proceso de Trabajo Detallado

1. **Inicialización**: Analizar el directorio objetivo para lenguajes y frameworks soportados. Crear una instantánea de respaldo usando `git tag backup-pre-morph-<timestamp>`.
2. **Evaluación de Dependencias**: Escanear package.json/requirements.txt en busca de versiones obsoletas. Marcar vulnerabilidades usando `npm audit` o `pip-audit`.
3. **Detección de Patrones**: Usar análisis AST para identificar patrones heredados (por ejemplo, declaraciones var, infierno de callbacks, I/O síncrono).
4. **Ejecución de Transformación**: Aplicar refactorizaciones específicas en lotes, ejecutando pruebas después de cada cambio importante para asegurar estabilidad.
5. **Linting y Formateo**: Ejecutar ESLint/Prettier o Black/Flake8 para aplicar consistencia de estilo.
6. **Integración de Pruebas**: Generar o actualizar pruebas unitarias para código refactorizado, asegurando cobertura del 80%+.
7. **Verificación Final**: Ejecutar suite completa de pruebas y verificaciones de linting. Generar un reporte de transformación detallando los cambios realizados.

## Reglas de Oro

- Siempre crear respaldos antes de las transformaciones; nunca modificar código de producción sin staging.
- Preservar funcionalidad existente; las transformaciones deben pasar todas las pruebas existentes.
- Manejar importaciones y dependencias con cuidado; importaciones rotas pueden causar errores en cascada.
- Usar cambios incrementales; evitar reescrituras masivas que introduzcan múltiples errores.
- Documentar todos los cambios en mensajes de commit con ejemplos claros de antes/después.
- Respetar convenciones del proyecto; imitar estilo de código y patrones existentes.
- Probar exhaustivamente; ejecutar la suite completa de pruebas después de cada lote de transformación.

## Ejemplos

### Ejemplo 1: Migración de JavaScript ES5 a ES6
**Prompt del Usuario**: `openclaw code-morpher transform-js --target=es2020 --preserve-comments=true /path/to/legacy-app/src`

**Código de Entrada** (legacy.js):
```javascript
var express = require('express');
var app = express();

function handleRequest(req, res) {
    res.send('Hello World');
}

app.get('/', handleRequest);
module.exports = app;
```

**Código de Salida** (modernized.js):
```javascript
import express from 'express';
const app = express();

const handleRequest = (req, res) => {
    res.send('Hello World');
};

app.get('/', handleRequest);
export default app;
```

**Salida de CLI**: Transformados 15 archivos, actualizadas 8 dependencias, corregidos 23 problemas de linting. Ejecutar `npm test` para verificar.

### Ejemplo 2: Actualización de Python 2 a 3
**Prompt del Usuario**: `openclaw code-morpher transform-py --from-py2 --add-type-hints /path/to/data-pipeline`

**Código de Entrada** (legacy.py):
```python
import urllib2

def fetch_data(url):
    response = urllib2.urlopen(url)
    data = response.read()
    print data
    return data
```

**Código de Salida** (modernized.py):
```python
from typing import str
import urllib.request

def fetch_data(url: str) -> str:
    with urllib.request.urlopen(url) as response:
        data = response.read().decode('utf-8')
        print(data)
        return data
```

**Salida de CLI**: Migrados 42 archivos de Python 2 a 3, agregados type hints a 28 funciones, actualizadas importaciones para urllib.

### Ejemplo 3: Refactorización de Clase React a Hooks
**Prompt del Usuario**: `openclaw code-morpher refactor-components --framework=react --add-tests=true /path/to/react-app/components`

**Código de Entrada** (LegacyComponent.js):
```javascript
import React from 'react';

class LegacyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = { count: 0 };
    }

    componentDidMount() {
        console.log('Mounted');
    }

    increment = () => {
        this.setState({ count: this.state.count + 1 });
    }

    render() {
        return <button onClick={this.increment}>{this.state.count}</button>;
    }
}

export default LegacyComponent;
```

**Código de Salida** (ModernComponent.js):
```javascript
import React, { useState, useEffect } from 'react';

const ModernComponent = () => {
    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log('Mounted');
    }, []);

    const increment = () => {
        setCount(prevCount => prevCount + 1);
    };

    return <button onClick={increment}>{count}</button>;
};

export default ModernComponent;
```

**Salida de CLI**: Refactorizados 12 componentes a hooks, generados 8 nuevos archivos de pruebas, actualizadas 5 interfaces de props.

## Pasos de Verificación

1. **Ejecutar Pruebas**: Ejecutar `npm test` o `pytest` para asegurar no hay regresiones.
2. **Verificación de Linting**: Ejecutar `npx eslint .` o `black --check .` para verificar cumplimiento de estilo de código.
3. **Verificación de Build**: Intentar un build completo con `npm run build` o `python setup.py build` para detectar errores de compilación.
4. **Auditoría de Dependencias**: Usar `npm audit` o `pip list --outdated` para confirmar no hay nuevas vulnerabilidades.
5. **Verificación de Rendimiento**: Perfilar funciones clave para regresiones de rendimiento usando herramientas como Lighthouse o cProfile.

## Solución de Problemas

- **Errores de Sintaxis Después de Transformar**: Verificar análisis AST incompleto; revisar manualmente el archivo transformado y ajustar casos extremos.
- **Fallos de Importación**: Asegurar que todas las dependencias se actualicen en package.json/requirements.txt; ejecutar `npm install` o `pip install -r requirements.txt`.
- **Fallos de Pruebas**: Las transformaciones pueden cambiar firmas de funciones; actualizar pruebas para coincidir con nuevas APIs o revertir cambios específicos.
- **Problemas de Memoria**: Para bases de código grandes, procesar en lotes más pequeños usando el flag `--batch-size=10`.
- **Errores Específicos de Framework**: Verificar compatibilidad de framework (por ejemplo, la versión de React debe soportar hooks); actualizar framework si es necesario.
- **Rollback Necesario**: Si los problemas persisten, usar git para revertir cambios: `git reset --hard backup-pre-morph-<timestamp>`.

## Comandos de Rollback

- **Revertir Completo**: `git reset --hard backup-pre-morph-<timestamp>` (restaura la base de código completa al estado pre-transformación).
- **Deshacer Selectivo**: `git revert <commit-hash>` (deshace commits específicos de transformación mientras preserva otros).
- **Rollback de Dependencias**: `npm install <package>@<previous-version>` o `pip install <package>==<previous-version>` (revierte actualizaciones específicas de paquetes).
- **Restaurar Archivo Específico**: `git checkout HEAD~1 -- <file-path>` (restaura archivos individuales desde antes del último commit).
- **Revertir Limpio**: `git clean -fd && git reset --hard HEAD` (remueve archivos no rastreados y resetea al último commit, útil si las transformaciones agregaron archivos temporales).