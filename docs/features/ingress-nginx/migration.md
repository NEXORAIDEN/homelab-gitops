# Ingress AWS Migration

Current

Raspberry Pi
↓
NodePort
↓
Ingress NGINX

Future

Route53
↓
AWS Load Balancer Controller
↓
ALB
↓
Ingress

---

# Migration Mapping

| Current | AWS |
|----------|------|
| NodePort | ALB |
| Local DNS | Route53 |
| Self-Signed TLS | ACM |
| K3s | EKS |

---

# Future Enhancements

- Web Application Firewall
- Rate Limiting
- Geo Restrictions
- DDoS Protection
- Multi-Region Routing
