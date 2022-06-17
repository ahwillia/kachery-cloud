<img src="https://user-images.githubusercontent.com/3679296/161265718-1127dd6a-a7c4-419b-b9e0-915740c418bc.svg" width="400px" />

# kachery-cloud

> :warning: This project is in alpha stage of development.

> **IMPORTANT**: This package is intended for collaborative sharing of data for scientific research. It should not be used for other purposes.

Share scientific research data in the cloud using Python.

Kachery-cloud is the foundation of [figurl](https://github.com/scratchrealm/figurl2).

Contents

* [Installation and setup](#installation-and-setup)
* [Basic usage](#basic-usage)
* [Using your own storage bucket](#using-your-own-storage-bucket)
* [Environment variables](#environment-variables)
* [Sharing the kachery cloud directory between multiple users](#sharing-the-kachery-cloud-directory-between-multiple-users)
* [Notes](#notes)

## Installation and setup

Kachery-cloud is often installed as a dependency of other projects, but there are times when you may want to use it stand-alone.

It is best to use a conda environment or a venv.

Requirements
* Python >= 3.8
* numpy

```bash
pip install kachery-cloud

# or for the development version, clone this repo and install via "pip install -e ."
```

To complete the setup, open a terminal and run 

```bash
# One-time initialization
kachery-cloud-init

# Follow the instructions to associate your computer with your Google user on the kachery-cloud network
```

Clicking the link will bring you to a page where you associate this account with a Google user ID for the purpose of managing projects and tracking usage. This initialization only needs to be performed once on your computer. The client information will be stored in `~/.kachery-cloud`.

If you are using a colab or jupyter notebook and do not have easy access to a terminal, you can also run this one-time step in the notebook:

```python
# One-time initialization (alternate method)
import kachery_cloud as kcl
kcl.init()

# Follow the instructions to associate the client with your Google user on the kachery-cloud network
```

## Basic usage

The four entities managed by kachery-cloud are files, feeds, tasks, and mutables.

* [Storing and loading data in the kachery cloud](doc/store_load_data.md)
* [Storing and loading data in the local cache](doc/store_load_data_local.md)
* [Setting and getting mutables in the cloud or locally](doc/set_get_mutable.md)
* [Feeds (append-only logs)](doc/feeds.md)
* [Tasks](doc/tasks.md)

## Using your own storage bucket

By default, projects will use our inexpensive cloud storage, and your data is not guaranteed to be available forever. However, it is also possible to configure your own cloud storage provider, which you pay for. This configuration is available in the web app at the time you configure your kachery-cloud client.

There are four main choices for configuring your own storage bucket for use with kachery-cloud. Click the links in the table below to get detailed instructions on configuring a bucket for each service.

> Note: The following prices are estimates based on glancing at the websites. Actual pricing may depend on other factors.

| Provider | Storage per TB month | Download per TB | Notes |
| -------- | ------------- | ----- | --- |
| [AWS](./doc/aws_bucket_instructions.md) | $23 | $90 |  |
| [Google](./doc/google_bucket_instructions.md) | $23  | $120 | You need to obtain S3-style credentials |
| [Wasabi](./doc/wasabi_bucket_instructions.md) | $6 | Free for reasonable use | |
| Filebase | $6 | $6 | |

Wasabi is the recommended choice right now. The first two options are a lot more expensive, but presumably more reliable.

## Environment variables

You can use environment variables to control the storage/configuration directory used by kachery-cloud and the project ID used for storing data in the cloud.

```bash
# Set the storage/configuration directory used by kachery-cloud
# If unset, $HOME/.kachery-cloud will be used
# The client ID will be determined by this directory
# You can share the same kachery-cloud directory between multiple users,
# but you will need to set mult-user mode for the client
export KACHERY_CLOUD_DIR="..."

# Set the project ID for storing data in the cloud
# If unset, the default project associated with the client will be used
# The default project can be configured at https://cloud.kacheryhub.org
export KACHERY_CLOUD_PROJECT="..."

# In ephemeral mode, the client does not need to actually be registered
# but you will only be able to perform a subset of operations (mostly readonly)
# This can be useful for unit testing environments when you want to download
# test files from the kachery network without registering a client.
export KACHERY_CLOUD_EPHEMERAL="FALSE"
```

It is recommend that you set these variables in your `~/.bashrc` file.

## Sharing the kachery cloud directory between multiple users

On a shared system, you may want to share your kachery cloud directory between multiple users so that
they can utilize the same projects, mutables, local files, and task backends. Follow these steps:

* Create a new kachery cloud directory in a location where the users may access it
with read and write permissions. For example, this could be on a shared drive.
* Have each user set the KACHERY_CLOUD_DIR environment variable to point to this
directory on their system (see above)
* Have the main user (the one who will own the client) initiatialize the client as usual via
`kachery-cloud-init`
* Set `multiuser` to `true` in `$KACHERY_CLOUD_DIR/config.yaml`

The last step is necessary so that all files are created with read/write access for
all users.

## Notes

This project is still in the alpha stage of development and we do not guarantee that your data will be available forever.

## Authors

Jeremy Magland and Jeff Soules, [Center for Computational Mathematics, Flatiron Institute](https://www.simonsfoundation.org/flatiron/center-for-computational-mathematics)

## License

Apache 2.0
