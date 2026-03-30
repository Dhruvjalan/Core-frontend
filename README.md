# Core Frontend

This repository contains the React-based frontend for the Core Project, a high-performance record management and search dashboard. It is integrated with a robust CI/CD pipeline for automated deployment and health verification.

---

## 🚀 Live Demo & Access

* **Public URL:** [http://13.63.165.243/core-frontend/dashboard](http://13.63.165.243/core-frontend/dashboard)
* **Demo Video:** [Watch the Walkthrough](https://drive.google.com/file/d/1SUBR7g88A8HfmBhAYl5qPmWmlnayY-ws/view?usp=sharing)

---

## 📁 Project Structure

The project follows a modular React/Vite architecture with TypeScript for type safety:

```text
core-frontend/
├── .github/workflows/    # CI/CD Deployment & Test Pipelines (YAML)
├── public/               # Static assets
├── src/
│   ├── assets/           # Global styles, SVGs, and images
│   │   └── styles/       # Component-specific CSS files
│   ├── components/       # Reusable UI components
│   ├── pages/            # View components (Login.tsx, Dashboard.tsx)
│   ├── App.tsx           # Main Routing logic
│   └── main.tsx          # Application entry point
├── smoke-test.sh         # Bash script for post-deployment API health checks
├── index.html            # SPA Template
├── package.json          # Dependencies and scripts
└── vite.config.ts        # Vite build and environment configuration
```

---

## 🔑 Access Instructions

The application uses an Smail-based roll number extraction for authentication. For testing and demonstration, access is restricted to the following authorized accounts:

### Authorized Credentials
To access the dashboard, use one of these Smail accounts at the login screen:
* `ed24b047@smail.iitm.ac.in`
* `me23b081@smail.iitm.ac.in`
* `ch23b056@smail.iitm.ac.in`

> **Authentication Logic:** The frontend extracts the first 8 characters of the email (the roll number) and verifies it against the backend database before granting access to the dashboard.

---

## ⚙️ Configuration (Secrets & Environment)

The application relies on specific environment variables for both the build process and the automated testing pipeline.

### GitHub Repository Secrets
To ensure the CI/CD pipeline runs successfully, the following secrets must be configured in **Settings > Secrets and variables > Actions**:

| Secret Name | Description |
| :--- | :--- |
| `EC2_HOST` | The Public IP of your Frontend EC2 instance. |
| `EC2_USER` | Usually `ubuntu`. |
| `EC2_SSH_KEY` | Your private `.pem` key content for SSH access. |
| `VITE_PROD_BACKEND_URL` | The URL of your production Backend API. |
| `VITE_DEV_BACKEND_URL` | The URL of your development/staging API. |
| `VITE_PRODUCTION` | Set to `true` for production builds. |
| `FRONTEND_BASE_URL` | The base URL where the frontend is hosted. |

### Local Environment (`.env`)
For local development, create a `.env` file in the root directory:
```env
VITE_PROD_BACKEND_URL=your_api_url
VITE_PRODUCTION=true
```

---

## 🛠 CI/CD Workflow & Testing Pipeline

The repository utilizes **GitHub Actions** to automate the deployment lifecycle.

### 1. Build & Injection
* The runner compiles the React application using `npm run build`.
* **Vite Env Injection:** GitHub Secrets are injected into the build, ensuring the `dist/` folder contains the correct API endpoints.

### 2. Automated Deployment (SCP)
* The `dist/` folder is transferred to `/var/www/html/core-frontend` on the EC2 instance.
* **Cleanup:** The `rm: true` flag ensures old assets are deleted before new ones are uploaded.

### 3. Permission & Health Check
* **SSH Step:** Automatically resets permissions on EC2: `sudo chown -R ubuntu:www-data /var/www/html/core-frontend`.
* **Smoke Test:** Executes `smoke-test.sh` to verify:
    * **User Check:** API responds to authorized roll numbers.
    * **Data Integrity:** The `e21-students` search returns exactly **50 records**.

---

## 💻 Local Development

1. **Clone:** `git clone <repo-url>`
2. **Install:** `npm install`
3. **Dev:** `npm run dev`
