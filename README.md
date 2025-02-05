# OLM Convert

The free, open-source OLM convertor.

<img src="olmConvert.png" width="100px"/>

[Download for Mac (Apple Silicon)](https://github.com/PeterWarrington/olm-convert/releases/download/v1.2.0/OLM.Convert.1.2.0.Mac.Arm.zip) |
[Download for Mac (Intel)](https://github.com/PeterWarrington/olm-convert/releases/download/v1.2.0/OLM.Convert.1.2.0.Mac.Intel.zip) | 
[Download for Windows (x64)](https://github.com/PeterWarrington/olm-convert/releases/download/v1.2.0/OLM.Convert.1.2.0.Win.x64.zip) |
[Download Python source (Linux, or command-line)](https://github.com/PeterWarrington/olm-convert/archive/refs/tags/v1.2.0.zip)

A utility to convert an Outlook for Mac archive (.olm file), that can only be opened with Outlook for Mac, to a set of standard .eml files that can be opened by almost all email clients.

Output EML messages are organised hierarchically, for example a message contained in an OLM file will be output to an EML file like `"<output directory>/example@example.com/Inbox/Subject - Mon, 04 July 2022 21.04.56.eml"`.

Available as a GUI, a python command-line interface, and as a module.

Supports attachments but can only output emails with the HTML and plain text content types.

## Usage

### GUI:

<img src="screenshot.png" width="400px" alt="Screenshot of GUI"/>

### Command line:
```
python3 olmConvert.py <path to OLM file> <output directory> [--noAttachments]
```

#### Command line options

* `--noAttachments` - No attachments are included in generated EML files (including embedded images), reducing file size of generated EML files.

## Module reference

### convertOLM(olmPath, outputDir, noAttachments=False, verbose=False)
Convert OLM file specified by `olmPath`, creating a directory of EML files at `outputDir`. Will not include attachments if optional parameter `noAttachments` is set to True.

### processMessage(xmlString, olmZip=None, noAttachments=False)
Reads OLM format XML message (`xmlString`) and returns a ConvertedMessage object containing the message converted to a EML format string. Can also return ValueError.

`olmZip` parameter is a instance of `zipfile.ZipFile` open on the OLM file. This parameter is required in order to convert attachments.

If `noAttachments` parameter is True, no attachments will be included in generated EML messages.

### headerEncode(value)
Converts email header string value to RFC2047 base64 encoded UTF-8 string (<https://datatracker.ietf.org/doc/html/rfc2047>).

### addressEncode(addressElm)
Converts `<emailAddress>` element ([xml.etree.ElementTree.Element](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element)) to an email header value.

### generateBoundaryUUID()
Generates a MIME boundary ID (<https://datatracker.ietf.org/doc/html/rfc2046#section-5.1.1>).

### lineWrapBody(body)
Wraps lines of a given HTML body to maximum of 78 characters as recommended by RFC 2822 (https://datatracker.ietf.org/doc/html/rfc2822#section-2.1.1).

### processAttachment(attachmentElm, olmZip)
Generates EML section (without MIME boundaries) specifying attachments.

`attachmentElm` specifies the `<messageAttachment>` OLM element ([xml.etree.ElementTree.Element](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element)) containing the attachment.

`olmZip` parameter is a instance of `zipfile.ZipFile` open on the OLM file. Required as attachment files are contained within OLM file.
