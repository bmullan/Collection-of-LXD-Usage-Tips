### IMAGE MANAGEMENT {#image-management}

When a container is created from a remote image, LXD downloads the image by pulling its full hash, short hash or alias into it&#039;s image store, marks it as cached and records it&#039;s origin.

Importing Images

From Remote Image Servers to Local Image Store

LXD can simply cache the image locally by copying the remote image into the local image store. This process will not create a container from it.

Below example will simply copy the Ubuntu 14.04 image into the local image store and create a filesystem for it.

$ lxc image copy ubuntu:14.04 local

We can also provide an alias name for the fingerprint that will be generated for the new image. Specifying alias name is an easy way to remember the image.

$ lxc image copy ubuntu:14.04 local: --alias ubuntu1404

It is also possible to use the alias that are already set on the remote image server. LXD can also keep the local image updated just like the images that are cached by specifying the &quot;--auto-update&quot; flag while importing the image.

$ lxc image copy images:centos/6/amd64 local: --copy-aliases --auto-update

Later we can create a container using these local images.

$ lxc launch centos/6/amd64 c2-centos6

From Tarballs to Local Image Store

Alternatively, containers can also be made from images that are created using tarballs. These tarballs can be downloaded fromlinuxcontainers.org. There we can find one LXD metadata tarball and filesystem image tarball.

The below example will import an image using both tarballs and assign an alias &quot;imported-ubuntu&quot;.

$ lxc image import meta.tar.xz rootfs.tar.xz --alias imported-ubuntu

From URL to Local Image Store

LXD also facilitates importing of images from a local webserver in order to create containers. Images can be pulled using their LXD-image-URL and ultimately get stored in the image store.

$ lxc image import http://imageserver.org/lxd/images --alias opensuse132-amd64

Exporting Images

The images of running containers stored in local image store can also be exported to tarballs.

$ lxc image export

Exporting images creates two tarballs: metadata tarball containing the metadata bits that LXD uses and filesystem tarball containing the root filesystem to bootstrap new containers.

Creating &amp; Publishing Images

Creating Images using Containers

To create an image, stop the container whose image you want to publish in the local store, then we can create a new container using the new image.

$ lxc publish container1 --alias new-c1s1

A snapshot of a container can also be used to create images.

$ lxc publish container1/c1s1 --alias new-snap-image

Creating Images Manually

1\. Generate the container filesystem for ubuntu usingdebootstrap.

2\. Make a compressed tarball of the generated filesystem.

3\. Write a metadata yaml file for the container.

4\. Make a tarball of metadata.yaml file.

Sample metadata.yaml file

architecture: &quot;i686&quot;

creation_date: 1458040200

properties:

architecture: &quot;i686&quot;

description: &quot;Ubuntu 12.04 LTS server (20160315)&quot;

os: &quot;ubuntu&quot;

release: &quot;precise&quot;

templates:

/var/lib/cloud/seed/nocloud-net/meta-data:

when:

- start

template: cloud-init-meta.tpl

/var/lib/cloud/seed/nocloud-net/user-data:

when:

- start

template: cloud-init-user.tpl

properties:

default: |

#cloud-config

{}

/var/lib/cloud/seed/nocloud-net/vendor-data:

when:

- start

template: cloud-init-vendor.tpl

properties:

default: |

#cloud-config

{}

/etc/init/console.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty1.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty2.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty3.override:

when:

- create

template: upstart-override.tpl

/etc/init/tty4.override:

when:

- create

template: upstart-override.tpl

5\. Import both the tarballs as LXD images.

$ lxc image import **Illegal HTML tag removed :** <meta.tar.gz>--alias imported-container</meta.tar.gz>

LXD is very likely going to deprecate the lxd-images import functionality for LXD. The image servers are much more efficient for this task.

By default, every LXD daemon plays image server role and every created image is private image i.e. only trusted clients can pull those private images. To create a public image the LXD server must be listening to the network. Below are a few steps to make the LXD server listen to the networ and serve as public image server.

1\. Bind all addresses on port 8443 to enable remote connections to the LXD daemon.

$ lxc config set core.https_address &quot;[::]:8443&quot;

2\. Add public image server in the client machines.

$ lxc remote add **Illegal HTML tag removed :** <ip-address>--public</ip-address>

Adding a remote server as public provides an authentication-less connection between client and server. Still, the images that are marked as private in public image server cannot be accessed by the client.

Images can be marked as public / private using &quot;lxc image edit&quot; command described above in previous sections.

Listing available remote image servers

$ lxc remote list

List images in images: remote server

$ lxc image list images:

List images using filters

$ lxc image list amd64

$ lxc image list os=ubuntu

Get detailed information of an image

$ lxc image info ubuntu