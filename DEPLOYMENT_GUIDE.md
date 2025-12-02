# 🚀 Deployment Guide - UltraLearn AI

This guide explains how to deploy your UltraLearn AI app as a standalone website with a custom domain.

## 📋 Current Situation

Your app is built with:
- React 19 + TypeScript
- Vite build system
- Creao platform SDK for authentication and file uploads

## 🎯 Deployment Options

### **Option 1: Deploy As-Is (Keep Creao Backend)**

Deploy the frontend to a custom domain while keeping Creao authentication and file storage.

**Pros:**
- File uploads work immediately (uses Creao's S3 storage)
- AI features work (uses Creao's AI API)
- No backend infrastructure needed

**Cons:**
- Users must have Creao accounts to upload files
- Still depends on Creao platform

**Steps:**

1. **Build the production files:**
   ```bash
   npm run build
   ```

2. **Deploy to a hosting platform:**

   **Vercel (Recommended - Easiest):**
   ```bash
   npm install -g vercel
   vercel --prod
   ```

   **Netlify:**
   - Go to https://app.netlify.com/drop
   - Drag the `dist/` folder

   **Cloudflare Pages:**
   - Go to https://dash.cloudflare.com/
   - Create new Pages project
   - Upload `dist/` folder

   **GitHub Pages:**
   ```bash
   npm install -g gh-pages
   gh-pages -d dist
   ```

3. **Add custom domain:**
   - In your hosting platform dashboard, go to Domain settings
   - Add your custom domain (e.g., `ultralearn.ai`)
   - Update DNS records at your domain registrar:
     ```
     Type: CNAME
     Name: @
     Value: [provided by hosting platform]
     ```

4. **Configure environment:**
   - Set `TENANT_ID` if needed (optional)
   - Ensure CORS is configured for your domain

---

### **Option 2: Fully Independent (No Authentication)**

Remove Creao dependencies and use browser-based file processing.

**Pros:**
- Anyone can use without login
- Fully standalone
- No backend costs

**Cons:**
- File uploads stay in browser (not persisted to cloud)
- Limited AI features (would need to integrate OpenAI/Anthropic directly)
- More complex to implement

**Required Changes:**
1. Replace file upload with local file reading
2. Replace AI chat with direct OpenAI/Anthropic API calls
3. Add API key management (user provides their own API key)
4. Use localStorage/IndexedDB for document storage

---

## 🌟 Quick Start: Deploy Right Now

**Fastest way to get online (Option 1):**

```bash
# 1. Build production files
npm run build

# 2. Deploy to Vercel
npx vercel --prod

# 3. Follow prompts to set up custom domain
```

**Netlify (No CLI needed):**
1. Run `npm run build`
2. Go to https://app.netlify.com/drop
3. Drag `dist/` folder
4. Click "Domain settings" → "Add custom domain"

---

## 🔧 Configuration for Deployment

### Update `vite.config.js` for production:

The current config already handles production builds correctly. No changes needed!

### Environment Variables:

Create `.env.production` if you need to override settings:
```env
VITE_API_URL=https://api.creao.ai
TENANT_ID=your-tenant-id
```

---

## 📱 Domain Setup Examples

### Vercel:
1. Deploy with `vercel --prod`
2. Go to project settings → Domains
3. Add `www.yourdomain.com` and `yourdomain.com`
4. Update DNS at your registrar:
   ```
   Type: CNAME
   Name: www
   Value: cname.vercel-dns.com

   Type: A
   Name: @
   Value: 76.76.21.21
   ```

### Netlify:
1. Deploy via web UI or CLI
2. Go to Domain management
3. Add custom domain
4. Update DNS:
   ```
   Type: CNAME
   Name: www
   Value: your-site.netlify.app

   Type: A
   Name: @
   Value: 75.2.60.5
   ```

### Cloudflare Pages:
1. DNS automatically configured if domain is on Cloudflare
2. Just add domain in Pages settings

---

## ⚠️ Important Notes

### File Upload Limitations:

Currently, file uploads **require Creao authentication**. For public access:

1. **Keep as-is**: Users must sign up at creao.ai
2. **Make standalone**: Implement Option 2 above (significant refactoring)
3. **Hybrid**: Make most features public, upload only for logged-in users

### CORS Configuration:

If deploying to custom domain, ensure Creao backend allows CORS from your domain. Contact Creao support or update settings.

---

## 🎓 Recommended Approach

**For fastest deployment with working features:**

1. Use **Vercel** (free tier, automatic HTTPS, best performance)
2. Run `npm run build && npx vercel --prod`
3. Add your custom domain in Vercel dashboard
4. Users can access at your domain and sign in with Creao accounts for uploads

**For fully public no-login app:**

1. Let me know and I can refactor the app to work without authentication
2. This requires replacing file uploads with browser-based file reading
3. And adding direct OpenAI API integration (users provide API keys)

---

## 📊 Comparison

| Feature | Option 1 (Creao Backend) | Option 2 (Standalone) |
|---------|-------------------------|----------------------|
| Deployment Time | 5 minutes | 2-3 hours of development |
| User Login Required | Yes (Creao) | No |
| File Storage | Cloud (Creao S3) | Browser only |
| AI Features | ✅ Full | Requires API key |
| Cost | Free (Creao tier) | API costs |
| Maintenance | Low | Medium |

---

## 🚀 Next Steps

Choose your option:

**Want to deploy now with Creao backend?**
```bash
npm run build
npx vercel --prod
```

**Want fully standalone app?**
Let me know and I'll refactor the code to remove authentication requirements and work completely in the browser.
