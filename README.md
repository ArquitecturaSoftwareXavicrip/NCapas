# Sistema de Eventos Deportivos

Sistema web desarrollado con Django que implementa una arquitectura en 3 capas y el patrón MVC para la gestión de eventos deportivos, deportes y participantes.

## Arquitectura del Sistema

El proyecto sigue una arquitectura en 3 capas bien definida:

```
┌─────────────────────────────────────┐
│   Capa de Presentación (Views)     │
│   - Controllers (Django Views)      │
│   - Templates (HTML)                │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Capa de Lógica de Negocio         │
│   - Services (Business Logic)       │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│   Capa de Acceso a Datos            │
│   - Models (Django ORM)              │
│   - Repositories (Data Access)      │
└─────────────────────────────────────┘
```

### Capas Implementadas

1. **Capa de Presentación**
   - `views.py`: Controladores que manejan las peticiones HTTP
   - `templates/`: Vistas HTML para la interfaz de usuario
   - `forms.py`: Formularios Django para validación de entrada

2. **Capa de Lógica de Negocio**
   - `services.py`: Servicios que contienen la lógica de negocio y validaciones
   - `DeporteService`, `EventoService`, `ParticipanteService`

3. **Capa de Acceso a Datos**
   - `models.py`: Modelos de datos (Django ORM)
   - `repositories.py`: Repositorios que abstraen el acceso a la base de datos
   - `DeporteRepository`, `EventoRepository`, `ParticipanteRepository`

## Características

- ✅ CRUD completo para Deportes, Eventos y Participantes
- ✅ Relaciones entre entidades (Evento-Deporte, Evento-Participantes)
- ✅ Validaciones de negocio en la capa de servicios
- ✅ Separación clara de responsabilidades entre capas
- ✅ Interfaz web funcional con navegación entre páginas
- ✅ Admin de Django configurado para gestión rápida
- ✅ Pipeline CI/CD con GitHub Actions

## Modelos de Datos

### Deporte
- `id`: Identificador único
- `nombre`: Nombre del deporte (único)
- `descripcion`: Descripción del deporte
- `fecha_creacion`: Fecha de creación del registro

### Evento
- `id`: Identificador único
- `nombre`: Nombre del evento
- `deporte`: Relación ForeignKey con Deporte
- `fecha`: Fecha y hora del evento
- `lugar`: Lugar donde se realiza el evento
- `descripcion`: Descripción del evento
- `participantes`: Relación Many-to-Many con Participante (a través de EventoParticipante)
- `fecha_creacion`: Fecha de creación del registro

### Participante
- `id`: Identificador único
- `nombre`: Nombre del participante
- `apellido`: Apellido del participante
- `email`: Email del participante (único)
- `telefono`: Teléfono del participante (opcional)
- `fecha_creacion`: Fecha de creación del registro

### EventoParticipante
- Tabla intermedia para la relación Many-to-Many entre Evento y Participante
- `evento`: Relación ForeignKey con Evento
- `participante`: Relación ForeignKey con Participante
- `fecha_inscripcion`: Fecha de inscripción
- `numero_inscripcion`: Número de inscripción (opcional)

## Requisitos

- Python 3.9 o superior
- Django 4.2.27
- SQLite (incluido en Python)

## Instalación

1. Clonar el repositorio:
```bash
git clone <url-del-repositorio>
cd arquitecturaCapas
```

2. Crear un entorno virtual:
```bash
python3 -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

3. Instalar dependencias:
```bash
pip install -r requirements.txt
```

4. Ejecutar migraciones:
```bash
python manage.py migrate
```

5. Crear un superusuario (opcional):
```bash
python manage.py createsuperuser
```

6. Ejecutar el servidor de desarrollo:
```bash
python manage.py runserver
```

7. Acceder a la aplicación:
   - Aplicación web: http://127.0.0.1:8000/
   - Panel de administración: http://127.0.0.1:8000/admin/

## Estructura del Proyecto

```
arquitecturaCapas/
├── manage.py
├── requirements.txt
├── README.md
├── eventos_deportivos/          # Proyecto Django principal
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── eventos/                     # Aplicación Django
│   ├── models.py                # Capa de Acceso a Datos - Models
│   ├── repositories.py          # Capa de Acceso a Datos - Repositories
│   ├── services.py              # Capa de Lógica de Negocio
│   ├── views.py                 # Capa de Presentación - Controllers
│   ├── forms.py                 # Formularios Django
│   ├── urls.py                  # URLs de la aplicación
│   ├── admin.py                 # Configuración del admin
│   └── templates/               # Capa de Presentación - Views (Templates)
│       └── eventos/
│           ├── base.html
│           ├── home.html
│           ├── lista_deportes.html
│           ├── crear_deporte.html
│           ├── detalle_deporte.html
│           ├── lista_eventos.html
│           ├── crear_evento.html
│           ├── detalle_evento.html
│           ├── lista_participantes.html
│           └── crear_participante.html
├── static/                      # Archivos estáticos
│   └── css/
│       └── style.css
└── .github/                     # GitHub Actions workflows
    └── workflows/
        └── ci.yml               # Pipeline de CI/CD
```

## Reglas de Negocio Implementadas

### Deportes
- El nombre del deporte debe ser único
- No se puede eliminar un deporte que tenga eventos asociados

### Participantes
- El email debe ser único
- El email debe tener un formato válido
- Nombre y apellido son obligatorios

### Eventos
- No se pueden crear eventos en el pasado
- No se puede cambiar la fecha de un evento al pasado
- No se pueden inscribir participantes a eventos pasados
- Un participante no puede estar inscrito dos veces en el mismo evento

## Flujo de Datos

```
Usuario → View (Controller) → Service (Business Logic) → Repository (Data Access) → Model (Database)
         ↓
      Template (View) ← Response
```

## CI/CD

El proyecto incluye un pipeline de GitHub Actions (`.github/workflows/ci.yml`) que ejecuta:

- **Lint**: Verificación de código Python con flake8
- **Tests**: Ejecución de tests unitarios de Django
- **Migrations Check**: Verificación de migraciones
- **Security Scan**: Escaneo de vulnerabilidades en dependencias
- **Build**: Verificación de construcción exitosa

## Tecnologías Utilizadas

- **Backend**: Django 4.2.27
- **Base de Datos**: SQLite
- **Frontend**: HTML5, CSS3, Bootstrap 5.3
- **CI/CD**: GitHub Actions
- **Python**: 3.9+

## Autor

Sistema desarrollado como práctica de Arquitectura de Software - Arquitectura en 3 Capas

## Licencia

Este proyecto es de uso educativo.

