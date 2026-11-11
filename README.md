# Leave Planner

A single-page leave planner for South Australia: annual leave (AL) and long service leave (LSL), public holidays, the company shutdown, an optimiser, scenarios, export and print.

No build step, no backend, no dependencies. Everything is in `index.html`. Settings and plans are saved in the browser (localStorage); use **Backup / Restore** to move them between devices.

## Run it locally
Double-click `index.html`, or:

    python3 -m http.server 8000   # then open http://localhost:8000

## Publish on GitHub Pages
1. Create a new repository (private is fine on paid plans; Pages on free plans needs a public repo).
2. Add `index.html` and `README.md`:

       git init
       git add index.html README.md
       git commit -m "Leave planner"
       git branch -M main
       git remote add origin git@github.com:YOUR-USER/leave-planner.git
       git push -u origin main

3. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: main / (root) → Save.**
4. After a minute the app is live at `https://YOUR-USER.github.io/leave-planner/`.

Note: localStorage is per browser and per address, so each device keeps its own plans. Use Backup on one device and Restore on the other.

## Editing the data
- Public holidays: **Holidays** tab (add years, fix dates). Defaults live in `DEFAULT_HOLIDAYS` in `index.html`.
- Everything else is on the **Setup** tab.

## Tests
The date engine is in `<script id="engine">` and has no DOM dependencies. The **Assumptions & tests** tab runs 21 checks against the worked examples. To run them in Node:

    node -e "const fs=require('fs'),vm=require('vm');const h=fs.readFileSync('index.html','utf8');const c={};vm.createContext(c);vm.runInContext(h.match(/<script id=\"engine\">([\s\S]*?)<\/script>/)[1]+';this.r=runTests();',c);c.r.forEach(x=>console.log(x.pass?'PASS':'FAIL',x.name));"
