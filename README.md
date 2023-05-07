# qdrant-apikey-docker
A docker-compose setup for adding api-key authentication to the open-source Qdrant container

This is a minimal `docker-compose` setup to put api-key auth on Qdrant.  The open source Qdrant Docker image does not have any authentication options.  This setup puts an **Nginx** reverse proxy in front of the open-source **Qdrant** container and adds api-key authentication to this proxy.

The setup and naming chosen for the api-key setup is the same as the **Qdrant Cloud** hosted setup so that the Qdrant API clients can be used out of the box.  See examples below.  

> Note that this setup does not include any SSL certificates.  This is insecure.  Either add SSL certificates using certbot or put behind a load-balancer such as Traefik which will manage the SSL certificates for you so it will work on HTTPS (port 443)

## Docker-compose

The `docker-compose.yml` file is a simple template that spins up the `qdrant` and the `nginx` containers.

This setup does not include any SSL certificates and will run on HTTP port 80 so can be easily put behind a load-balancer that manages the SSL.

## Nginx configuration

The configuration for `nginx` is in the `nginx-conf/nginx.conf` file.  

At the top of this file are to be valid API tokens.  These could be pulled out into environment variables if you wish or a separate file.  

The API key should be supplied in the request headers as `api-key: TOKEN`

This configuration also imposes rate limiting on the IP address to limite DDoS attacks.

## Example - connecting using the Qdrant client

To use the Python `qdrant-client` API, just use the `api-key` in the same way as in the cloud setup. 

```python
from qdrant_client import QdrantClient

client = QdrantClient(url="https://qdrant.example.com:443", api_key="TOKEN")

# Check by fetching collections
client.get_collections()
```

See [Qdrant documentation](https://qdrant.tech/documentation/cloud/cloud-quick-start) for more information

## Final note

This is a very minimal setup.  View as a starting point to add on features and security you need.
