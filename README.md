# MLOps First Pipeline

A beginner MLOps project that trains a Random Forest classifier on scikit-learn's Iris dataset, saves the model, and serves predictions through a FastAPI application. The app can also be packaged as a Docker image.

## Project structure

```text
.
|-- .gitignore
|-- README.md
`-- mlops-first-pipeline/
    |-- Dockerfile
    |-- main.py
    |-- train.py
    |-- requirements.txt
    |-- models/
    |   `-- iris_random_forest_model.joblib
    |-- tests/
    |   `-- test_api.py
    `-- .github/
        `-- workflows/
            `-- ci.yml
```

The root `kiet-cse` folder is a local Python virtual environment and is excluded from Git.

## Run locally (Windows PowerShell)

From the workspace root:

```powershell
python -m venv kiet-cse
.\kiet-cse\Scripts\Activate.ps1
Set-Location .\mlops-first-pipeline
python -m pip install -r requirements.txt
python train.py
python -m uvicorn main:app --reload
```

Training saves the model to `models/iris_random_forest_model.joblib`. Train before starting the API if that file is missing. Open [http://localhost:8000/docs](http://localhost:8000/docs) to explore the API.

### Endpoints

- `GET /` - service information
- `GET /health` - health check
- `POST /predict` - predict an Iris species

Example request body for `/predict`:

```json
{
  "sepal_length": 5.1,
  "sepal_width": 3.5,
  "petal_length": 1.6,
  "petal_width": 0.5
}
```

## Build and run with Docker

From the `mlops-first-pipeline` directory:

```powershell
docker build -t cse-mlops .
docker run --rm -p 8000:8000 cse-mlops
```

Then visit [http://localhost:8000/docs](http://localhost:8000/docs). Docker Desktop must be running.

## Next learning steps

Add API tests in `mlops-first-pipeline/tests/test_api.py`, then configure `.github/workflows/ci.yml` to run those tests and build the Docker image on pushes and pull requests.
