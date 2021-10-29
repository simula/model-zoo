# Simula Model Zoo

Repository for the internal Simula machine learning model zoo.

### Development

The webpages are statically generated from Markdown files using
[Hugo](https://gohugo.io/documentation/). To add an entry in the model zoo,
clone the repo and use `hugo new` to make a new page with default metadata:

```shell
git clone git@github.com:simula/model-zoo.git
cd model-zoo
hugo new models/<my-new-model>/index.md   # Replace "<my-new-model>"
```

Then edit the new file, and run a local server to check how it looks:

```shell
hugo server
# Go to http://localhost:1313
```
Make sure to set `draft=false` in the new markdown files, otherwise they will
not be published.

If everything looks good, create a branch for the new changes and submit a pull
request:
```shell
git checkout -b feature/<my-new-model>
git add models/<my-new-model>/index.md
git ci -m "Added my new model"
git push -u origin feature/<my-new-model>
# Click the link to github.com and finish the PR
``` 

### Deployment

After the PR has been approved and merged, log in to  `modelzoo.simula.no` and
build the HTML pages:

```shell
git pull    # Pull changes
hugo        # Build pages
sudo cp -r public/* /var/www/modelzoo/*
```
