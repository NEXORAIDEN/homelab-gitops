# cert-manager AWS Migration

Current

SelfSigned
↓
cert-manager
↓
TLS Secret

---

Future Option 1

Let's Encrypt
↓
cert-manager
↓
TLS Secret

---

Future Option 2

AWS ACM

↓

ALB

↓

TLS

---

# Migration Mapping

| Current | Future |
|----------|---------|
| SelfSigned | Let's Encrypt |
| SelfSigned | ACM |
| Local DNS | Route53 |
| K3s | EKS |

---

# Future Enhancements

- Wildcard Certificates
- Automatic DNS Validation
- ACM Integration
- Multi-Domain Support
