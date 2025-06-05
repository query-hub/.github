# QueryHub

**AI-powered business chat platform that learns from websites to provide intelligent customer service.**

[![Live Demo](https://img.shields.io/badge/Live-Demo-blue?style=for-the-badge)](https://query-hub-frontend.vercel.app)
[![API Docs](https://img.shields.io/badge/API-Documentation-green?style=for-the-badge)](https://backend-production.up.railway.app/docs)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-black?style=for-the-badge)](https://github.com/your-username/queryhub)

> **✨ From simple form handler to AI-powered knowledge system** - QueryHub grows with your business needs through progressive MVPs.

## 🚀 Quick Start

**Get QueryHub running in 3 ways:**

```bash
# Option 1: Local Development (Recommended)
git clone https://github.com/your-username/queryhub.git
cd queryhub
make local-setup    # Sets up Python + Node.js locally
make local-start    # Starts all services without Docker

# Option 2: Docker Development  
make setup          # Sets up with Docker Compose
make start          # Starts all services with Docker

# Option 3: Production Deploy
# Backend: Deploy to Railway
# Frontend: Deploy to Vercel
# See deployment section below
```

## 📋 Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Development Setup](#-development-setup)
- [Deployment](#-deployment)
- [API Usage](#-api-usage)
- [Project Structure](#-project-structure)
- [Scripts & Commands](#-scripts--commands)
- [Environment Variables](#-environment-variables)
- [Contributing](#-contributing)

## ✨ Features

### **Current (Phase 1: Form Handler MVP)**
- 🔥 **Zero-Backend Form Handler** - Accept form submissions with one line of HTML
- 📊 **Real-time Analytics** - Track submissions, common fields, conversion rates
- 🎛️ **Modern Dashboard** - React/Next.js dashboard with dark mode
- 🔐 **User Authentication** - JWT-based auth with user management
- 📱 **Responsive Design** - Works on desktop, tablet, and mobile
- ⚡ **Background Processing** - Celery-powered async task processing
- 🛡️ **CORS Handling** - Production-ready cross-origin support

### **Coming Soon (Progressive MVPs)**
- 🤖 **AI Chat Widget** - Intelligent chat that learns from your website
- 🕷️ **Website Crawler** - Automatic content discovery and understanding
- 🧠 **Knowledge Graph** - Neo4j-powered business intelligence
- 💬 **WhatsApp Integration** - Multi-channel customer service
- 📈 **Advanced Analytics** - Conversation insights and performance metrics

## 🏗️ Architecture

```mermaid
graph TB
    subgraph "Frontend (Vercel)"
        A[Next.js Dashboard] 
        B[Landing Pages]
    end
    
    subgraph "Backend (Railway)"
        C[FastAPI Server]
        D[Celery Workers]
        E[Background Tasks]
    end
    
    subgraph "Data Layer (Railway)"
        F[(PostgreSQL)]
        G[(Redis)]
    end
    
    subgraph "External Services"
        H[OpenAI API]
        I[Customer Websites]
    end
    
    A --> C
    B --> C
    I --> C
    C --> F
    C --> G
    D --> G
    D --> F
    C --> H
    E --> H
```

**Tech Stack:**
- **Backend**: Python 3.11, FastAPI, SQLAlchemy, Alembic
- **Frontend**: Next.js 15, TypeScript, Tailwind CSS v4
- **Database**: PostgreSQL 15 with async support
- **Cache/Queue**: Redis 7 with Celery
- **Deployment**: Railway (Backend) + Vercel (Frontend)
- **Development**: Docker Compose + Local Python/Node.js

## 🛠️ Development Setup

### **Prerequisites**

- **Python 3.11+** (preferably via pyenv)
- **Node.js 18+** and npm
- **PostgreSQL 15+** and **Redis 7+**
- **Git** and **Make**

### **Option 1: Local Development (Recommended)**

**Full setup with system dependencies:**
```bash
# 1. Clone and setup
git clone https://github.com/your-username/queryhub.git
cd queryhub

# 2. Install system dependencies
make bootstrap

# 3. Setup local development environment
make local-setup

# 4. Start all services locally
make local-start
```

**Quick setup (if you have dependencies):**
```bash
# Backend setup
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
alembic upgrade head

# Frontend setup  
cd ../frontend
npm install

# Start services
make local-start  # Or use individual terminal windows
```

### **Option 2: Docker Development**

```bash
# Full Docker setup
make setup      # Creates containers and databases
make start      # Starts all services
make logs       # View logs
make stop       # Stop services
```

### **Development URLs**

| Service | URL | Description |
|---------|-----|-------------|
| **Frontend** | http://localhost:3000 | Next.js dashboard |
| **Backend API** | http://localhost:8000 | FastAPI server |
| **API Docs** | http://localhost:8000/docs | Interactive Swagger UI |
| **ReDoc** | http://localhost:8000/redoc | Alternative API docs |

## 🚀 Deployment

### **Production Deployment (Railway + Vercel)**

**1. Backend to Railway:**
```bash
cd backend
npx @railway/cli login
npx @railway/cli init
npx @railway/cli up

# Add databases via Railway Dashboard:
# - PostgreSQL database
# - Redis database
```

**2. Frontend to Vercel:**
```bash
cd frontend
vercel login
vercel --prod
```

**3. Configure Environment Variables:**

**Railway (Backend):**
```env
DATABASE_URL=${{Postgres.DATABASE_URL}}
REDIS_URL=${{Redis.REDIS_URL}}
SECRET_KEY=your-secret-key
OPENAI_API_KEY=your-openai-key
ENVIRONMENT=production
```

**Vercel (Frontend):**
```env
NEXT_PUBLIC_API_URL=https://query-hub-backend-production.up.railway.app
```

### **Alternative Deployment Options**

- **Render**: Similar to Railway with Docker support
- **Google Cloud Run**: For containerized production deployment  
- **AWS ECS**: For enterprise-scale deployment
- **Self-hosted**: Use the Docker Compose setup

## 📡 API Usage

### **Simple Form Handler**

Add this to any website to start capturing submissions:

```html
<form action="https://query-hub-backend-production.up.railway.app/api/v1/forms/f/your-business-id" method="POST">
  <input type="text" name="name" placeholder="Name" required />
  <input type="email" name="email" placeholder="Email" required />
  <textarea name="message" placeholder="Message"></textarea>
  <button type="submit">Send Message</button>
</form>
```

### **JavaScript Integration**

```javascript
// Handle form submission with JavaScript
const formData = new FormData();
formData.append('name', 'John Doe');
formData.append('email', 'john@example.com');

fetch('https://query-hub-backend-production.up.railway.app/api/v1/forms/f/your-business-id', {
  method: 'POST',
  body: formData
})
.then(response => response.json())
.then(data => console.log('Success:', data));
```

### **API Endpoints**

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/forms/f/{business_id}` | Submit a form |
| `GET` | `/api/v1/forms/{business_id}/submissions` | Get submissions |
| `GET` | `/api/v1/forms/{business_id}/analytics` | Get analytics |
| `POST` | `/api/v1/auth/register` | Register user |
| `POST` | `/api/v1/auth/login` | Login user |
| `GET` | `/api/v1/businesses` | Get user businesses |

**Full API documentation:** [https://query-hub-backend-production.up.railway.app/docs](https://query-hub-backend-production.up.railway.app/docs)

## 📁 Project Structure

```
queryhub/
├── 🎯 README.md                    # You are here
├── 🐳 docker-compose.yml           # Local Docker setup
├── 📋 Makefile                     # Development commands
├── 🔧 .env.example                 # Environment template
│
├── 🐍 backend/                     # Python FastAPI backend
│   ├── 🐳 Dockerfile               # Backend container
│   ├── 📦 requirements.txt         # Python dependencies
│   ├── 🔧 alembic.ini             # Database migrations config
│   ├── 🗃️ alembic/                # Database migrations
│   └── 📱 app/                     # FastAPI application
│       ├── 🚀 main.py              # FastAPI app entry
│       ├── ⚙️ config.py            # App configuration
│       ├── 🌐 api/v1/              # API endpoints
│       ├── 🗂️ models/              # Database models
│       ├── 📋 schemas/             # Pydantic schemas
│       ├── 🔧 core/                # Core utilities
│       └── ⚡ background/          # Celery tasks
│
├── ⚛️ frontend/                    # Next.js React frontend
│   ├── 🐳 Dockerfile               # Frontend container  
│   ├── 📦 package.json             # Node.js dependencies
│   ├── ⚙️ next.config.ts           # Next.js configuration
│   ├── 🎨 tailwind.config.ts       # Tailwind CSS config
│   ├── 📱 app/                     # Next.js app directory
│   │   ├── 🏠 page.tsx             # Landing page
│   │   ├── 🔐 auth/                # Authentication pages
│   │   ├── 📊 dashboard/           # Dashboard pages
│   │   └── 📚 docs/                # Documentation pages
│   ├── 🧩 components/              # Reusable components
│   └── 📚 lib/                     # Utilities and API client
│
├── 🔮 widget/                      # Embeddable chat widget (Future)
│   ├── 📦 package.json
│   └── 🎯 src/
│
└── 📜 scripts/                     # Development scripts
    ├── 🏗️ bootstrap.sh             # Install system dependencies
    ├── ⚙️ setup.sh                 # Docker setup
    ├── 🚀 setup_local.sh           # Local development setup
    ├── ▶️ start_local.sh           # Start local services
    └── ⏹️ stop_local.sh            # Stop local services
```

## 🛠️ Scripts & Commands

### **Make Commands**

```bash
# 🏗️ Initial Setup
make bootstrap      # Install system dependencies (Docker, Node.js, etc.)
make setup         # Setup with Docker Compose
make local-setup   # Setup for local development

# 🚀 Development
make local-start   # Start all services locally (no Docker)
make start         # Start with Docker Compose  
make stop          # Stop all services
make restart       # Restart services

# 📊 Monitoring
make logs          # View all service logs
make local-logs    # View local service logs
make local-status  # Check service status
make ps           # List running containers

# 🗄️ Database
make migrate       # Run database migrations
make migration m="message"  # Create new migration
make db           # Access database shell

# 🧹 Maintenance  
make clean        # Remove Docker containers and volumes
make dev-reset    # Reset development environment
make fix          # Fix common issues
```

### **Individual Service Commands**

```bash
# Backend (FastAPI)
cd backend
source venv/bin/activate
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

# Frontend (Next.js)
cd frontend  
npm run dev

# Celery Worker
cd backend
source venv/bin/activate
celery -A app.background.celery_app worker --loglevel=info

# Database Migration
cd backend
source venv/bin/activate
alembic upgrade head
```

## 🔧 Environment Variables

### **Backend (.env)**
```env
# Database
DATABASE_URL=postgresql://queryhub:queryhub@localhost:5432/queryhub
REDIS_URL=redis://localhost:6379

# Security
SECRET_KEY=your-secret-key-change-this
JWT_ALGORITHM=HS256

# OpenAI (for future AI features)
OPENAI_API_KEY=your-openai-api-key

# App Configuration
ENVIRONMENT=development
DEBUG=true
```

### **Frontend (.env.local)**
```env
# API Configuration
NEXT_PUBLIC_API_URL=http://localhost:8000

# App Configuration  
NODE_ENV=development
```

### **🔐 Generating Secure Keys**

Generate a strong secret key for JWT tokens:

```bash
# Using Node.js (recommended)
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"

# Using Python
python -c "import secrets; print(secrets.token_urlsafe(32))"

# Using OpenSSL
openssl rand -base64 32

# Using online generator (for development only)
# https://generate-secret.vercel.app/32
```

**⚠️ Important**: Never commit real secret keys to version control. Use different keys for development and production.

## 🧪 Testing & Quality

```bash
# Run backend tests
cd backend && source venv/bin/activate && pytest

# Run frontend tests  
cd frontend && npm test

# Type checking
cd frontend && npm run type-check

# Linting
cd frontend && npm run lint
cd backend && flake8 app/

# Test form submission
curl -X POST http://localhost:8000/api/v1/forms/f/demo-business \
  -F "name=Test User" \
  -F "email=test@example.com" \
  -F "message=Hello QueryHub!"
```

## 🐛 Troubleshooting

### **Common Issues**

**Port conflicts:**
```bash
make fix          # Automatic issue detection and fixes
make force-stop   # Force stop all services
```

**Database connection issues:**
```bash
# Check if PostgreSQL is running
pg_isready -h localhost -p 5432

# Reset database
make clean && make setup
```

**Frontend/Backend connection issues:**
```bash
# Check CORS settings in backend/app/main.py
# Verify NEXT_PUBLIC_API_URL in frontend/.env.local
curl http://localhost:8000/health  # Test backend
```

**Permission issues (Linux/macOS):**
```bash
# Make scripts executable
chmod +x scripts/*.sh

# Fix Docker permissions
sudo usermod -aG docker $USER
# Logout and login again
```

**WSL Docker issues:**
```bash
# Use docker.exe instead of docker
make fix-wsl
```

## 🏗️ Development Workflow

### **Adding a New Feature**

1. **Backend**: Add endpoint in `backend/app/api/v1/`
2. **Database**: Create migration with `make migration m="add feature"`
3. **Frontend**: Add page/component in `frontend/app/`
4. **API Client**: Update `frontend/lib/api-client.ts`
5. **Test**: Use `make local-start` for development

### **Database Changes**

```bash
# 1. Modify model in backend/app/models/
# 2. Generate migration
make migration m="add new table"

# 3. Apply migration  
make migrate

# 4. Test changes
make local-start
```

### **Frontend Development**

```bash
# Start frontend only
cd frontend && npm run dev

# With backend API client
# API calls automatically proxy to http://localhost:8000
```

### **Adding Dependencies**

```bash
# Backend (Python)
cd backend
source venv/bin/activate
pip install new-package
pip freeze > requirements.txt

# Frontend (Node.js)
cd frontend
npm install new-package
# package.json automatically updated
```

## 🌟 Progressive MVP Roadmap

- [x] **Phase 1**: Form Handler MVP (Current)
  - [x] Zero-backend form submissions
  - [x] Real-time analytics dashboard
  - [x] User authentication & business management
  - [x] Production deployment (Railway + Vercel)
  - [x] Local & Docker development environments

- [ ] **Phase 2**: Website Crawler + Basic AI Chat
  - [ ] Web crawler service
  - [ ] Content extraction pipeline
  - [ ] Basic chat widget
  - [ ] AI-powered responses from crawled content

- [ ] **Phase 3**: Database Integration Layer
  - [ ] Multi-database connectors (PostgreSQL, MySQL, MongoDB)
  - [ ] Natural language to SQL queries
  - [ ] Real-time data sync
  - [ ] Query builder UI

- [ ] **Phase 4**: Knowledge Graph System
  - [ ] Neo4j graph database integration
  - [ ] Entity recognition and relationship mapping
  - [ ] Advanced context understanding
  - [ ] Business intelligence insights

- [ ] **Phase 5**: Multi-Channel Support
  - [ ] WhatsApp Business API integration
  - [ ] SMS and email automation
  - [ ] Unified messaging dashboard
  - [ ] Cross-platform conversation threading

- [ ] **Phase 6**: Enterprise Features
  - [ ] Advanced security and compliance (SOC 2, GDPR)
  - [ ] White-labeling and custom branding
  - [ ] Enterprise SSO integration
  - [ ] Advanced analytics and reporting

## 🤝 Contributing

We welcome contributions! Here's how to get started:

### **Development Contribution**

1. **Fork the repository**
2. **Clone your fork**:
   ```bash
   git clone https://github.com/YOUR-USERNAME/queryhub.git
   cd queryhub
   ```
3. **Set up development environment**:
   ```bash
   make local-setup
   make local-start
   ```
4. **Create your feature branch**:
   ```bash
   git checkout -b feature/amazing-feature
   ```
5. **Make your changes** and test them:
   ```bash
   make test
   ```
6. **Commit your changes**:
   ```bash
   git commit -m 'Add amazing feature'
   ```
7. **Push to your branch**:
   ```bash
   git push origin feature/amazing-feature
   ```
8. **Open a Pull Request**

### **Bug Reports & Feature Requests**

- **Bug Reports**: [Create an issue](https://github.com/your-username/queryhub/issues/new?template=bug_report.md)
- **Feature Requests**: [Create an issue](https://github.com/your-username/queryhub/issues/new?template=feature_request.md)
- **Questions**: [Start a discussion](https://github.com/your-username/queryhub/discussions)

### **Code Style & Standards**

- **Backend**: Follow PEP 8, use Black for formatting
- **Frontend**: Use Prettier, follow TypeScript best practices
- **Commits**: Use conventional commit messages
- **Documentation**: Update README for any user-facing changes

## 📊 Performance & Monitoring

### **Local Performance Monitoring**

```bash
# Check service status
make local-status

# Monitor logs in real-time
make local-logs

# Database performance
psql postgresql://queryhub:queryhub@localhost:5432/queryhub
# Run: \dt to see tables, EXPLAIN ANALYZE SELECT...
```

### **Production Monitoring**

- **Railway**: Built-in metrics and logging dashboard
- **Vercel**: Analytics and performance insights
- **Database**: PostgreSQL slow query logs
- **Frontend**: Next.js built-in performance monitoring

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links & Resources

- **🌐 Live Demo**: [https://query-hub-frontend.vercel.app](https://query-hub-frontend.vercel.app)
- **📚 API Documentation**: [https://query-hub-backend-production.up.railway.app/docs](https://query-hub-backend-production.up.railway.app/docs)  
- **🐛 Issues**: [GitHub Issues](https://github.com/your-username/queryhub/issues)
- **💬 Discussions**: [GitHub Discussions](https://github.com/your-username/queryhub/discussions)
- **📖 Documentation**: [Full Documentation](https://query-hub-frontend.vercel.app/docs)

### **Community & Support**

- **Discord**: [Join our community](https://discord.gg/queryhub)
- **Twitter**: [@QueryHub](https://twitter.com/queryhub)
- **Email**: support@queryhub.com

---

## 🎯 Quick Reference

### **Most Common Commands**
```bash
# Development
make local-start    # Start everything locally
make local-stop     # Stop everything

# Deployment
vercel --prod      # Deploy frontend
railway up         # Deploy backend

# Database
make migrate       # Apply migrations
make db           # Database shell
```

### **Important URLs**
- **Local Frontend**: http://localhost:3000
- **Local Backend**: http://localhost:8000
- **API Docs**: http://localhost:8000/docs
- **Test Form**: http://localhost:3000/dashboard/test-form

---

**Built with ❤️ for businesses worldwide**

*QueryHub - The AI that actually understands your business*

**🚀 Ready to get started?** Run `make local-setup` and you'll be up and running in minutes!
