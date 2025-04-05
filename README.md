# Proyecto Final DAW - Films + Web de Mantenimiento

Este documento proporciona las instrucciones necesarias para configurar y utilizar tanto el entorno de desarrollo como el entorno de producción para el proyecto, incluyendo la web de mantenimiento.

---

## ⚙️ Requisitos previos

Asegúrate de tener instalados:

- **Git**: Para gestionar el código fuente.
- **Docker**: Para crear entornos aislados.
- **Docker Compose**: Para orquestar múltiples contenedores.

---

## 🧪 Instrucciones para entorno de desarrollo

### 1. Clonar los repositorios

```bash
git clone git@github.com:Luissanju/films-frontend.git
git clone git@github.com:Luissanju/films-backend.git
git clone https://github.com/Luissanju/films-mantenimiento.git
```

---

### 2. Levantar contenedores

#### Backend

```bash
cd films-backend
docker-compose -f docker-compose-dev.yml up -d
```

#### Frontend

```bash
cd films-frontend
docker-compose -f docker-compose-dev.yml up -d
```

#### Mantenimiento

```bash
cd films-mantenimiento
docker-compose -f docker-compose-dev.yml up -d
```

---

### 3. Acceder a las aplicaciones

Añade estas líneas al archivo `hosts`:

```
127.0.0.1 films.dev.com
127.0.0.1 films.api.dev.com
127.0.0.1 mantenimiento.dev.com
```

- **Frontend:** http://films.dev.com:8081  
- **Backend:** http://films.api.dev.com:8080/api  
- **Mantenimiento:** http://mantenimiento.dev.com:8081

📍 Archivo `hosts`:
- **Linux/macOS**: `/etc/hosts`

---

### 4. Parar los contenedores

Situado en los respectivos directorios
docker-compose -f docker-compose-dev.yml down
```

---

## 🚀 Instrucciones para entorno de producción (AWS EC2)

### 1. Clonar los repositorios en la instancia EC2

```bash
git clone git@github.com:Luissanju/films-frontend.git
git clone git@github.com:Luissanju/films-backend.git
git clone https://github.com/Luissanju/films-mantenimiento.git
```

---

### 2. Añadir archivo `.env` a `films-backend`

Crea el archivo `films-backend/config/prod/.env` con:

```env
MYSQL_ROOT_PASSWORD=root_password
MYSQL_USER=dbuser
MYSQL_PASSWORD=dbpassword
```

---

### 3. Levantar los contenedores

#### Backend

```bash
cd films-backend
docker-compose -f docker-compose.yml up -d --build
```

#### Frontend

```bash
cd films-frontend
docker-compose -f docker-compose.yml up -d --build
```

#### Mantenimiento

```bash
cd films-mantenimiento
docker-compose -f docker-compose.yml up -d --build
```

---

### 4. Configurar dominio (FreeDNS)

Asegúrate de que tienes los siguientes dominios registrados en FreeDNS:

- `films-luis-daw.api.chickenkiller.com` → Backend
- `films-luis-daw.chickenkiller.com` → Frontend
- `mantenimiento-luis-daw.chickenkiller.com` → Web de mantenimiento

---

### 5. Activar la web de mantenimiento temporalmente

En el proyecto del **frontend**, edita el archivo `.htaccess` para activar la redirección:

```apache
RewriteEngine On
RewriteCond %{HTTP_HOST} ^films-luis-daw\.chickenkiller\.com$ [NC]
RewriteRule ^(.*)$ http://mantenimiento-luis-daw.chickenkiller.com [R=302,L]
```

Guarda y reinicia el contenedor del frontend si es necesario.

---

### 6. Acceder a las aplicaciones

- **Frontend:** http://films-luis-daw.chickenkiller.com  
- **Backend:** http://films-luis-daw.api.chickenkiller.com:8080/api  
- **Mantenimiento:** http://mantenimiento-luis-daw.chickenkiller.com

---

### 7. Parar los contenedores

```bash
docker-compose -f docker-compose.yml down
```
