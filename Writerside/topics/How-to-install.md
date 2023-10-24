# How to install

Instructions on how to install the package using PIP & Docker.

```bash
$ pip install qiskit-classroom-converter
```

> **Warning**
>
> Mac ARM chips users may have issues running this package.
> 
> We have provided a Dockerfile, which can be used GitHub Container registry.
>
{style="warning"}

## Docker Pull & Run

Alternative installation with docker image.

You can run the Jupyter notebook Server.

```shell
docker pull ghcr.io/kmu-quantum-classroom/qiskit-classroom-converter
docker run -p 8888:8888 ghcr.io/kmu-quantum-classroom/qiskit-classroom-converter
```