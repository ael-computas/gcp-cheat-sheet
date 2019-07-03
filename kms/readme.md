# Usage
Use this to encrypt passwords/keys/etc so you can store that along with your code.

## Encrypting and decrypting a variable:
Just cut and paste this into a shell.. it should encrypt
"This is an example" into something unreadable
Then decrypt the unreadable string into "This is an example" again.

Keyring must be created in the GCP project: https://console.cloud.google.com/security/kms
You must create a key in that keyring. (same url)

```bash
original_message="This is an example"

encrypted_base64=$(echo -n "$original_message" | gcloud kms encrypt --ciphertext-file=- --plaintext-file=- --key=cryptokey --keyring=keyring --location=global | base64)
echo $encrypted_base64

decrypted_message=$(echo -n "$encrypted_base64" | base64 --decode | gcloud kms decrypt --ciphertext-file=- --plaintext-file=- --key=cryptokey --keyring=keyring --location=global)
echo $decrypted_message

if [ "$original_message" == "$decrypted_message" ]
then
   echo "Wow, its the same"
fi

```

### Recap, encrypting:
Copy the result here to the variable you want to use (example an env variable in app.yaml)
```bash
original_message="secret text, for example an api key."

encrypted_base64=$(echo -n "$original_message" | gcloud kms encrypt --ciphertext-file=- --plaintext-file=- --key=cryptokey --keyring=keyring --location=global | base64)
echo $encrypted_base64
```

### Recap, decrypting:
Copy the value you want to read (example an env variable in app.yaml)
```bash
encrypted_base64="COPY VALUE HERE"
decrypted_message=$(echo -n "$encrypted_base64" | base64 --decode | gcloud kms decrypt --ciphertext-file=- --plaintext-file=- --key=cryptokey --keyring=keyring --location=global)
echo $decrypted_message
```