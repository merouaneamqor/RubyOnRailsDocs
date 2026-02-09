### 1. **What are Rails credentials?**
A secure way to store encrypted secrets in:

```
config/credentials.yml.enc
```

Decrypted only using the `master.key` (or environment variable).

Used for:
- API keys  
- SMTP credentials  
- database passwords  
- encryption secrets  

---

### 2. **How do you edit credentials?**

```bash
rails credentials:edit
```

This opens the encrypted file in your editor.

Example content:

```yaml
aws:
  access_key_id: 123
  secret_access_key: 456
secret_key_base: abcdef
```

---

### 3. **Where is the master key stored?**
Default location:

```
config/master.key
```

Should **never** be committed to Git.

On production:
- Provided via `RAILS_MASTER_KEY` environment variable

---

### 4. **Difference between credentials and environment variables?**

| Category        | Best For |
|-----------------|----------|
| **Credentials** | app-wide secrets, structured configs |
| **ENV vars**    | deployment environment settings |

Credentials are encrypted — environment variables are not.

---

### 5. **How do you access credentials in Rails?**

```ruby
Rails.application.credentials.dig(:aws, :access_key_id)
```

Or shorthand:

```ruby
Rails.application.credentials.aws[:access_key_id]
```

---

### 6. **What are environment-specific credentials?**
Rails allows multiple encrypted credential files:

```
credentials/development.yml.enc
credentials/production.yml.enc
```

Edit using:

```bash
rails credentials:edit --environment production
```

Prevents leaking production secrets into development.

---

### 7. **Should API keys be stored in Git repo?**
YES — the **encrypted** file can be stored.  
NO — the **master.key** must never be stored in Git.

This ensures:
- version control  
- secure encryption  
- easy deployment  

---

### 8. **How does Rails secure credentials internally?**
Rails uses:
- AES-128-GCM encryption  
- secure key derivation  
- auto-rotation support  

---

### 9. **What should NOT be stored in credentials?**
- large binary files  
- per-machine configuration  
- secrets that change extremely often  

Use environment variables in those cases.

---

### 10. **How do you handle CI/CD environments?**
Set the master key as an environment variable:

```
RAILS_MASTER_KEY=your_key_here
```

CI can decrypt credentials during builds without exposing secrets.

---

