## How to package python application

This project is using `pyproject.toml` approach for configuring build system, described in [PEP-518](https://peps.python.org/pep-0518/). It is using [hatchling](https://pypi.org/project/hatchling/) build backend, configured per [PEP-517](https://peps.python.org/pep-0517/). The result of building is a wheel file.

In order to comply with [PEP-668](https://peps.python.org/pep-0668/) enforced in Debian Bookworm, we have 2 packaging options:

- create a virtual environment and install wheel in there
- create DEB package from wheel and install it using apt-get

Each approach is demonstrated in the corresponding Dockerfile. Each Dockerfile is multi-stage:

`Dockerfile.venv` stages:

- Build a wheel using python base image
- Install wheel in a virtual env on debian:bookworm.
  Notice `ENV PATH=/app/venv/bin:$PATH` part, which makes scripts installed into venv
  reachable for execution from shell

`Dockerfile.deb` stages:

- Build a wheel using python base image
- Package a wheel into a DEB package using [wheel2deb](https://github.com/upciti/wheel2deb)
- Install DEB package on debian:bookworm

Both images set `CMD ["add-cli"]` - a cli script provided by the wheel.

## Additional notes

The project is using [src-layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/).

The project is using [pipenv](https://pipenv.pypa.io/en/latest/) for managing its virtualenv and dependencies

wheel2deb is a tool that creates deb packages from python's built distribution format (wheel). There're 2 more tools available to create deb packates from python's source distribution format: [stdeb](https://pypi.org/project/stdeb/) and [py2deb](https://pypi.org/project/py2deb/)

More informaion about difference between built distribution and source distribution formats: https://packaging.python.org/en/latest/specifications/#package-distribution-file-formats
