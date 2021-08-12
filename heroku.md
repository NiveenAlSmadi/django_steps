# Heroku

- `docker-compose up -d --build`
- `heroku create maismoviesapp1`
- `https://maismoviesapp1.herokuapp.com/`
- in root create `heroku.yml`add:
```python
build:
    docker:
        web: Dockerfile
release:
    image: web
    command:
        - mkdir -p static
        - python manage.py collectstatic --noinput
run:
    web: gunicorn movies_project.wsgi
```
- `git init`
- `git add .`
- `git commit -am'herokuuuuu'`
- `heroku git:remote -a maismoviesapp1`
- `heroku stack:set container`
- ` git push heroku master`
- add Config Vars in heroku
- edit this in .env file and herokyu`ALLOWED_HOSTS=localhost,127.0.0.1,0.0.0.0,.maismoviesapp1.com,maismoviesapp1.herokuapp.com`
- also in heroku `Config Vars` -->ADD `DJANGO_ALLOWED_HOSTS`= `.maismoviesapp1.com,maismoviesapp1.herokuapp.com`
- `git add .`
- `git commit -am'herokuuuuu'`
- ` git push heroku master`
