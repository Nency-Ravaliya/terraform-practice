Now, let's use the private key created by this resource in another resource of type local file. Update the key.tf file with the requirements:


Resource Name: key_details

File Name: /root/key.txt

Content: use a reference expression to use the attribute called private_key_pem of the pvtkey resource.


When ready, run terraform init, plan and apply.

=========================

`key.tf`

```
resource "tls_private_key" "pvtkey"{
    algorithm = "RSA"
    rsa_bits = "4096"
}

resource "local_file" "key_details" {
    filename = "/root/key.txt"
    content = tls_private_key.pvtkey.private_key_pem
}

```


