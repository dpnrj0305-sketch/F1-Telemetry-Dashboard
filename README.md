# FastAPI Backend - F1 Telemetry & AI Race Engineer Dashboard

## Overview

This is the FastAPI backend service for the F1 Telemetry Dashboard. It provides REST APIs for:
- Loading F1 telemetry data via FastF1 library
- Generating AI-powered race engineer analysis
- Managing database operations
- Serving CORS-enabled endpoints to the frontend

## Project Structure

```
backend/
├── app/
│   ├── main.py                 # FastAPI application initialization
│   ├── api/
│   │   ├── routes_telemetry.py # Telemetry data endpoints
│   │   └── routes_ai.py        # AI analysis endpoints
│   ├── services/
│   │   ├── fastf1_client.py    # FastF1 data loading
│   │   └── llm_engineer.py     # AI analysis generation
│   └── database/
│       ├── connection.py       # PostgreSQL connection setup
│       └── models.py           # SQLAlchemy ORM models
├── requirements.txt            # Python dependencies
├── .env.example               # Environment variables template
└── README.md                  # This file
```

## Setup Instructions

### 1. Create Virtual Environment

**Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure Environment

```bash
# Copy environment template
copy .env.example .env

# Edit .env with your configuration
```

**Required Configuration:**
```
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your_password
DB_NAME=f1_telemetry
OPENAI_API_KEY=your_key  (or GEMINI_API_KEY)
```

### 4. Initialize Database

```bash
# Create PostgreSQL database
createdb f1_telemetry

# Initialize schema
psql -U postgres -d f1_telemetry -f ../database/schema.sql
```

### 5. Run Backend Server

```bash
python -m uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Backend will be available at: `http://localhost:8000`

## API Documentation

Once running, visit:
- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

## Available Endpoints

### Health Check
- `GET /health` - Server status check

### Telemetry
- `GET /api/telemetry/session/{year}/{event}/{session_type}` - Load session data
- `GET /api/telemetry/driver/{driver_number}/lap/{lap_number}` - Get specific lap
- `GET /api/telemetry/compare?driver_a=X&driver_b=Y` - Compare drivers

### AI Analysis
- `POST /api/ai/analyze` - Generate race engineer analysis
- `POST /api/ai/compare-telemetry` - AI-powered telemetry comparison
- `GET /api/ai/analysis-history/{driver_number}` - Get analysis history
- `GET /api/ai/insights/{year}/{event}` - Get event insights

## Environment Variables

### Database (Required)
```
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your_password
DB_NAME=f1_telemetry
```

### LLM Configuration
Choose one LLM provider:

**OpenAI:**
```
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4
ACTIVE_LLM_PROVIDER=openai
```

**Google Gemini:**
```
GEMINI_API_KEY=...
GEMINI_MODEL=gemini-pro
ACTIVE_LLM_PROVIDER=gemini
```

### Optional
```
API_HOST=0.0.0.0
API_PORT=8000
DEBUG=False
LOG_LEVEL=INFO
FRONTEND_URL=http://localhost:3000
```

## Key Dependencies

- **FastAPI** - Modern async web framework
- **Uvicorn** - ASGI server
- **SQLAlchemy** - ORM for database operations
- **psycopg2** - PostgreSQL adapter
- **FastF1** - F1 telemetry data library
- **OpenAI / Google Generativeai** - LLM APIs
- **Pydantic** - Data validation

## Development

### Running Tests
```bash
pytest
```

### Code Quality
```bash
# Linting
pylint app/

# Formatting
black app/

# Type checking
mypy app/
```

### Hot Reload
The server runs with `--reload` flag, which auto-restarts on code changes.

## Common Issues

### "ModuleNotFoundError: No module named 'fastapi'"
- Activate virtual environment: `.\venv\Scripts\Activate.ps1`
- Reinstall dependencies: `pip install -r requirements.txt`

### "psycopg2.OperationalError: could not connect to server"
- Check PostgreSQL is running
- Verify DB credentials in `.env`
- Try creating database: `createdb f1_telemetry`

### "OpenAI API Error"
- Verify API key in `.env`
- Check your OpenAI account has credits
- Ensure key has correct permissions

### CORS Errors
- Frontend URL must be in CORS allowed origins
- Edit `main.py` to add your frontend URL if needed

## Performance Tips

1. **Enable Query Caching** (FastF1):
   ```python
   import fastf1
   fastf1.Cache.enable_cache('./cache')
   ```

2. **Use Async Database Queries**:
   - Already implemented in route handlers

3. **Database Indexing**:
   - Indexes created in `schema.sql`

## Security

⚠️ **Important**: This is an MVP. For production:
- [ ] Add user authentication (JWT)
- [ ] Validate all inputs
- [ ] Use environment secrets manager
- [ ] Add rate limiting
- [ ] Enable HTTPS
- [ ] Add API key authentication
- [ ] Implement request logging
- [ ] Add query sanitization

## Deployment

### Production Checklist
- [ ] Set `DEBUG=False`
- [ ] Use production ASGI server (Gunicorn + Uvicorn)
- [ ] PostgreSQL on separate server
- [ ] Configure environment variables securely
- [ ] Set up reverse proxy (Nginx)
- [ ] Enable CORS properly
- [ ] Add SSL/TLS certificates
- [ ] Set up monitoring/logging

### Docker
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY app/ app/
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0"]
```

## Future Enhancements

- [ ] WebSocket support for real-time data
- [ ] Caching layer (Redis)
- [ ] GraphQL API option
- [ ] Advanced telemetry filtering
- [ ] Batch analysis processing
- [ ] User authentication system
- [ ] Webhook support
- [ ] Rate limiting & throttling

## Support

For issues:
1. Check error logs
2. Review API documentation at `/docs`
3. Verify configuration in `.env`
4. Check backend logs in terminal

---

**Backend Service Ready!** 🚀
