#  Sahih al-ʿIlm – Assistant intelligent des sciences religieuses (Hadith & Tafsīr)

**Sahih al-ʿIlm** signifie _« La science authentique »_ en arabe. Ce nom reflète l'engagement du projet à fournir un savoir fiable et vérifié à partir des sources reconnues de l’islam.

Ṭālib est un assistant RAG (Retrieval-Augmented Generation) en français, conçu pour répondre aux questions sur l'islam à partir des **hadiths authentiques (Ṣaḥīḥ al-Bukhārī)** et du **Tafsīr Ibn Kathīr**, le tout à travers un **moteur de recherche vectorielle unifié**.

---

##  Architecture du projet

 **Note : la construction de l'index vectoriel est effectuée séparément du projet backend. L'index est considéré comme fonctionnel et accessible depuis l'API.**

```
talib-assistant/
│
├── backend/                            #  Serveur FastAPI
│   ├── app/
│   │   ├── main.py                     # Entrée principale (API FastAPI)
│   │   ├── core.py                     # Logique RAG (embedding, query, réponse)
│   │   ├── query_router.py             # Détection et routage vers les sources
│   │   ├── pinecone_utils.py           # Fonctions d'interaction avec Pinecone
│   │   ├── hf_llm_api.py               # Appel au modèle de langage (Hugging Face/OpenAI)
│   │   ├── mongodb.py                  # Connexion MongoDB (hadiths + tafsir)
│   │   ├── models.py                   # Schémas de données (Pydantic)
│   │   └── auth.py                     # Authentification (JWT, Firebase)
│   ├── requirements.txt
│   ├── Dockerfile
│   └── .env.example
│
├── frontend/                           #  Interface React.js
│   ├── src/components/                 # Composants d'affichage (ChatBox, Bulle)
│   ├── src/pages/                      # Pages (Accueil, Chat)
│   ├── App.jsx, index.js, firebase.js
│
│  
│
├── infra/                              # Déploiement cloud
│   ├── render.yaml                     # Déploiement backend Render
│   ├── vercel.json                     # Déploiement frontend Vercel
│   └── README_DEPLOY.md
│
├── docs/                               #  Documentation fonctionnelle et technique
│   ├── architecture.md, prompts.md, userflow.md, api_endpoints.md
│
├── logs/                               #  Fichiers de log
│   ├── insert_log.json, query_log.json, failed_records.json
│
├── .gitignore
├── README.md
└── LICENSE
```

---

##  Interaction entre modules

| Module | Rôle | Interagit avec |
|--------|------|----------------|
| `query_engine.py` | Reçoit une question utilisateur et interroge l'index | `core.py` |
| `query_router.py` | Détecte si la question concerne les hadiths, le tafsir ou les deux | `core.py` |
| `hf_llm_api.py` | Génère des réponses fluides si besoin | `core.py` |
| `frontend/` | Affiche les messages, déclenche les requêtes API | `backend/app/main.py` |
| `mongodb.py` | Permet de retrouver les hadiths ou tafsirs originaux | `query_engine.py` |

---

##  Objectifs fonctionnels

- Poser une question libre en français
- Obtenir des réponses contextuelles et sourcées (verset ou hadith)
- Basées sur des sources reconnues : Ṣaḥīḥ al-Bukhārī et Tafsīr Ibn Kathīr
- Architecture prête pour être déployée sur Render / Vercel

---

##  Technologies utilisées

- Backend : **FastAPI**, **MongoDB**, **Pinecone**, **SentenceTransformers**
- Frontend : **React.js**, **Firebase Auth** (optionnel)
- Déploiement : **Render** (API), **Vercel** (interface)
- Embedding : `all-MiniLM-L6-v2` (modifiable)
- LLM  : HuggingFace Inference API ou OpenAI

---

## Auteur
**Nom du projet** : Sahih al-ʿIlm 
**Par** : Papa garba coulibaly