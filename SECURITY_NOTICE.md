# 🚨 URGENT SECURITY NOTICE - Exposed Google Cloud Service Account Credentials

## Summary

A Google Cloud service account credentials file was found to be publicly exposed in this repository. This file contains sensitive information including a private key that could allow unauthorized access to your Google Cloud Platform project.

## Exposed Credentials

**File:** `ios/devhub-65899-firebase-crashreporting-d5gaa-45a6ff6616.json`

**Exposed Information:**
- **Private Key ID:** `45a6ff66163210c4ad160ae430fa0d92f75ea123`
- **Service Account Email:** `firebase-crashreporting-d5gaa@devhub-65899.iam.gserviceaccount.com`
- **Project:** `devhub-65899`

## IMMEDIATE ACTIONS REQUIRED

### 1. Revoke the Exposed Credentials (URGENT - Do this IMMEDIATELY)

The exposed service account credentials **MUST** be revoked or rotated in Google Cloud Console:

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Select project `devhub-65899`
3. Navigate to **IAM & Admin** → **Service Accounts**
4. Find the service account: `firebase-crashreporting-d5gaa@devhub-65899.iam.gserviceaccount.com`
5. **Option A - Delete the service account key:**
   - Click on the service account
   - Go to the **Keys** tab
   - Find the key with ID `45a6ff66163210c4ad160ae430fa0d92f75ea123`
   - Click the three dots menu → **Delete**
6. **Option B - Disable or delete the entire service account** (if no longer needed):
   - Select the service account
   - Click **Delete** or **Disable**
7. **Generate new credentials** if still needed for your application

### 2. Clean Git History

The credential file may still exist in git history even after deletion from the current branch. To completely remove it:

#### Option A: Using BFG Repo-Cleaner (Recommended)

```bash
# Install BFG Repo-Cleaner
# Download from: https://rtyley.github.io/bfg-repo-cleaner/

# Clone a fresh bare repository
git clone --mirror https://github.com/ThuraNyunt/devhub.git

# Remove the sensitive file from all commits
java -jar bfg.jar --delete-files "devhub-65899-firebase-crashreporting-d5gaa-45a6ff6616.json" devhub.git

# Navigate into the repository
cd devhub.git

# Clean up and force push
git reflog expire --expire=now --all && git gc --prune=now --aggressive
git push --force
```

#### Option B: Using git-filter-repo

```bash
# Install git-filter-repo
pip install git-filter-repo

# Clone the repository
git clone https://github.com/ThuraNyunt/devhub.git
cd devhub

# Remove the file from history
git filter-repo --path ios/devhub-65899-firebase-crashreporting-d5gaa-45a6ff6616.json --invert-paths

# Force push to all branches
git push origin --force --all
git push origin --force --tags
```

### 3. Review Access Logs

Check Google Cloud Platform audit logs to see if the exposed credentials were accessed:

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Navigate to **Logging** → **Logs Explorer**
3. Filter by the service account: `firebase-crashreporting-d5gaa@devhub-65899.iam.gserviceaccount.com`
4. Review activity for any suspicious access patterns
5. Check for unauthorized API calls or resource access

### 4. Enable Security Monitoring

1. Enable **Security Command Center** in Google Cloud Platform
2. Set up alerts for:
   - Unusual service account activity
   - Changes to IAM permissions
   - Access from unexpected locations
3. Consider enabling **VPC Service Controls** to limit where service accounts can be used

### 5. Update Application Configuration

If your application uses these credentials:

1. Update your application to use the new credentials
2. Test thoroughly in a staging environment first
3. Deploy to production
4. Monitor for any authentication errors

## Prevention for the Future

This pull request includes the following preventive measures:

1. ✅ Added Firebase and GCP credential patterns to `.gitignore`
2. ✅ Created this security notice document
3. ✅ Verified no credential files exist in the current working tree

### Best Practices Going Forward

1. **Never commit credentials to git:**
   - Use environment variables for sensitive configuration
   - Use secret management services (e.g., Google Secret Manager, AWS Secrets Manager)
   - Use `.gitignore` to exclude credential files

2. **Use secret scanning tools:**
   - Enable GitHub secret scanning (if using GitHub)
   - Use pre-commit hooks with tools like `git-secrets` or `detect-secrets`
   - Consider tools like TruffleHog for historical scans

3. **Follow the principle of least privilege:**
   - Create service accounts with minimal necessary permissions
   - Regularly audit and rotate credentials
   - Use different service accounts for different environments (dev, staging, prod)

4. **Monitor and audit:**
   - Enable audit logging for all service accounts
   - Set up alerts for unusual activity
   - Regular security reviews

## References

- [Google Cloud: Managing service account keys](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)
- [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
- [git-filter-repo](https://github.com/newren/git-filter-repo)
- [GitHub: Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)

## Contact

If you have questions about this security issue or need assistance with credential revocation, please contact the repository maintainers immediately.

---

**Status:** This file has been removed from the current branch. Maintainers must still revoke the exposed credentials and clean git history.
