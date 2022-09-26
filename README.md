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
    <li><a href="#acknowledgments">Acknowledgments</a></li>
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

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

Use this space to list resources you find helpful and would like to give credit to. I've included a few of my favorites to kick things off!

* [Choose an Open Source License](https://choosealicense.com)
* [GitHub Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet)
* [Malven's Flexbox Cheatsheet](https://flexbox.malven.co/)
* [Malven's Grid Cheatsheet](https://grid.malven.co/)
* [Img Shields](https://shields.io)
* [GitHub Pages](https://pages.github.com)
* [Font Awesome](https://fontawesome.com)
* [React Icons](https://react-icons.github.io/react-icons/search)



<!-- MARKDOWN LINKS & IMAGES -->
[flask-url]: https://flask.palletsprojects.com/en/2.2.x/
[pyjwt-url]: https://pyjwt.readthedocs.io/en/stable/
[transformers-url]: https://huggingface.co/docs/transformers/index
[vit-url]: https://huggingface.co/google/vit-base-patch16-224
[gunicorn-url]: https://gunicorn.org/
[docker-install-url]: https://docs.docker.com/engine/install/
