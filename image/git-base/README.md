# git-base

git-init depends on a base image override (see it's .ko.yaml) to build the application inside an image which includes git, a shell and some other tooling used by the Tekton task.

However, the projects' chosen base image has changed upstream and doesn't include some of these tools in later releases.

While the project sorts this out, this is a simple base image to use for this purpose, which makes it possible to build git-init with the latest bugfixes - especially the "origin not found" showstopper bug.

## Build and push git-base

    buildah bud -t ghcr.io/sequitor/git-clone/git-base:latest -f Containerfile
    podman push ghcr.io/sequitor/git-clone/git-base:latest

## Build and push git-init using ko

    brew install ko
    cd images/git-init
    ko build -B .

## Post image build

Don't forget to update the tekton task git-clone with the latest sha256 digest for the git-init image.
