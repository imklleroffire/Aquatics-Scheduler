# Streamlit Deployment Guide

## Firebase credentials (Streamlit Cloud)

Configure Firebase via **Streamlit secrets** — never commit real keys.

1. Open your app on Streamlit Cloud → **Settings** → **Secrets**.
2. Add a service-account block like this (replace every placeholder):

```toml
[firebase]
type = "service_account"
project_id = "your-firebase-project-id"
private_key_id = "YOUR_PRIVATE_KEY_ID"
private_key = "-----BEGIN PRIVATE KEY-----\nYOUR_PRIVATE_KEY_MATERIAL\n-----END PRIVATE KEY-----\n"
client_email = "your-service-account@your-project.iam.gserviceaccount.com"
client_id = "YOUR_CLIENT_ID"
auth_uri = "https://accounts.google.com/o/oauth2/auth"
token_uri = "https://oauth2.googleapis.com/token"
auth_provider_x509_cert_url = "https://www.googleapis.com/oauth2/v1/certs"
client_x509_cert_url = "https://www.googleapis.com/robot/v1/metadata/x509/your-service-account%40your-project.iam.gserviceaccount.com"
universe_domain = "googleapis.com"
```

Download a fresh key from Firebase Console → Project settings → Service accounts if you need a new JSON file.

## Deploy

1. Push the repo to GitHub.
2. Connect the repo in Streamlit Cloud and deploy `app.py`.
3. Confirm secrets are set, then smoke-test sign-in.

## Local development

Place `serviceAccountKey.json` in the project root (gitignored). The app prefers Streamlit secrets when present, then falls back to the local file.

Optional:

```bash
export OPENAI_API_KEY=your_key_here
```

## Troubleshooting

### "Invalid database URL: None"
- Check Firebase config / secrets include a valid Realtime Database URL.

### "Invalid JWT Signature"
- Often clock skew or a bad/rotated service account key — generate a new key and update secrets.

### "serviceAccountKey.json not found"
- Local: add the file at the repo root.
- Cloud: configure the `[firebase]` secrets block above.

## Security

- Never commit `serviceAccountKey.json`, private keys, or populated secrets files.
- If a private key was exposed in git history, **rotate** the service account key in Google Cloud and update Streamlit secrets.
