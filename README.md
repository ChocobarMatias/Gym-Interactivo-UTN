# Gym-Interactivo-UTN
🏋️‍♂️ Gym Interactivo – UTN FRT x SoluxionCode

Plataforma Full Stack desarrollada como proyecto integrador UTN FRT.
Permite a los gimnasios y entrenadores gestionar socios, rutinas, pagos, métricas y recomendaciones automáticas con base en el Índice de Masa Corporal (IMC).
Optimizada para interacción directa en feria mediante QR, y preparada para integrarse con una IA de asesoramiento físico y nutricional.

🚀 Objetivos del Proyecto

Desarrollar una web app full stack funcional para administración de gimnasios.

Crear un panel interactivo para que usuarios y entrenadores registren progreso.

Incorporar cálculo automático de IMC y recomendaciones personalizadas.

Permitir acceso rápido por QR público (demo FitCompa).

Integrar una IA “coach” que responda consultas y envíe rutinas/dietas.

🧩 Arquitectura General

Frontend:   React + Vite + Zustand + Styled Components + SweetAlert2
Backend:    Node.js + Express
Database:   MySQL (Hostinger VPS)
Hosting:    Netlify (frontend) + VPS (backend + DB)
IA Coach:   OpenAI API (GPT-4o-mini)


Flujo general

Usuario → (QR) → App React FitCompa → API Node.js → MySQL
                                    ↳ IA Coach → Chat respuesta personalizada

⚙️ Instalación y ejecución local
1️⃣ Clonar el proyecto
```bash
git clone https://github.com/ChocobarMatias/Gym-Interactivo-UTN.git
cd Gym-Interactivo-UTN
```
2️⃣ Instalar dependencias
```bash
npm install
```

3️⃣ Configurar entorno

Crear archivo .env en la raíz del backend:
---
PORT=8000
DB_HOST=su_host_db
DB_USER=su_usuario_db
DB_PASSWORD=TuPasswordFuerte123
DB_NAME=
JWT_SECRET=claveSuperSegura
OPENAI_API_KEY=tu_api_key_aqui
---

npm run dev
---bash
npm run dev

---
5️⃣ Iniciar frontend
---
cd frontend
npm install
npm run dev
---
5️⃣ Iniciar frontend
----
Gym-Interactivo-UTN/
├── backend/
│   ├── server.js
│   ├── routes/
│   ├── controllers/
│   ├── models/
│   ├── middlewares/
│   └── utils/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── store/
│   │   ├── services/
│   │   └── styles/
│   └── vite.config.js
└── README.md
---

🗃️ Estructura de Base de Datos (MySQL)
---sql
CREATE TABLE USUARIOS (
  ID_USUARIO INT AUTO_INCREMENT PRIMARY KEY,
  NOMBRE VARCHAR(50),
  APELLIDO VARCHAR(50),
  EMAIL VARCHAR(100) UNIQUE,
  PASSWORD VARCHAR(255),
  ROL ENUM('admin', 'entrenador', 'socio'),
  FECHA_REGISTRO DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE PERFIL_FISICO (
  ID_PERFIL INT AUTO_INCREMENT PRIMARY KEY,
  ID_USUARIO INT,
  ALTURA DECIMAL(5,2),
  PESO DECIMAL(5,2),
  IMC DECIMAL(5,2),
  META ENUM('bajar', 'mantener', 'subir'),
  FOREIGN KEY (ID_USUARIO) REFERENCES USUARIOS(ID_USUARIO)
);

CREATE TABLE RUTINAS (
  ID_RUTINA INT AUTO_INCREMENT PRIMARY KEY,
  NOMBRE VARCHAR(100),
  DESCRIPCION TEXT,
  NIVEL ENUM('principiante', 'intermedio', 'avanzado'),
  DURACION_SEMANA INT
);

CREATE TABLE PLANES (
  ID_PLAN INT AUTO_INCREMENT PRIMARY KEY,
  ID_USUARIO INT,
  ID_RUTINA INT,
  FECHA_INICIO DATE,
  FECHA_FIN DATE,
  ACTIVO BOOLEAN DEFAULT TRUE,
  FOREIGN KEY (ID_USUARIO) REFERENCES USUARIOS(ID_USUARIO),
  FOREIGN KEY (ID_RUTINA) REFERENCES RUTINAS(ID_RUTINA)
);

CREATE TABLE PAGOS (
  ID_PAGO INT AUTO_INCREMENT PRIMARY KEY,
  ID_USUARIO INT,
  MONTO DECIMAL(10,2),
  FECHA DATE,
  ESTADO ENUM('pendiente','pagado'),
  FOREIGN KEY (ID_USUARIO) REFERENCES USUARIOS(ID_USUARIO)
);
---

🧠 Funcionalidades clave

| Módulo                     | Función                    | Descripción                                    |
| -------------------------- | -------------------------- | ---------------------------------------------- |
| **Login / JWT**            | Autenticación segura       | Acceso diferenciado (admin, socio, entrenador) |
| **Cálculo IMC**            | Análisis físico automático | Genera recomendaciones y alertas               |
| **Gestión de socios**      | CRUD completo              | Alta, baja y edición desde panel admin         |
| **Rutinas personalizadas** | Sugerencia automática      | Según peso, altura, meta                       |
| **Pagos**                  | Control y recordatorios    | Estados, montos, fechas                        |
| **Dashboard KPIs**         | Gráficos y métricas        | Visualización de rendimiento                   |
| **Chat IA FitCompa**       | Interacción natural        | Responde dudas y genera dieta                  |


💬 Integración con IA FitCompa

Ejemplo de prompt:
---java Script
const prompt = `
Sos un entrenador de gimnasio profesional.
El usuario mide ${altura} cm, pesa ${peso} kg y tiene un IMC de ${imc}.
Generá una rutina de 4 días con ejercicios y una dieta saludable.
`;
---

La IA devuelve:
---json
{
  "rutina": "Fuerza + Cardio",
  "dias": ["Lunes", "Miércoles", "Viernes", "Sábado"],
  "dieta": "Alta en proteínas, baja en grasas saturadas"
}
---

📱 Modo Feria (interactivo con QR)

Los visitantes escanean un QR público (Netlify).

Ingresan datos físicos en una landing responsive.

Reciben:

IMC calculado en vivo,

Rutina y dieta generadas por IA,

Opción de guardar o recibir por WhatsApp.

En backend se guarda registro anónimo como “visitante”.

🔒 Seguridad y buenas prácticas

Passwords encriptadas con bcrypt.

Tokens JWT con expiración.

Sanitización de datos (express-validator).

CORS configurado solo para dominios del proyecto.

Backups automáticos con mysqldump en VPS Hostinger.

📈 Próximas integraciones (versión 2.0)

App móvil Flutter integrada al backend.

Módulo de sensores (pulso, movimiento, calorías).

Seguimiento con IA personalizada.

API pública para terceros (SmartBands, relojes).