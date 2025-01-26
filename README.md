# 🚀 Low-Latency Video Streaming with H.264, FastAPI, WebCodecs, and Web Workers 🎥  

Welcome to the **Low-Latency Video Streaming** project! 🌐 This repository showcases a sleek, efficient, and ultra-smooth streaming solution, powered by **FastAPI** on the backend and **WebCodecs API** on the frontend. It's fast ⚡, efficient 📦, and perfect for those craving real-time streaming magic.  

---

## 🌟 Features  

✨ **Lightning-Fast Performance**: Leverages H.264 encoding and WebCodecs API to reduce delays dramatically.  
✨ **Backend Powered by FastAPI**: A robust and scalable backend to handle video streaming via WebSockets.  
✨ **Minimal Bandwidth Usage**: Achieves significant savings with delta frames as small as **0.01 KB**.  
✨ **Web Worker Magic**: Offloads decoding tasks to web workers to keep the UI buttery smooth.  
✨ **Scalable Design**: Works seamlessly across different network conditions.  

---

## 📽️ How It Works  

### Backend: FastAPI 🌐  
- **H.264 Encoding**: Frames are encoded using the H.264 codec, ensuring high compression and low latency.  
- **WebSocket Stream**: The backend streams the encoded frames (Key + Delta frames) via WebSockets for real-time delivery.  
- **Optimized Performance**: Includes `-tune zerolatency` to minimize buffering for live streaming scenarios.  

### Frontend: Vue.js + WebCodecs 🖥️  
- **Web Workers**: Decoding tasks are offloaded to a dedicated web worker, ensuring a smooth user experience.  
- **Canvas Rendering**: Decoded frames are rendered directly onto a `<canvas>` element for real-time updates.  

---

## 📊 Performance Comparison  

Let’s crunch the numbers 🥜:  

| Method              | Bandwidth @ 30 FPS 📡  |  
|---------------------|------------------------|  
| **Naive Approach**  | ~7.2 MBps (JPEG multipart) 😨 |  
| **This Project 🚀** | ~250 KB (Key) + 20 × 0.01 KB (Delta) ≈ **250.2 KBps** 🤯 |  

💡 *Result*: Our approach uses only **3.5%** of the bandwidth required by the naive method. That's like sipping a peanut smoothie instead of guzzling down a whole barrel! 🥜😄  

---

## 🥜 Why Use This?  

If you've read this far, it’s either because:  
1. You’re building the next big streaming app.  
2. You're avoiding something important at work.  
3. You really, *really* love peanuts. 🥜  

Whatever the reason, this project is here to make your life easier (and more entertaining).  

---

## 💬 Thanks for Reading!  

If this project helped you, or if you just enjoyed the peanut jokes, give this repo a ⭐! And remember, if you’re still reading, maybe it’s time to start building that streaming app you’ve been dreaming about? 😉  

Happy Streaming! 🎉  

---  
