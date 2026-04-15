# F1 Telemetry & AI Race Engineer Dashboard

> An MVP dashboard for F1 telemetry visualization and AI-powered race engineer analysis

## 🌟 Features

- **Real-time Telemetry Visualization**: Interactive charts displaying speed, throttle, brake, RPM, and gear data
- **FastF1 Integration**: Load telemetry data from historical and current F1 seasons
- **AI Race Engineer Analysis**: ChatGPT/Gemini-powered driver comparison and insights
- **PostgreSQL Backend**: Persistent storage for sessions and analyses
- **Responsive UI**: Modern Tailwind CSS styling with Recharts visualizations

## 🏗️ Tech Stack

- **Frontend**: Next.js/React 18, Recharts, Tailwind CSS
- **Backend**: FastAPI (Python), SQLAlchemy ORM
- **Database**: PostgreSQL
- **Data Processing**: FastF1, Pandas, NumPy
- **AI Integration**: OpenAI GPT-4, Google Gemini

## 📁 Project Structure

```
f1-telemetry-dashboard/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── TelemetryChart.jsx
│   │   │   ├── AIDriverSummary.jsx
│   │   │   ├── SessionSelector.jsx
│   │   │   └── index.js
│   │   ├── services/
│   │   │   └── api.js
│   │   ├── App.jsx
│   │   └── index.js
│   ├── public/
│   ├── package.json
│   ├── .env.example
│   └── tailwind.config.js
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── api/
│   │   │   ├── routes_telemetry.py
│   │   │   └── routes_ai.py
│   │   ├── services/
│   │   │   ├── fastf1_client.py
│   │   │   └── llm_engineer.py
│   │   └── database/
│   │       ├── connection.py
│   │       └── models.py
│   ├── requirements.txt
│   ├── .env.example
│   └── README.md
├── database/
│   └── schema.sql
└── README.md
```

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- Node.js 18+
- PostgreSQL 12+
- Git

### Backend Setup

1. **Create Virtual Environment** (Windows/PowerShell):
   ```powershell
   cd backend
   python -m venv venv
   .\venv\Scripts\Activate.ps1
   ```

2. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure Database**:
   - Create PostgreSQL database: `createdb f1_telemetry`
   - Copy `.env.example` to `.env` and configure database credentials
   - Initialize schema: `psql -U postgres -d f1_telemetry -f ../database/schema.sql`

4. **Set API Keys** (in `.env`):
   ```
   OPENAI_API_KEY=your_key_here
   # or
   GEMINI_API_KEY=your_key_here
   ```

5. **Run Backend**:
   ```bash
   python -m uvicorn app.main:app --reload
   ```
   Backend runs at `http://localhost:8000`

### Frontend Setup

1. **Install Dependencies**:
   ```bash
   cd frontend
   npm install
   ```

2. **Configure Environment**:
   - Copy `.env.example` to `.env.local`
   - Ensure `REACT_APP_API_URL=http://localhost:8000/api`

3. **Run Development Server**:
   ```bash
   npm start
   ```
   Frontend runs at `http://localhost:3000`

## 📖 API Endpoints

### Telemetry Endpoints
- `GET /api/telemetry/session/{year}/{event}/{session_type}` - Get session telemetry
- `GET /api/telemetry/driver/{driver_number}/lap/{lap_number}` - Get specific lap data
- `GET /api/telemetry/compare?driver_a=X&driver_b=Y` - Compare two drivers

### AI Endpoints
- `POST /api/ai/analyze` - Generate race engineer analysis
- `POST /api/ai/compare-telemetry` - Compare telemetry using AI
- `GET /api/ai/analysis-history/{driver_number}` - Get analysis history
- `GET /api/ai/insights/{year}/{event}` - Get event insights

## 🎛️ Configuration

### Backend Environment Variables
See `backend/.env.example` for all available configuration options.

Key variables:
- `DB_*` - PostgreSQL connection settings
- `OPENAI_API_KEY` / `GEMINI_API_KEY` - LLM API credentials
- `ACTIVE_LLM_PROVIDER` - Choose between `openai` or `gemini`
- `DEBUG` - Enable debug logging

### Frontend Environment Variables
See `frontend/.env.example` for configuration.

## 🔧 Development

### Running Tests (Backend)
```bash
cd backend
pytest
```

### Code Quality
```bash
# Backend
pylint app/
black app/

# Frontend
npm run lint
npm run format
```

## 📊 Database Schema

### Tables
- `users` - Dashboard users
- `saved_sessions` - Saved F1 sessions
- `ai_analyses` - Generated AI analyses
- `telemetries` - Raw telemetry data storage
- `favorites` - User favorite analyses
- `comments` - Analysis annotations

## 🤖 AI Integration

The dashboard supports two LLM providers:

### OpenAI (Recommended)
1. Get API key from https://platform.openai.com/api-keys
2. Set `OPENAI_API_KEY` in `.env`
3. Set `ACTIVE_LLM_PROVIDER=openai`

### Google Gemini
1. Get API key from https://makersuite.google.com/app/apikey
2. Set `GEMINI_API_KEY` in `.env`
3. Set `ACTIVE_LLM_PROVIDER=gemini`

## 🐳 Docker Deployment (Future)

```bash
docker-compose up -d
```

## 📝 Common Issues

### Backend Connection Error
- Ensure PostgreSQL is running
- Verify database credentials in `.env`
- Check database exists: `psql -l`

### CORS Errors
- Verify frontend URL in `CORS_ORIGINS` in backend `.env`
- Ensure backend is running on port 8000

### Module Not Found (Python)
- Activate virtual environment
- Reinstall requirements: `pip install -r requirements.txt --upgrade`

### API Not Responding
- Check backend logs for errors
- Verify `REACT_APP_API_URL` in frontend `.env.local`

## 🚦 Roadmap

- [ ] User authentication & authorization
- [ ] Real-time telemetry streaming
- [ ] Advanced filtering & comparison tools
- [ ] Session sharing & collaboration
- [ ] Mobile-responsive design improvements
- [ ] Docker containerization
- [ ] CI/CD pipeline
- [ ] Performance optimization
- [ ] Interactive telemetry data export

## 📝 License

MIT License - See LICENSE file for details

## 🤝 Contributing

Contributions are welcome! Please follow these steps:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 💬 Support

For issues and questions:
- Open an GitHub Issue
- Check existing documentation
- Review API documentation at `/api/docs` (backend)

## 🙏 Acknowledgments

- FastF1 library for F1 telemetry data
- Recharts for visualization
- FastAPI for backend framework
- React & Next.js for frontend

---

**Happy Racing! 🏁**
