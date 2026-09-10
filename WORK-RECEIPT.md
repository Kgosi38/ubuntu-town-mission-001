# UBUNTU TOWN WORK RECEIPT

Mission: UT-TECH-001
Builder: Kgosiemang Mogogole
Title: Mobile Builder Launch

## Objective
Deploy a public website entirely from my Android phone.

## Environment
Android: Android phone (Termux via F-Droid)
Termux: latest (F-Droid build)
Git: 2.55.0
Node: v24.18.0
Python: 3.14.6

## What I learned
I learned how to operate a terminal from scratch — navigating folders, creating and reading files, and installing a full development toolchain (Git, Node.js, npm, Python, SSH, curl) directly on my phone using Termux. I learned how SSH key authentication works: a public key that is safe to share with GitHub, and a private key that never leaves my device, and how the two are mathematically linked. I also learned the Git workflow end to end — init, add, commit, push — and how a static site connects to GitHub and then to a cloud host to become a live, public webpage.

## What I built
A static HTML/CSS webpage titled "From My Phone To The World," documenting the mission itself — with cards explaining Terminal, Git, GitHub, and Cloudflare, plus the build chain PHONE -> TERMUX -> GIT -> GITHUB -> CLOUDFLARE -> INTERNET. The page is responsive and styled entirely with plain CSS, no frameworks.

## Problems encountered
1. My first SSH connection to GitHub failed with "Permission denied (publickey)" even after adding my public key.
2. When first deploying to Cloudflare, I ended up in the Workers setup flow instead of Pages, which uses a different deploy command (npx wrangler deploy) meant for JavaScript projects rather than static HTML.

## How I diagnosed and solved them
For the SSH issue, I ran `ssh-keygen -lf ~/.ssh/id_ed25519.pub` locally and compared the fingerprint to the one shown next to my key on GitHub. They didn't match, which told me the key had been pasted incorrectly. I deleted the bad key from GitHub, copied the public key directly from the terminal output instead of retyping it, and re-added it. The fingerprints matched afterward, and `ssh -T git@github.com` succeeded.

For the Cloudflare issue, the deployment actually completed successfully even though it went through the Workers flow — Cloudflare auto-detected my static files and deployed them anyway, giving me a working public workers.dev URL. I verified this by opening the live link directly in my browser and confirming the page displayed correctly. I also confirmed continuous deployment worked by editing a line of text locally, committing, and pushing — the live site updated automatically without me touching the Cloudflare dashboard.

## AI I used
Claude (Anthropic)
## Where AI helped
Claude explained each command and concept in plain terms before I ran anything and helped me diagnose the SSH authentication failure by suggesting the fingerprint comparison. It also helped me understand what actually happened during the Cloudflare Workers deployment.

## What I verified myself
I personally ran every command in Termux, watched each output, and confirmed success at each stage — the terminal proof screenshots, the SSH fingerprint match, the GitHub repository containing my actual code, the live public URL loading correctly in my browser, and the live page updating after my second push.

## GitHub repository
https://github.com/Kgosi38/ubuntu-town-mission-001

## Live deployment
https://ubuntu-town-mission-001.mogogolek832.workers.dev

## Final commit SHA
(run: git log --oneline -1, and paste the hash here)

## What I can now do without help
I can now operate a terminal, install and inspect development tools, generate and troubleshoot SSH keys, create Git commits and push them to GitHub, and deploy a static site to Cloudflare — and I can explain why each step in that chain matters.
