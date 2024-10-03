# TLS Key and Certificate Request Generation with Terraform

This Terraform configuration generates a TLS private key and Certificate Signing Request (CSR) for `flexit.com`.

## main.tf

```hcl
resource "local_file" "key_data" {
        filename       = "/tmp/.pki/private_key.pem"
        content = tls_private_key.private_key.private_key_pem
        file_permission =  "0400"
}
resource "tls_private_key" "private_key" {
  algorithm   = "RSA"
  dsa_bits  = 2048
  ecdsa_curve = "P384"
}
resource "tls_cert_request" "csr" {
  private_key_pem = file("/tmp/.pki/private_key.pem")
  depends_on = [ local_file.key_data ]

  subject {
    common_name  = "flexit.com"
    organization = "FlexIT Consulting Services"
  }
}
```
