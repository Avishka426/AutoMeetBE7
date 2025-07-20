# Choreo Environment Configuration

## Required Environment Variables

For your Ballerina application to work properly in Choreo, you need to configure the following environment variables in the Choreo console:

### MongoDB Configuration
```
mongoDbUrl = mongodb+srv://USERNAME:PASSWORD@your-cluster.cb3avmr.mongodb.net/?retryWrites=true&w=majority&ssl=false&authSource=admin
dbName = automeet
```

**Critical for Choreo SSL Issues:**
Your logs show `SSLException: Received fatal alert: internal_error` on all MongoDB Atlas shards. This is a known issue with Choreo's Java environment. **Try these connection strings in order**:

**Option 1 (Recommended):** Force disable SSL
```
mongoDbUrl = mongodb+srv://USERNAME:PASSWORD@your-cluster.cb3avmr.mongodb.net/?retryWrites=true&w=majority&ssl=false&tls=false
```

**Option 2:** Use non-SRV connection (if Option 1 fails)
```
mongoDbUrl = mongodb://USERNAME:PASSWORD@ac-nppqaa3-shard-00-00.cb3avmr.mongodb.net:27017,ac-nppqaa3-shard-00-01.cb3avmr.mongodb.net:27017,ac-nppqaa3-shard-00-02.cb3avmr.mongodb.net:27017/?ssl=false&replicaSet=atlas-xxxxxx-shard-0&authSource=admin&retryWrites=true&w=majority
```

**Option 3:** Minimal connection string
```
mongoDbUrl = mongodb+srv://USERNAME:PASSWORD@your-cluster.cb3avmr.mongodb.net/automeet?retryWrites=true&w=majority
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
4. **For replica set connection (Option 2):** 
   - Click "View Details" on your cluster
   - Note the replica set name (usually `atlas-xxxxxx-shard-0`)
   - Use the individual shard addresses shown in your error logs

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

### MongoDB SSL Connection Errors
If you see errors like `SSLException: Received fatal alert: internal_error` in Choreo logs:

**Your Error Pattern:**
```
SSLException: Received fatal alert: internal_error
servers=[{address=ac-nppqaa3-shard-00-02.cb3avmr.mongodb.net:27017, type=UNKNOWN, state=CONNECTING}]
```

**Solution Steps:**
1. **First, try Option 1 connection string** with both SSL and TLS disabled:
   ```
   mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority&ssl=false&tls=false
   ```

2. **If that fails, use direct replica set connection** (Option 2):
   ```
   mongodb://username:password@ac-nppqaa3-shard-00-00.cb3avmr.mongodb.net:27017,ac-nppqaa3-shard-00-01.cb3avmr.mongodb.net:27017,ac-nppqaa3-shard-00-02.cb3avmr.mongodb.net:27017/?ssl=false&replicaSet=atlas-xxxxxx-shard-0&authSource=admin&retryWrites=true&w=majority
   ```

3. **Check MongoDB Atlas Settings**:
   - Go to Network Access in MongoDB Atlas
   - Add `0.0.0.0/0` to allow all IPs (for testing)
   - Ensure your cluster supports the driver version

4. **Verify Credentials**:
   - Double-check username and password in connection string
   - Ensure the database user has proper permissions

### General MongoDB Issues
If you still see MongoDB connection issues:
1. Check MongoDB Atlas network access settings
2. Verify the connection string is correctly set as an environment variable
3. Check Choreo logs for specific SSL/TLS error details
4. Consider using MongoDB Atlas connection string troubleshooter
5. Test the connection string locally first

### Quick Connection Test
To verify your connection string works:
1. **Local Test**: Try connecting with a MongoDB client tool using the same connection string
2. **Check Choreo Environment Variables**: Ensure the `mongoDbUrl` is correctly set in Choreo's "Configs & Secrets"
3. **Monitor Logs**: Look for the "MongoDB client initialized successfully" message in Choreo logs
4. **Connection Timeout**: If you see 30-second timeouts, it's usually an SSL/network issue

### Log Patterns to Watch For
- ✅ **Success**: `MongoDB client initialized successfully`
- ❌ **SSL Error**: `SSLException: Received fatal alert: internal_error`
- ❌ **Auth Error**: `Authentication failed`
- ❌ **Network Error**: `Timed out while waiting for a server`
