# AirBnB Clone – Phase 3: RESTful API

![Python](https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?logo=flask&logoColor=white)
![Swagger](https://img.shields.io/badge/Swagger-flasgger-85EA2D?logo=swagger&logoColor=black)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-D71F00?logo=sqlalchemy&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?logo=mysql&logoColor=white)

Third phase of the ALX/Holberton **AirBnB clone**: a **RESTful JSON API** built with Flask on top of the storage engines from the earlier phases. Every resource (states, cities, amenities, users, places, reviews) supports full CRUD, places can be searched by state, city and amenity, and the whole API is documented with Swagger through `flasgger`.

Project series: [v1 – console](https://github.com/Blueskale/AirBnB_clone) · [v2 – MySQL and Flask](https://github.com/Blueskale/AirBnB_clone_v2) · **v3 – REST API** · [v4 – dynamic web](https://github.com/Blueskale/AirBnB_clone_v4)

## Highlights

- Flask application with a versioned blueprint (`/api/v1`), CORS enabled and JSON `404` handling
- Works with either storage engine: JSON file storage by default or MySQL via SQLAlchemy when `HBNB_TYPE_STORAGE=db`
- `storage.get()` and `storage.count()` helpers added to both engines for the API
- Swagger UI generated from YAML specs in `api/v1/views/documentation/`
- The console, models and tests from the earlier phases are included

## Getting started

```bash
git clone https://github.com/Blueskale/AirBnB_clone_v3.git
cd AirBnB_clone_v3
python3 -m venv venv && source venv/bin/activate
pip install Flask flask-cors flasgger SQLAlchemy mysqlclient
```

### Run with file storage

```bash
HBNB_API_HOST=0.0.0.0 HBNB_API_PORT=5000 python3 -m api.v1.app
```

### Run with MySQL

```bash
cat setup_mysql_dev.sql | mysql -uroot -p
HBNB_MYSQL_USER=hbnb_dev HBNB_MYSQL_PWD=hbnb_dev_pwd HBNB_MYSQL_HOST=localhost \
HBNB_MYSQL_DB=hbnb_dev_db HBNB_TYPE_STORAGE=db \
HBNB_API_HOST=0.0.0.0 HBNB_API_PORT=5000 python3 -m api.v1.app
```

Swagger UI is served at `http://0.0.0.0:5000/apidocs/`.

### Quick check

```bash
curl -s http://0.0.0.0:5000/api/v1/status
# {"status": "OK"}

curl -s http://0.0.0.0:5000/api/v1/stats
# {"amenities": 0, "cities": 0, "places": 0, "reviews": 0, "states": 0, "users": 0}

curl -s -X POST -H "Content-Type: application/json" -d '{"name": "Delta"}' \
     http://0.0.0.0:5000/api/v1/states
# {"__class__": "State", "created_at": "...", "id": "...", "name": "Delta", "updated_at": "..."}
```

## Endpoints

All routes are prefixed with `/api/v1`.

| Resource | Endpoints |
|---|---|
| Health | `GET /status`, `GET /stats` |
| States | `GET /states`, `POST /states`, `GET /states/<id>`, `PUT /states/<id>`, `DELETE /states/<id>` |
| Cities | `GET /states/<state_id>/cities`, `POST /states/<state_id>/cities`, `GET /cities/<id>`, `PUT /cities/<id>`, `DELETE /cities/<id>` |
| Amenities | `GET /amenities`, `POST /amenities`, `GET /amenities/<id>`, `PUT /amenities/<id>`, `DELETE /amenities/<id>` |
| Users | `GET /users`, `POST /users`, `GET /users/<id>`, `PUT /users/<id>`, `DELETE /users/<id>` |
| Places | `GET /cities/<city_id>/places`, `POST /cities/<city_id>/places`, `GET /places/<id>`, `PUT /places/<id>`, `DELETE /places/<id>` |
| Place search | `POST /places_search` with optional `states`, `cities` and `amenities` id lists |
| Reviews | `GET /places/<place_id>/reviews`, `POST /places/<place_id>/reviews`, `GET /reviews/<id>`, `PUT /reviews/<id>`, `DELETE /reviews/<id>` |
| Place amenities | `GET /places/<place_id>/amenities`, `POST /places/<place_id>/amenities/<amenity_id>`, `DELETE /places/<place_id>/amenities/<amenity_id>` |

Requests with a body must send JSON (`Content-Type: application/json`); invalid JSON or missing required fields return `400`, unknown ids return `404`.

## Environment variables

| Variable | Description |
|---|---|
| `HBNB_API_HOST`, `HBNB_API_PORT` | Bind address and port (default `0.0.0.0:5000`) |
| `HBNB_TYPE_STORAGE` | `db` for MySQL, otherwise JSON file storage |
| `HBNB_MYSQL_USER`, `HBNB_MYSQL_PWD`, `HBNB_MYSQL_HOST`, `HBNB_MYSQL_DB` | MySQL connection details |
| `HBNB_ENV` | `test` drops all tables on start |

## Running the tests

```bash
python3 -m unittest discover tests
```

The suites include PEP 8 checks and need the `pep8` package (`pip install pep8`).

## Project structure

```
AirBnB_clone_v3/
├── api/v1/
│   ├── app.py                       # Flask app, CORS, Swagger, error handlers
│   └── views/                       # one module per resource + Swagger YAML docs
├── console.py                       # command interpreter from phase 1
├── models/                          # data models and the two storage engines
├── web_flask/, web_static/          # web pages from phase 2
├── tests/                           # unittest suites
└── setup_mysql_dev.sql, setup_mysql_test.sql
```

## Author

Princewill Mmadu – [GitHub](https://github.com/Blueskale)
