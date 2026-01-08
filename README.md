Crispy Brittles — Hosting & Run Instructions

This document explains simple ways to host and run the Crispy Brittles static website (files in this folder) locally and on common servers.

1) Quick local testing (Python 3)

- Open a terminal in the project folder (e:\CB) and run:

```powershell
python -m http.server 8000
```

- Open http://localhost:8000 in your browser.

- To run in background on Windows PowerShell:

```powershell
Start-Process -NoNewWindow -FilePath python -ArgumentList '-m','http.server','8000'
```

2) Quick local testing (Node)

- If you have Node.js installed, install `serve` or `http-server`:

```bash
npm install -g serve
# or
npm install -g http-server
```

- Run:

```bash
serve -s . -l 8000
# or
http-server -p 8000
```

3) Using Visual Studio Code Live Server

- Install the Live Server extension.
- Open the folder in VS Code and click "Go Live" (serves on a local port).

4) Deploy to GitHub Pages (static site)

- Create a git repo and push the project, then enable GitHub Pages from the repository Settings (choose `main` branch or the `/docs` folder). Example commands:

```bash
git init
git add .
git commit -m "Initial site"
git remote add origin <your-git-url>
git push -u origin main
```

- In GitHub repo Settings → Pages, choose branch `main` (or `gh-pages`) and save. Wait a few minutes and visit the provided URL.

5) Deploy to Netlify (drag & drop)

- Go to https://app.netlify.com/sites/new and drag & drop the site folder (`e:\CB`) into the dashboard.
- Or use the Netlify CLI for continuous deploys.

6) Host on a Linux server with Nginx (production)

- Copy files to server (example using SCP):

```bash
scp -r . user@your-server:/home/user/site
```

- On the server, move to a web directory and give proper ownership:

```bash
sudo mv /home/user/site /var/www/crispybrittles
sudo chown -R www-data:www-data /var/www/crispybrittles
```

- Create an Nginx site file `/etc/nginx/sites-available/crispybrittles` with root pointing to `/var/www/crispybrittles` and enable it:

```nginx
server {
    listen 80;
    server_name example.com; # replace with your domain or server IP
    root /var/www/crispybrittles;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/crispybrittles /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

- (Optional) Configure UFW to allow HTTP/HTTPS:

```bash
sudo ufw allow 'Nginx Full'
```

7) Host on Windows Server (IIS) — basic outline

- Install IIS using Windows Features.
- Create a site pointing to the project folder and set bindings.
- Ensure static content role service is enabled and firewall rules allow HTTP(S).

8) Useful tips & notes

- All files are static (HTML, CSS, JS, images). No backend required.
- Ensure the logo file referenced in HTML (`crispybrittles.jpeg`) exists in the same folder as the HTML pages.
- If you change ports or use a reverse proxy, update any firewall or DNS rules accordingly.
- For production, serve over HTTPS (use Let's Encrypt for Nginx).

9) Troubleshooting

- Blank page or missing assets: check that asset paths are relative and that `index.html` and `styles.css` are in the same folder.
- Port already in use: choose another port (e.g., 8080) and restart.
- If using GitHub Pages and you see 404s, ensure `index.html` is at the repo root (or in `/docs`) and Pages settings point to the correct path.

If you want, I can also:
- Add a simple `start-server.bat` and `start-server.sh` to this folder for one-click starts.
- Generate an `nginx` config file and a sample `systemd` service for running a local static server.

