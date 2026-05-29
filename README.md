## Running the Demo

```bash
docker compose up --build
```


## Testing for internal auth

```bash
# 1. get auth container name
docker ps --format "table {{.Names}}\t{{.Status}}"
# The output is shown below: 
# NAMES                     STATUS
# traefik-oursky-pre-test   Up 19 minutes
# oursky-auth-1             Up 23 minutes

# 2. container shell access
docker exec -it oursky-auth-1 //bin//sh

# 3. testing, 
# valid request
curl -v -H "x-pretest: valid-token" http://localhost:5000/auth
curl -v -H "x-pretest: valid-token" http://localhost:5000/health 
curl -v -H "x-pretest: wrong-token" http://localhost:5000/health # The token cannot be ignored, since Execing into a container

# invalid request
curl -v -H "x-pretest: wrong-token" http://localhost:5000/auth # The token cannot be ignored 
                                                               # since auth.py contains headers for further inspection of the token. 
```


## Testing

You can test manually:

```bash
# Valid request
curl -v -H "x-pretest: valid-token" http://localhost:8080/health
curl -v -H "x-pretest: valid-token" http://localhost:8080/404

# Invalid request
curl -v -H "x-pretest: wrong-token" http://localhost:8080/health

# Missing header
curl http://localhost:8080
``` 
