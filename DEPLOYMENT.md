# Google Ads MCP - Smithery Deployment Guide

## 🚀 Deploy naar Smithery Cloud

Deze guide helpt je om je Google Ads MCP server te deployen naar Smithery zonder credentials in je repository.

### Prerequisites

1. **Smithery Account**: Maak een account aan op [Smithery](https://smithery.ai)
2. **Google Cloud Project**: Zorg dat je Google Ads API hebt ingeschakeld
3. **OAuth Credentials**: Maak OAuth 2.0 credentials aan in Google Cloud Console

### Stap 1: Repository Voorbereiden

```bash
# Fork deze repository naar je eigen GitHub account
# Zorg dat de repository privé is voor security

# Clone je fork lokaal
git clone https://github.com/YOUR_USERNAME/mcp-google-ads.git
cd mcp-google-ads
```

### Stap 2: Smithery CLI Installeren

```bash
# Installeer Smithery CLI
npm install -g @smithery/cli

# Login met je Smithery account
smithery login
```

### Stap 3: Deploy naar Smithery

```bash
# Deploy vanuit je lokale repository
npx -y @smithery/cli@latest deploy .

# Of deploy direct vanuit GitHub
npx -y @smithery/cli@latest deploy github:YOUR_USERNAME/mcp-google-ads
```

### Stap 4: Credentials Toevoegen via Smithery Dashboard

1. Ga naar je [Smithery Dashboard](https://app.smithery.ai)
2. Klik op je gedeployde MCP server
3. Ga naar de "Secrets" tab
4. Voeg de volgende environment variables toe:

```env
GOOGLE_ADS_DEVELOPER_TOKEN=your_actual_developer_token
GOOGLE_ADS_LOGIN_CUSTOMER_ID=your_customer_id
GOOGLE_ADS_AUTH_TYPE=oauth
GOOGLE_ADS_CLIENT_ID=your_oauth_client_id
GOOGLE_ADS_CLIENT_SECRET=your_oauth_client_secret
```

### Stap 5: OAuth Setup in Google Cloud Console

1. Ga naar [Google Cloud Console > APIs & Services > OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent)
2. Stel de OAuth consent screen in:

   - **User Type**: External
   - **App Information**: Vul basis informatie in
   - **Scopes**: Voeg `https://www.googleapis.com/auth/adwords` toe
   - **Test Users**: Voeg je Google account toe
   - **Publishing Status**: Testing (voor development)

3. Ga naar [Credentials](https://console.cloud.google.com/apis/credentials)
4. Klik op je OAuth 2.0 Client ID
5. Voeg redirect URIs toe:
   - `http://localhost`
   - `http://localhost:8080`

### Stap 6: Gebruik je Gedeployde MCP

Na deployment krijg je een URL zoals: `https://your-mcp-name.mcp.run`

Gebruik deze URL in je MCP configuratie:

```json
{
  "mcpServers": {
    "googleAdsServer": {
      "command": "https://your-mcp-name.mcp.run",
      "env": {
        "GOOGLE_ADS_DEVELOPER_TOKEN": "your_token",
        "GOOGLE_ADS_LOGIN_CUSTOMER_ID": "your_customer_id"
      }
    }
  }
}
```

## 🔒 Security Checklist

- [ ] Repository is privé
- [ ] Geen credentials in code
- [ ] .gitignore bevat alle credential bestanden
- [ ] Credentials alleen via Smithery Secrets
- [ ] OAuth app is geconfigureerd voor testing

## 🛠 Troubleshooting

### "App not verified" Error

- Klik "Advanced" → "Go to [app name] (unsafe)"
- Voor productie: voltooi Google OAuth verificatie

### Connection Errors

- Controleer of alle environment variables zijn ingesteld
- Zorg dat OAuth redirect URIs correct zijn geconfigureerd

### Permission Errors

- Zorg dat je Google account toegang heeft tot de Google Ads accounts
- Controleer of de developer token geldig is

## 📚 Meer Informatie

- [Smithery Documentation](https://docs.smithery.ai)
- [Google Ads API Documentation](https://developers.google.com/google-ads/api/docs/start)
- [MCP Protocol](https://modelcontextprotocol.io/)
