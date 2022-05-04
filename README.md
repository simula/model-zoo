# Simula Model Zoo

Repository for the internal Simula machine learning [model zoo](modelzoo.simula.no).

### Development

The webpages are statically generated from Markdown files using
[Hugo](https://gohugo.io/documentation/). To install it, run for example

```shell
snap install hugo  # Ubuntu
brew install hugo  # MacOS
```
or consult [Installing Hugo](https://gohugo.io/getting-started/installing)
for other installalation options.

To add an entry in the model zoo, clone the repo and use `hugo new` to make a
new page with default metadata:

```shell
git clone git@github.com:simula/model-zoo.git
cd model-zoo
hugo new models/<my-new-model>/index.md   # Replace "<my-new-model>" with the model name
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

After the PR has been approved and merged, the changes must be propagated to
the server at <modelzoo.simula.no>. Either an admin will do this for you, or
you can request admin access by submitting the IT support
[form](https://sites.google.com/a/simula.no/support/).

With admin access, log on to `modelzoo.simula.no` and build the HTML pages:

```shell
# If this is the first time for this user, install Hugo and clone the repo:
sudo snap install hugo
git clone git@github.com:simula/model-zoo.git

# Otherwise, just pull the changes, build public pages and copy them to the
# appropriate location
git pull
hugo
sudo cp -r public/* /var/www/modelzoo/
```


