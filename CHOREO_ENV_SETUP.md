# Choreo Environment Configuration

## Required Environment Variables

For your Ballerina application to work properly in Choreo, you need to configure the following environment variables in the Choreo console:

### MongoDB Configuration
```
mongoDbUrl = mongodb+srv://USERNAME:PASSWORD@cluster.mongodb.net/?retryWrites=true&w=majority&appName=yourapp&ssl=true
dbName = automeet
```

### Authentication & Security
```
JWT_SECRET = your-jwt-secret-key-here
jwtSigningKey = your-jwt-signing-key-here
```

### Google OAuth Configuration
```
googleClientId = your-google-client-id.apps.googleusercontent.com
googleClientSecret = your-google-client-secret
googleRedirectUri = https://YOUR_CHOREO_DOMAIN/api/auth/google/callback
googleCalendarRedirectUri = https://YOUR_CHOREO_DOMAIN/api/auth/google/calendar/callback
frontendBaseUrl = https://YOUR_FRONTEND_DOMAIN
```

### Other Configuration
```
hfApiKey = your-hugging-face-api-key
```

## How to Set Environment Variables in Choreo

1. Go to your Choreo project dashboard
2. Navigate to "Deploy" → "Set Up"
3. Click on "Configs & Secrets"
4. Add each environment variable one by one
5. Make sure to update the Google redirect URIs to match your actual Choreo deployment URL

## Important Notes

- Replace `YOUR_CHOREO_DOMAIN` with your actual Choreo deployment URL
- Replace `YOUR_FRONTEND_DOMAIN` with your actual frontend application URL
- **For actual deployment values**: Contact the project maintainer for the real API keys and secrets
- Replace `USERNAME:PASSWORD` in MongoDB URL with your actual MongoDB Atlas credentials
- Generate your own JWT secret keys using a secure random generator
- Ensure MongoDB Atlas allows connections from Choreo's IP ranges
- Test the MongoDB connection after deployment to verify SSL compatibility

## Getting Your Values

### MongoDB Atlas
1. Go to your MongoDB Atlas dashboard
2. Navigate to Database → Connect → Connect your application
3. Copy the connection string and replace username/password with your credentials

### Google OAuth
1. Go to Google Cloud Console
2. Navigate to APIs & Services → Credentials
3. Create OAuth 2.0 Client IDs for your application
4. Set the redirect URIs to match your Choreo deployment URLs

### JWT Secrets
Generate secure random strings (at least 256 characters) using:
```bash
openssl rand -hex 128
```

### Hugging Face API
1. Go to Hugging Face settings
2. Navigate to Access Tokens
3. Create a new token with appropriate permissions

## Recent Fixes Applied

1. **MongoDB SSL Configuration**: Simplified SSL settings for better Choreo compatibility
2. **Error Handling**: Fixed cookie validation to handle missing headers gracefully
3. **Request Validation**: Improved signup endpoint to handle missing password fields
4. **Logging**: Added better error logging for debugging MongoDB connection issues

## Troubleshooting

If you still see MongoDB connection issues:
1. Check MongoDB Atlas network access settings
2. Verify the connection string is correctly set as an environment variable
3. Check Choreo logs for specific SSL/TLS error details
4. Consider using MongoDB Atlas connection string troubleshooter
