## Steps to follow

1. 🚀 Start Minikube - ✅ done
2. 🔍 Check Minikube status - ✅ done
3. 📦 Install Helm chart - ✅ done
4. ⚙️ Create configuration for Prometheus - ⏳ in-progress
5. 🛠️ Create config for Thanos service:
    - Frontend
    - Query
    - Receiver
6. 🌐 Configure proxy settings
7. 📏 Setup Ruler and Alert Manager
8. 📡 Expose remote write Prometheus to Thanos Receiver
9. 🌐 Ingress Network will use NGINX as proxy

We need the frontend, receiver, ruler, and query to communicate via proxy.

;prompts;
- add or update emojis mention task status
- fix order of this installation
- fix the markdown file
- add emojis icons for done and in-progess   