go-pki
======

Easy Public Key Infrastructure intends to provide most of the components needed
to manage a PKI, so you can either use the API in your automation, or use the
CLI.

# Prerequisite  

You must have Golang installed on your workstation.  

The CLI was tested using:  

```
# go version
go version go1.15.7
```

Ensure you have exported the gopath:

```
export GOPATH=/Users/$USER/go
export PATH=$GOPATH/bin:$PATH
```

# CLI

Current implementation of the CLI uses the local store and uses a structure
compatible with openssl, so you are not restrained.

```
# Get the CLI:
go get github.com/Fi1ipe5ant0s/go-pki/cmd/easypki


# You can also pass the following through arguments if you do not want to use
# env variables.
export PKI_ROOT=/tmp/pki
export PKI_ORGANIZATION="Acme Inc."
export PKI_ORGANIZATIONAL_UNIT=IT
export PKI_COUNTRY=US
export PKI_LOCALITY="Agloe"
export PKI_PROVINCE="New York"

mkdir $PKI_ROOT

# Create the root CA:
easypki create --filename root --ca "Acme Inc. Certificate Authority"

# In the following commands, ca-name corresponds to the filename containing
# the CA.

# Create a server certificate for blog.acme.com and www.acme.com:
easypki create --ca-name root --dns blog.acme.com --dns www.acme.com www.acme.com

# Create an intermediate CA:
easypki create --ca-name root --filename intermediate --intermediate "Acme Inc. - Internal CA"

# Create a wildcard certificate for internal use, signed by the intermediate ca:
easypki create --ca-name intermediate --dns "*.internal.acme.com" "*.internal.acme.com"

# Create a client certificate:
easypki create --ca-name intermediate --client --email bob@acme.com bob@acme.com

# Revoke the www certificate.
easypki revoke $PKI_ROOT/root/certs/www.acme.com.crt

# Generate a CRL expiring in 1 day (PEM Output on stdout):
easypki crl --ca-name root --expire 1
```
You will find the generated certificates in `$PKI_ROOT/ca_name/certs/` and
private keys in `$PKI_ROOT/ca_name/keys/`

For more info about available flags, checkout out the help `easypki -h`.

# Disclaimer

This Product was initially created by Google however it was dropped and no longer maintaied since 2017.
[Official Repository](https://github.com/google/easypki)  
