# Duplicate Image Detector

A full-stack web application to detect duplicate images using perceptual hashing.

![Demo](https://via.placeholder.com/800x400?text=Duplicate+Image+Detector)

## 📁 Project Structure

```
duplicate-photo-finder/
├── frontend/          # Next.js + Tailwind CSS
│   ├── src/app/
│   │   ├── page.tsx   # Main upload page
│   │   └── layout.tsx # Root layout
│   ├── .env.local     # Environment variables
│   └── package.json
│
├── backend/           # FastAPI + OpenCV
│   ├── main.py        # API endpoints
│   ├── requirements.txt
│   └── Dockerfile
│
└── README.md
```

## 🚀 Quick Start (Local Development)

### 1. Start the Backend

```bash
cd backend

# Create virtual environment (optional but recommended)
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Mac/Linux

# Install dependencies
pip install -r requirements.txt

# Run the server
uvicorn main:app --reload --port 8000
```

Backend will be running at: `http://localhost:8000`

### 2. Start the Frontend

```bash
cd frontend

# Install dependencies
npm install

# Run development server
npm run dev
```

Frontend will be running at: `http://localhost:3000`

---

## 🌐 Deployment Instructions

### Step 1: Deploy Backend to Render (Free Tier)

1. **Push your code to GitHub**
   - Create a new repo and push the entire `duplicate-photo-finder` folder

2. **Go to [Render.com](https://render.com)** and sign up/login

3. **Create New Web Service**
   - Click "New" → "Web Service"
   - Connect your GitHub repo
   - Configure:
     - **Name**: `duplicate-detector-api`
     - **Root Directory**: `backend`
     - **Environment**: `Python 3`
     - **Build Command**: `pip install -r requirements.txt`
     - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`

4. **Click "Create Web Service"**
   - Wait for deployment (first time takes ~5 minutes)
   - Copy your URL: `https://your-app-name.onrender.com`

### Step 2: Deploy Frontend to Vercel (Free Tier)

1. **Go to [Vercel.com](https://vercel.com)** and sign up/login

2. **Import Project**
   - Click "Add New" → "Project"
   - Import your GitHub repo

3. **Configure**
   - **Root Directory**: `frontend`
   - **Framework Preset**: Next.js (auto-detected)
   - **Environment Variables**: Add this variable:
     ```
     NEXT_PUBLIC_API_URL = https://your-render-app.onrender.com
     ```
     (Replace with your actual Render URL from Step 1)

4. **Click "Deploy"**
   - Wait for deployment (~2 minutes)
   - Your app is live! 🎉

---

## ⚙️ Environment Variables

### Frontend (.env.local)
```
NEXT_PUBLIC_API_URL=http://localhost:8000  # Local
# NEXT_PUBLIC_API_URL=https://your-render-app.onrender.com  # Production
```

---

## 🔧 Technical Details

### How Duplicate Detection Works

The app uses **perceptual hashing (pHash)** algorithm:

1. **Resize** image to 8x8 pixels
2. **Convert** to grayscale
3. **Calculate** average brightness
4. **Generate** 64-bit hash (1 if pixel > average, else 0)
5. **Compare** hashes using Hamming distance

**Difference Score:**
- `0` = Exact duplicate
- `1-5` = Very similar (likely duplicate)
- `>5` = Different images

### API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Health check |
| `/detect-duplicates` | POST | Upload images & find duplicates |

### Limits
- Max file size: 5MB per image
- Supported formats: JPG, PNG, JPEG

---

## 🆓 Free Tier Limits

### Render Free Tier
- 750 hours/month
- Spins down after 15 min inactivity (cold starts)
- Good for personal projects

### Vercel Free Tier
- Unlimited static hosting
- 100GB bandwidth/month
- Perfect for Next.js apps

---

## 🛠️ Troubleshooting

### "Failed to fetch" error
- Make sure backend is running
- Check if CORS is enabled (it is by default in our code)
- Verify the API URL in `.env.local`

### Backend cold starts (Render)
- Free tier spins down after 15 min
- First request may take 30-60 seconds
- Add a loading message for users

### Images not processing
- Check file size (max 5MB)
- Ensure valid image format (JPG, PNG, JPEG)

---

## 📝 License

MIT License - Feel free to use and modify!
