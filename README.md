# Grablytic Official F-Droid Repository

Welcome to the official self-hosted F-Droid repository for **Grablytic**!

## Adding to F-Droid or Droid-ify

1. Open your F-Droid client (F-Droid, Droid-ify, or Neo Store).
2. Go to **Settings** → **Repositories** → **Add New Repository (+)**.
3. Enter the repository URL:
   ```
   https://onlyxianzo.github.io/grablytic-fdroid/repo
   ```
4. Save and refresh repositories.
5. Search for **Grablytic** and install. You will now receive automatic background updates whenever a new release is published!

## Signing the repository (one-time maintainer setup)

The index must be signed — modern `fdroidserver` refuses unsigned publishes,
so the workflow fails fast until this is done once:

```bash
# 1. Generate a dedicated repo key (keep the passwords — losing the keystore
#    means every client must re-add the repo):
keytool -genkeypair -alias grablytic-fdroid -keyalg RSA -keysize 4096 \
  -validity 10000 -keystore keystore.jks -storepass 'CHOOSE-LONG-RANDOM' \
  -keypass 'CHOOSE-LONG-RANDOM' -dname 'CN=Grablytic F-Droid'

# 2. Export the public cert fingerprint for config.yml (`repo_pubkey`):
keytool -list -rfc -keystore keystore.jks -alias grablytic-fdroid \
  | openssl x509 -noout -fingerprint -sha256

# 3. Store the keystore as a repo secret and re-run the workflow:
base64 -w0 keystore.jks   # save output as secret FDROID_KEYSTORE_BASE64
rm keystore.jks           # never commit the private key
```

Then put the SHA-256 fingerprint (hex, no colons) in `config.yml` as
`repo_pubkey`, and add matching `keystorepass`/`keypass` handling per the
[fdroidserver manual](https://f-droid.org/docs/Setup_an_F-Droid_App_Repo/).
Re-run `Update F-Droid Repository` afterwards.
