# Alpine Linux with VNC

> A really simple Alpine Linux setup with VNC installed.

 :exclamation: **Note:** The image will be flattened to try and keep things as small as possible once Docker Hub builds support squashing (see [Issue 955](https://github.com/docker/hub-feedback/issues/955)).

[![Docker Build Status](https://img.shields.io/docker/build/version42/alpine-vnc.svg)](https://hub.docker.com/r/version42/alpine-vnc/) [![Docker Pulls](https://img.shields.io/docker/pulls/version42/alpine-vnc.svg)](https://hub.docker.com/r/version42/alpine-vnc/) [![Docker Stars](https://img.shields.io/docker/stars/version42/alpine-vnc.svg)](https://hub.docker.com/r/version42/alpine-vnc/) [![](https://images.microbadger.com/badges/image/version42/alpine-vnc:develop.svg)](https://microbadger.com/images/version42/alpine-vnc:develop "Get your own image badge on microbadger.com") [![](https://images.microbadger.com/badges/version/version42/alpine-vnc:develop.svg)](https://microbadger.com/images/version42/alpine-vnc:develop "Get your own version badge on microbadger.com")

This is [yet another] very basic container specification of [Alpine Linux 3.5](https://alpinelinux.org/) with [X11VNC (0.9.13-r0)](https://wiki.archlinux.org/index.php/Official_repositories) installed.

Yes, I know there's a few of these out there already but I want one that is specifically under my own control in case the other ones happen to disappear or make changes that could potentially cause issues.  I encourage you to do the same so that you have something you can keep up-to-date based on this and other images for testing and verification of all of your deployments.

A list of all versions and all what you can pull from the builds is available on the [Docker Hub](https://hub.docker.com/r/version42/alpine-vnc/).

## Why This Image?

On the surface, there's not much to this image and it's not incredibly useful.  This is more of a start to getting things rolling for various UI concerns I have.  I am using this as the basis for all of my other Docker  work that involves things like setting up images that support [Selenium](http://www.seleniumhq.org) tests, libraries that for some stupid reason require a display to do some work, etc.

## Tagging and Branching Strategy

The strategy I am using for tagging and deployment is pretty simple.  Basically the *master* branch will always be tagged as *latest* and development will always be tagged as *development*.  That aside, all other versions/branches will be tagged using an appended string made up of:

* version of Alpine
* version of X11VNC

For example, Alpine 3.5 with X11VNC 0.9.13-r0 will be tagged as the following in Docker Hub and the Git branch associated with it will be named exactly the same:

`alpine3.5-vnc0.9.13-r0`

You should note that this is the default process from Docker Hub:  *the Docker tag matches the Git branch name*.  It's a great convenience to have this as we do not override or specialize the functionality since their "best practices defaults" matches our desired strategy.

## Why did you pack the APKs into the repository?

There's some oddities I ran across where I could not reliably get the exact packages and versions I wanted installed.  For this reason I downloaded and put them in the repository.  Some of this can be seen through the policies associated with Alpine and via [this commit](https://git.alpinelinux.org/cgit/aports/log/unmaintained/x11vnc?showmsg=1) within the Alpine repositories.

## How to Build the Container Image

### Building

> The basic image build

Basic build command as you'd expect:
```bash
docker build -t "version42/alpine-vnc" .
```

### Launching

> So you want to actually launch the image?  OK...

The basic launch is what you'd expect with one additional flag:
```bash
docker run --cap-add=SYS_ADMIN -p 5900:5900 version42/alpine-vnc
```

See that `--cap-add`?  We need that for certain things you may install like [Chromium](https://www.chromium.org/Home).  Without this you'll get namespace issues and other things that will pop-up unless you run those applications with specialized options (such as `--no-sandbox` for Chromium).  Some applications, like [Firefox](http://www.getfirefox.com), don't suffer from the same issues as others do and therefore it **may** not be necessary; however, I still recommend adding it as it will reduce some errors for basic container usage.  Remove it if you are sure you don't need it, but I keep it around for this particular image as I use this mostly for testing.

### Deployment

> What, the Docker Hub automated process not good enough for you?

1. Build (see above)

2. [Tag](https://docs.docker.com/engine/reference/commandline/tag/) the image based on the strategy listed above (we'll use our 3.5/0.9.13-r0 example):
```bash
docker tag "version42/alpine-vnc" version42/alpine-vnc:alpine3.5-vnc0.9.13-r0
```

4. [Push](https://docs.docker.com/engine/reference/commandline/push/) the image to [Docker Hub](https://hub.docker.com):
```bash
docker push version42/alpine-vnc:alpine3.5-vnc0.9.13-r0
```
