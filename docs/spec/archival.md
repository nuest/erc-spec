# Archival extension

This extension places the ERC in the context of preservation workflows by defining structural information and other metadata that guarantee interpretability and enable the bundling of the complete ERC as a self-contained, archivable digital object.

## Archival bundle

In order to bundle the complete ERC, it can be packaged using the [BagIt][bagit] standard. This allows to store and transfer arbitrary content along with minimal metadata as well as checksum based payload validation.

For the purpose of transferring and storing an ERC, this extension defines how an ERC can be packaged using BagIt as the outer container and what limitations may apply.

The remainder of this section comprises (i) a description of the outer container, and (ii) a BagIt profile.

### BagIt outer container

The ERC base directory MUST be the BagIt payload directory `data/`.
The path to the ERC configuration file subsequently MUST be `<path-to-bag>/data/erc.yml`.

The bag metadata file `bag-info.txt` MUST contain a metadata element of the label `ERC-Version` and the version of the ERC payload as value.

```txt
Payload-Oxum: 2172457623.43
Bagging-Date: 2016-02-01
Bag-Size: 2 GB
ERC-Version: 1
```

Example file tree for a bagged ERC:

```txt
├── bag-info.txt
├── bagit.txt
├── data
│   ├── 2016-07-17-sf2.Rmd
│   ├── erc.yml
│   ├── metadata.json
│   ├── Dockerfile
│   └── image.tar
├── manifest-md5.txt
└── tagmanifest-md5.txt
```

### BagIt profile - _Under Development_

A [Bagit Profile][bagitprofiles] as outlined below could make the requirements of this extension more explicit.

```json
{
     "BagIt-Profile-Info":{
      "BagIt-Profile-Identifier":"http://o2r.info/erc-bagit-v1.json",
      "Source-Organization":"o2r.info",
      "Contact-Name":"o2r Team",
      "Contact-Email":"o2r@uni-muenster.de",
      "External-Description":"BagIt profile for packaging executable research compendia.",
      "Version":"1"
   },
   "Bag-Info":{
      "Contact-Name":{
         "required":true
      },
      "Contact-Email":{
         "required":true
      },
      "External-Identifier":{
         "required":true
      },
      "Bag-Size":{
         "required":true
      },
      "Payload-Oxum":{
         "required":true
      }
   },
   "Manifests-Required":[
      "md5"
   ],
   "Allow-Fetch.txt":false,
   "Serialization":"optional",
   "Accept-Serialization":[
      "application/zip"
   ],
   "Tag-Manifests-Required":[
     "md5"
   ],
   "Tag-Files-Required":[
      ".erc/metadata.json",
      ".erc.yml"
   ],
   "Accept-BagIt-Version":[
      "0.96"
   ]
}
```

[bagit]: http://tools.ietf.org/html/draft-kunze-bagit
[bagitprofiles]: https://github.com/ruebot/bagit-profiles

## ERC preservation aspects in the light of OAIS

The Open Archival Information System (OAIS) reference model is a framework for the preservation and dissimination of digital objects (assets). It has been created by the Consultative Committee for Space Data Systems (CCSDS) and  has since been adopted by a wide range of international institutions. The OAIS provides the terminology and concept of the _information package_ as primitive of the digital preservation workflow. Submission information packages (SIP), Dissimination information packages (DIP) and Archival information packages (AIP) refer to different functional roles of a digital object during (long term) preservation.

The representation information object within OAIS consists of structural and semantic information and is itself linked to other units of representation information, building a representation network. The information contained within the ERC and its metadata serves as representation information and enables the interpretability of the archived software, environment, code, data, text and UI bindings. Standards used for representation are included as local copy of the underlying schema and as reference to their persistent identifiers. The network of information objects describes how the parts of the ERC relate to each other and how they are to be used and understood.


References

- CCSDS (2012): RECOMMENDED PRACTICE FOR AN OAIS REFERENCE MODEL. CCSDS 650.0-M-2 (Magenta Book). http://public.ccsds.org/publications/archive/650x0m2.pdf
- Maack, M.N. (2015). The Open Archival Information System (oais) Reference Model.


## Secondary metadata files

The ERC as an object can be used in a broad range of cases. For example, it can be an item under review during a journal publication, it can be the actual publication at a workshop or conference or it can be a preserved item in a digital archive. All of these have their own standards and requirements to apply, when it comes to metadata.

~~These metadata requirements _are not_ part of this specification, but~~ the following conventions are made to simplify and coordinate the variety.

Metadata specific to a particular domain or use case MUST replicate the information required for the specific case in an independent file.
Domain metadata SHOULD follow domain conventions and standards regarding format and encoding of metadata.
Duplicate information is accepted, because it lowers the entry barrier for domain experts and systems, who can simply pick up a metadata copy in a format known to them.

Metadata documents of specific use cases MUST be stored in a directory `.erc`, which is a child-directory of the ERC base directory.

Metadata documents SHOULD be named according to the used standard or platform, and the used format respectively encoding, e.g. `datacite40.xml` or `zenodo_sandbox10.json`, and SHOULD use a suitable mime type.

### Requirements of secondary metadata

In order to comply to their governing schemas, secondary metadata must include the mandatory information as set by 3rd party services. While the documentation of this quality is a perpetual task, we have gathered the information most relevant our selection of connected services.

**Zenodo**

+ Accepts metadata as json.
+ Mandatory elements:
	+ Upload Type (e.g. Publication)
	+ Publication Type
	+ Title
	+ Creators
	+ Description
	+ Publication Date
	+ Access Right
	+ License



**DataCite (4.0)**

+ Accepts metadata as xml.
+ Mandatory elements:
	+ Identifier
	+ Creator
	+ Title
	+ Publisher
	+ Publication Year
	+ Ressource Type


Other third party standards that will be considered comprise: _CodeMeta_, _EuDat_, _mets/mods_.