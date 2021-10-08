# Simula Model Zoo

Repository for the internal Simula machine learning model zoo.

### Development

The webpages are statically generated from Markdown files using
[Hugo](https://gohugo.io/documentation/). To add an entry in the model zoo,
clone the repo and use `hugo new` to make a new page with default metadata:

```shell
git clone git@github.com:simula/model-zoo.git
cd model-zoo
hugo new models/my-new-model/index.md
```

Then edit the new file, and run a local server to check how it looks:

```shell
hugo server -D
# Go to localhost:1313/
```

### Deployment

To build the webpage HTML and save it in the `public/` directory, run
```shell
hugo
```
Make sure to set `draft=false` in the new markdown files, otherwise they will
not be published. 

TODO: Move files to server 
