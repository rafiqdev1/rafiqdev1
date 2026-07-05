
<!<div align="center">
  <h1>🔥 Vidly Pro</h1>
  <h3>Advanced Media Downloader CLI for Termux</h3>

  <p>
    <img src="https://img.shields.io/badge/Termux-Compatible-brightgreen?style=for-the-badge&logo=android" alt="Termux">
    <img src="https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python" alt="Python">
    <img src="https://img.shields.io/badge/yt--dlp-Powered-red?style=for-the-badge" alt="yt-dlp">
    <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License">
  </p>

  <img src="https://via.placeholder.com/800x250/FF4500/FFFFFF?text=Vidly+Pro+-+Termux+Media+Downloader" alt="Vidly Pro Banner" width="800">
</div>

---

**Vidly Pro** is a powerful, beautiful, and user-friendly Command Line Tool built specifically for **Termux** that lets you download high-quality videos and audio from YouTube, Instagram, Facebook, TikTok, and 1000+ other platforms.

### ✨ Key Features

- **Stunning CLI Interface** with animated banner and Rich styling
- **High-Quality Video Downloads** (Best, 1080p, 720p, 480p, etc.)
- **Audio Only (MP3)** with metadata & thumbnail embedding
- **Playlist & Channel Support**
- **Custom Download Folder** selection
- **Real-time Progress Bar** with ETA and speed
- **Fast & Reliable** using latest yt-dlp
- **Lightweight & Optimized** for Android/Termux
- **Multiple Language Support** (English + Urdu)

### 📸 Screenshots / Demo

*(Add your screenshots and GIFs here — highly recommended)*

### 🚀 Quick Installation

```bash
# Update Termux
pkg update && pkg upgrade -y

# Install dependencies
pkg install python git ffmpeg -y
pip install yt-dlp rich colorama

# Clone repository
git clone https://github.com/rafiqdev1/vidly-pro.git
cd vidly-pro

# Install requirements
pip install -r requirements.txt

# Make it easily accessible (optional but recommended)
chmod +x install.sh
bash install.sh
```
## Defensive Web Application Security Review

This repository includes a safe, defensive checklist for reviewing your own web application code for common security issues without exploit weaponization or attack steps.

- [Safe Web Application Security Review Checklist](WEB_APP_SECURITY_REVIEW_CHECKLIST.md)
