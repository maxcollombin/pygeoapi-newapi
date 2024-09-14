# Adding a new API to pygeoapi

Start the container with the following command:

```bash
docker compose up -d
```

The new default path is available at `http://localhost:5000/my-function` or via the curl command:

```bash
curl -X 'GET' \
  'http://localhost:5000/my-function' \
  -H 'accept: application/geo+json'
```
