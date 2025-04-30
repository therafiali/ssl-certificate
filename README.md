# SSL Certificate Management Guide

This guide provides instructions for managing SSL certificates on the server.

## Checking Certificate Status

To check the status of all SSL certificates:
```bash
sudo certbot certificates
```

This will show:
- Certificate names
- Serial numbers
- Domains covered
- Expiry dates
- Certificate and private key paths

## Certificate Renewal

### Automatic Renewal
Let's Encrypt certificates auto-renew when they're within 30 days of expiration. The renewal process is handled by a cron job.

### Manual Renewal
To manually renew a specific certificate:
```bash
sudo certbot renew --cert-name domain.com
```

To renew all certificates:
```bash
sudo certbot renew
```

## Certificate Locations

Certificates are stored in:
- Live certificates: `/etc/letsencrypt/live/`
- Configuration files: `/etc/letsencrypt/renewal/`
- Archived certificates: `/etc/letsencrypt/archive/`

## Nginx Configuration

### Viewing Current Configuration
```bash
sudo nginx -T
# or
cat /etc/nginx/sites-enabled/*
```

### Updating Certificate Paths
If a new certificate is generated (e.g., with -0001 suffix), update the Nginx configuration:
```bash
sudo sed -i 's|/etc/letsencrypt/live/domain.com/|/etc/letsencrypt/live/domain.com-0001/|g' /etc/nginx/sites-enabled/domain.com
```

### Testing and Reloading Nginx
After making changes:
```bash
sudo nginx -t              # Test configuration
sudo systemctl reload nginx # Apply changes
```

## Troubleshooting

### Common Issues

1. **Certificate Expired**
   - Check certificate status: `sudo certbot certificates`
   - Renew certificate: `sudo certbot renew --cert-name domain.com`

2. **Renewal Failed**
   - Check logs: `sudo tail -f /var/log/letsencrypt/letsencrypt.log`
   - Ensure domain points to server
   - Check firewall settings
   - Verify Nginx configuration

3. **Wrong Certificate Path**
   - Update Nginx configuration with correct path
   - Reload Nginx

### Useful Commands

- Check certificate expiry: `openssl x509 -in /etc/letsencrypt/live/domain.com/fullchain.pem -noout -dates`
- View certificate details: `openssl x509 -in /etc/letsencrypt/live/domain.com/fullchain.pem -text -noout`
- Test SSL connection: `curl -vI https://domain.com`

## Best Practices

1. **Regular Monitoring**
   - Check certificate status monthly
   - Monitor expiration dates
   - Review renewal logs

2. **Backup**
   - Keep backups of certificate configurations
   - Document any manual changes

3. **Security**
   - Keep private keys secure
   - Use appropriate file permissions
   - Regularly update SSL protocols and ciphers

## Quick Reference

### Emergency Renewal
```bash
# 1. Check certificate status
sudo certbot certificates

# 2. Renew specific certificate
sudo certbot renew --cert-name domain.com

# 3. Update Nginx if needed
sudo sed -i 's|old-path|new-path|g' /etc/nginx/sites-enabled/domain.com

# 4. Test and reload Nginx
sudo nginx -t
sudo systemctl reload nginx
```

### Verification
```bash
# Check certificate status
sudo certbot certificates

# Test SSL connection
curl -vI https://domain.com
```

Remember to replace `domain.com` with your actual domain name in all commands. 


For example:

sudo certbot renew --cert-name rafi.com
