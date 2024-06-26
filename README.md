# raccoon-spotter
![](https://i.imgur.com/zodNdbL.png)

## Overview
This project utilizes the Kedro framework to build and manage a deep learning pipeline for a simple raccoon object detection. Data is stored in an S3 bucket. We also incorporate Weights & Biases for the monitoring and tracking of  experiments.

## Rules and guidelines
* Don't remove any lines from the provided `.gitignore` file
* Make sure your results can be reproduced by following a [data engineering convention](https://docs.kedro.org/en/stable/faq/faq.html#what-is-data-engineering-convention)
* Don't commit data to the repository
* Don't commit any credentials or local configuration to the repository. Keep all the credentials and local configuration in `conf/local/`
* Adhere to the [commit conventions](<https://www.conventionalcommits.org/en/v1.0.0/>)

## Environment setup
1. Install [pipenv](<https://pipenv.pypa.io/en/latest/>) if you don't already have it:
	```
	pip install --user pipenv
	```
2. Navigate to the project directory:
	```
	cd raccoon-spotter
	```
3. Create virtual environment:
	```
	pipenv install --dev
	```
4. Enable pre-commit
	```
	pipenv run pre-commit install
	pipenv run nb-clean add-filter --remove-empty-cells
	```
5. Authenticate with external serivices
	```
	kedro authenticate
	```

## Workflow
### Virtual environment
Activate your virtual environment to start working on the project:
```
pipenv shell
```
And deactivate it when you are done:
```
exit
```

### Kedro
This project is managed by the kedro framework.
To run the project execute:
```
kedro run
```
To visualize the pipeline run:
```
kedro viz run
```
Use notebooks to access individual nodes and run experiments:
```
kedro jupyter notebook --no-browser
```
Cleaning the local project data:
```
kedro clean data
kedro clean notebooks
```

### Testing
Have a look at the files `src/tests/*` for examples on how to write your tests. Run the tests as follows:
```
pytest
```
To configure the coverage threshold, look at the `.coveragerc` file.

### Linting
Ruff linting is done automatically on each commit.

## Application
### Local deployment
Build and run a dockerized application with:
```
kedro app
```
You can also specify a custom model path by adding `--model <path>` option.
Models are imported from `data/06_models/trained_model.keras` by default.

### API Usage
#### <strong>Endpoint:</strong> `/predict`  
<strong>Method:</strong> POST  
<strong>Request Body:</strong> JSON containing a base64 encoded image string.

```json
{ "image": "base64_encoded_image_data" }
``` 

<strong>Response:</strong> JSON object with the processed image encoded in base64.

```json
{ "image_base64": "processed_image_data" }
```

For technical details have a look at the Dockerfile and the `app/` files.