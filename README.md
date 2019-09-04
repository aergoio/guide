# Aergo Guide

[![Readthedocs](https://readthedocs.com/projects/blocko-inc-guide/badge/?version=latest)](http://docs.aergo.io/en/latest/)

This is the official guide and documentation for the AERGO blockchain platform.

## Contribute

Feel free to edit any of the articles directly. Any contribution, including simple typo fixes, is more than welcome.
We are looking forward to your pull request.

For anything that involves more than simple text changes, please test the build locally to see that everything looks right.

Pre-requisites:

1. Install Sphinx globally: `pip install -U sphinx`
2. Install local requirements: `pip install -r requirements.txt`

Build locally:

    make html

Re-generate GRPC reference:

    make protoc
