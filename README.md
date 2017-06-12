# Machine Parsable Release Information Specification

## Status

The specification is currently an initial draft.

## Introduction

Several projects/vendors publish releases of their GNU/Linux distributions on
various cadences. They ship ISO and disk images (in various file formats) for
one or more flavours of their distributions. There is no common way to retrieve
the release information in a machine parsable and secure way. The user has to
visit the website of the project and search in the download section.

The proposed solution is to let the projects/vendors provide a machine parsable
release information file which contains the information about all their releases
and available files. To ease generating an overview page, specify a file format
to use.

## Conventions

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”,
“SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## Document Structure

The release information file MUST be valid JSON and follow the document
structure specified by [JSON API](http://jsonapi.org/).

The top level resource object in "data" MUST be of type "project". The id SHOULD
be the name of the vendor's project and the name SHOULD not cause a conflict
with other vendors. The project resource MUST contain the attributes "releases"
which is a list of objects. Each object MUST contain release, format, flavor,
href, checksums, date, size.

The "meta" object MUST either provide a signature member that points to a
detached gpg signature for the release information file or the full release
information file must be securely distributed (via https).

Example:

```json
{
  "data": {
    "type": "project",
    "id": "Ubuntu",
    "attributes": {
      "releases": [{
        "version": "17.04",
        "format": "iso",
        "flavor": "server",
        "architecture": "amd64",
        "href": "http://releases.ubuntu.com/17.04/ubuntu-17.04-server-amd64.iso",
        "size": 718274560,
        "date": "2017-04-12 03:20:00",
        "checksums": {
          "md5": "4672ce371fb3c1170a9e71bc4b2810b9",
          "sha1": "61479ab49395ac7374c1d0e2226267d997653715",
          "sha256": "ca5d9a8438e2434b9a3ac2be67b5c5fa2c1f8e3e40b954519462935195464034",
        }
      }]
    }
  },
  "meta": {
    "signature": "http://example.com/signature.gpg",
    "updated": "2017-05-05 18:54:33",
  }
}
```

### version

The major release of the project. Do not specify minor release numbers of
image build version in here.

### format

The file format. It MUST be one of

* iso: for ISO 9660 image (that are intended for CD-ROM, DVD, or Blu-ray)
* tar: tarball with no extra compression
* tar.xz: xz-compressed tarball
* raw: Raw disk image
* vmdk: Virtual Machine Disk (VMDK)
* vdi: VirtualBox Disk Image (VDI)
* vhd: Virtual Hard Disk (VHD) used by Microsoft Virtual PC
* qcow2: QEMU image format

Please provide a patch for this specification in case a file format is missing.

### flavor

The flavor of the image. It MUST be one of

* desktop
* server
* cloud

Please provide a patch for this specification in case a flavor is missing to
unify the names used here.

### href

The URL for downloading the image.

### checksums

checksums MUST be an object that MUST include one or more of these members:

* sha256
* sha3

and MAY include one or more of these members:

* md5
* sha1

Each member contains a string with the checksum for the file specified in href.

Please provide a patch for this specification in case a checksum format is
missing.
