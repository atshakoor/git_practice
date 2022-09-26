<!-- HEADER -->
<br />
<div align="center">
  <h3 align="center">API: Image Classifier Using Vision Transformer (ViT-B/16)</h3>
</div>
<br />


<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

This project implements a RESTful API that is capable of detecting objects present in an image. It utilizes google's Vision Transformer base model (ViT-B/16) which is trained and fine-tuned on 'ImageNet-21k' and 'ImageNet 2012' datasets respectively, and is capable of detecting 1000 classes. This app has been developed and configured to be deployed and served from a docker container.  

Features in brief:
* Authorization through Json Web Token (no users db, only single-user allowed in current implementation, can be changed upon discussion).
* Prediction of object label(s) present in an image.  



### Built With

This app has been developed utilizing the following, 

* [Flask][flask-url]
* [PyJWT][pyjwt-url]
* [Transformers][transformers-url]
* [Vision Transformer][vit-url]
* [gunicorn][gunicorn-url]



<!-- GETTING STARTED -->
## Getting Started

Please go through the following instructions to set up the project locally.

### Prerequisites

* Docker (Ubuntu)

	* Update the apt package index and install packages to allow apt to use a repository over HTTPS:
  	```sh
  	sudo apt-get update
  	
  	sudo apt-get install ca-certificates curl gnupg lsb-release
  	```
  	
  	* Add Docker’s official GPG key:
  	```sh
  	sudo mkdir -p /etc/apt/keyrings
  	
  	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  	```
  	
  	* Use the following command to set up the repository:
  	```sh
  	echo \
  	"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  	$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

  	```
  	
  	* Update the apt package index, and install the latest version of Docker Engine, containerd, and Docker Compose:
  	```sh
  	sudo apt-get update
  	
  	sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
  	```
 
To install Docker on some other distribution, please go through the [official documentation][docker-install-url]

### Installation

1. Clone the repo,
   ```sh
   git clone https://bitbucket.therapbd.net/scm/thrp/bi-advanced-analytics.git
   ```
2. Navigate to directory where 'Dockerfile' resides and build the docker image,
   ```sh
   docker build -t img-api-classifier:0.0.1dev1 .
3. By default, gunicorn server will be live on port 80 once the docker container is up. To change the port configuration, edit the WEB_BIND environment variable value to desired port in the docker-compose.yml file,
   ```sh
   environment:
      - WEB_BIND=0.0.0.0:80 
   ```
4. Run the docker container where docker-compose.yml file resides,
   ```sh
   docker compose up -d
   ```



<!-- USAGE EXAMPLES -->
## Usage

The app exposes two endpoints,
* http://<host>:<port>/users/login: Handles auth token generation.
* http://<host>:<port>/prediction: Handles object detection from image.

To use the endpoint '/prediction', an authorization token is requiered. The following is an example of a POST request generated using curl utility for user authentication and auth token generation,
```sh
curl --location --request POST 'http://192.168.0.1:80/users/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username":"therap",
    "password":"Therap321#"
}'
```
which will generate the following response payload,
```sh
{
    "data": {
        "exp": "Mon, 26 Sep 2022 04:45:08 GMT",
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYmNiOTBiNmYtYWFiMC00M2E4LWFlZTYtZDI2NzBjN2QyOWRhIiwidXNlcm5hbWUiOiJ0aGVyYXAiLCJleHAiOjE2NjQxNjc1MDh9.dPqFoVrGK0KAZtIePAYVUzAuz65haqN8tol52-lQkT8",
        "user_id": "bcb90b6f-aab0-43a8-aee6-d2670c7d29da",
        "username": "therap"
    },
    "message": "Successfully fetched auth token"
}
```

Use the username and token received in the header 'Authorization' to generate request for object detection. The token will be valid for 1 hour(s).
```sh
curl --location --request POST 'http://192.168.0.1:80/predict' \
--header 'Authorization: therap eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoiYmNiOTBiNmYtYWFiMC00M2E4LWFlZTYtZDI2NzBjN2QyOWRhIiwidXNlcm5hbWUiOiJ0aGVyYXAiLCJleHAiOjE2NjQxNjc1MDh9.dPqFoVrGK0KAZtIePAYVUzAuz65haqN8tol52-lQkT8' \
--form 'image=@"/home/user/Pictures/animals.jpg"'
``` 
The above request will generate the following response if successful,
```sh
{
    "data": {
        "result": "ice bear, polar bear, Ursus Maritimus, Thalarctos maritimus"
    },
    "message": "Label prediction successful!"
}
```

** Please note, currently this api can only be accessible by user 'therap'. If required, an user database can be implemented based on further discussion.  



<!-- MARKDOWN LINKS & IMAGES -->
[flask-url]: https://flask.palletsprojects.com/en/2.2.x/
[pyjwt-url]: https://pyjwt.readthedocs.io/en/stable/
[transformers-url]: https://huggingface.co/docs/transformers/index
[vit-url]: https://huggingface.co/google/vit-base-patch16-224
[gunicorn-url]: https://gunicorn.org/
[docker-install-url]: https://docs.docker.com/engine/install/
