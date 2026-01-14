# Security Policy

## Reporting Security Vulnerabilities

If you discover a security vulnerability in DevHub, please report it to the maintainers immediately. We take security issues seriously and will respond promptly to address them.

To report a security vulnerability:
- **Do not** open a public GitHub issue
- Contact the maintainers privately through GitHub's security advisory feature or via email

## Security Incident - Leaked Credentials Notice

### ⚠️ CRITICAL: Firebase Service Account Credentials Exposed

**Date of Incident Discovery:** January 2026

A Google Cloud service account JSON file containing sensitive credentials was previously present in this repository:
- **File Path:** `ios/devhub-65899-firebase-crashreporting-d5gaa-45a6ff6616.json`
- **Contains:** `private_key`, `client_email`, and other sensitive service account data

### Immediate Action Required

**If you are a maintainer or have access to the Google Cloud project `devhub-65899`:**

1. **Revoke the leaked service account immediately:**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Navigate to IAM & Admin → Service Accounts
   - Find the service account: `firebase-crashreporting-d5gaa@devhub-65899.iam.gserviceaccount.com`
   - Delete or disable this service account
   - Check audit logs for any unauthorized access

2. **Rotate all credentials:**
   - Create a new service account with minimal required permissions
   - Generate new private keys
   - Update all applications and services using the old credentials
   - **Never commit service account JSON files to version control**

3. **Review security practices:**
   - Use environment variables or secret management systems (e.g., Google Secret Manager, AWS Secrets Manager)
   - Add `*.json` files containing credentials to `.gitignore`
   - Implement pre-commit hooks to prevent accidental credential commits
   - Regularly audit repository for sensitive data

### Best Practices for Secret Management

To prevent similar incidents in the future:

- **Never commit secrets to Git:**
  - Service account keys
  - API keys
  - Passwords
  - OAuth tokens
  - Database connection strings

- **Use `.gitignore`:** Add patterns for sensitive files:
  ```
  **/google-services.json
  **/GoogleService-Info.plist
  **/*-firebase-*.json
  **/*.key
  **/*.pem
  .env
  .env.local
  ```

- **Use environment variables:** Store secrets in environment variables or secure secret management systems

- **Limit permissions:** Follow the principle of least privilege for service accounts

- **Regular audits:** Periodically scan repositories for accidentally committed secrets using tools like:
  - [git-secrets](https://github.com/awslabs/git-secrets)
  - [truffleHog](https://github.com/trufflesecurity/truffleHog)
  - [detect-secrets](https://github.com/Yelp/detect-secrets)

## Additional Security Measures

- Keep dependencies up to date to patch known vulnerabilities
- Use GitHub's Dependabot for automated dependency updates
- Enable GitHub's secret scanning for the repository
- Review and approve all pull requests carefully
- Use branch protection rules to prevent direct pushes to main branches
