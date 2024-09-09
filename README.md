# Adding a new API to pygeoapi

0. Add code to `pygeoapi/api/<your_api>.py`
    0. API functions
        0. Process headers/query parameters
        0. `<your functionality>`
        0. Output headers, HTTP status code, content
    0. OpenAPI stubs
0. Hook new API functions to `pygeoapi/api/__init__.py`
0. Glue routes to new API functions
    0. pygeoapi/flask_app.py
0. Additional considerations:
    0. HTML / UI output (in pygeoapi/templates)
    0. Using configuration (in api.config object)
0. Example: [add-new-api](https://github.com/tomkralidis/pygeoapi/tree/add-new-api)

[Reference](https://docs.google.com/presentation/d/16IP1g_MNjNRsvHEDkbrdSpU2VR5uZi9l4a8sgFC2VcA/edit#slide=id.g2eb16ef2b4c_0_0)


## Exemple

Download the files from the branch `add-new-api` from Tom Kralidis repository:

```bash
wget -O local-config.yml https://raw.githubusercontent.com/tomkralidis/pygeoapi/add-new-api/local-config.yml
wget -O local-openapi.yml https://raw.githubusercontent.com/tomkralidis/pygeoapi/add-new-api/local-openapi.yml
wget -O __init__.py https://raw.githubusercontent.com/tomkralidis/pygeoapi/add-new-api/pygeoapi/api/__init__.py
wget -O newapi.py https://raw.githubusercontent.com/tomkralidis/pygeoapi/add-new-api/pygeoapi/api/newapi.py
wget -O flask_app.py https://raw.githubusercontent.com/tomkralidis/pygeoapi/add-new-api/pygeoapi/flask_app.py

```

Create a new docker-compose file: `docker-compose.yml`:

```bash
services:
  pygeoapi:
    image: geopython/pygeoapi:latest
    container_name: pygeoapi
    ports:
      - "5000:5000"
    volumes:
      - ./pygeoapi.config.yml:/pygeoapi/local.config.yml
      - ./local.openapi.yml:/pygeoapi/local.openapi.yml
      - ./pygeoapi/api/__init__.py:/pygeoapi/api/__init__.py
      - ./pygeoapi/api/newapi.py:/pygeoapi/api/newapi.py
      - ./pygeoapi/flask_app.py:/pygeoapi/flask_app.py
      - ./entrypoint.sh:/pygeoapi/entrypoint.sh
    entrypoint: ["/bin/sh", "/pygeoapi/entrypoint.sh"]
    environment:
      - PYGEOAPI_CONFIG=/pygeoapi/local.config.yml
      - PYGEOAPI_OPENAPI=/pygeoapi/local.openapi.yml
  ```

Create a new entrypoint file: `entrypoint.sh`:

```bash
#!/bin/sh

# Set environment variables
export PYGEOAPI_CONFIG=/pygeoapi/local.config.yml
export PYGEOAPI_OPENAPI=/pygeoapi/local.openapi.yml

# Start the pygeoapi service
exec pygeoapi serve
```

Make it executable:

```bash
chmod +x entrypoint.sh
```


Run the service:

```bash
docker compose up -d
```