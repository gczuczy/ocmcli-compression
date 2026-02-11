# ocmcli-compression
Showcasing OCM issue https://github.com/open-component-model/open-component-model/issues/1388

To run it call `gmake dl`, which creates an OCM CV in a dir, and then extracts the packed payload, which is marked as compressed:
```
mkdir -p ctf
ocm add componentversions --create --file ctf /home/gczuczy/github.com/gczuczy/ocmcli-compression/component-descriptor.yaml
processing /home/gczuczy/github.com/gczuczy/ocmcli-compression/component-descriptor.yaml...
  processing document 1...
    processing index 1
found 1 component
adding component foo.bar/foobar:0.0.1...
  adding resource file: "name"="prayer","version"="0.0.1"...
ocm download resource -O prayer-dl.txt directory::ctf//foo.bar/foobar prayer
prayer-dl.txt: 319 byte(s) written
file prayer-dl.txt
prayer-dl.txt: gzip compressed data, original size modulo 2^32 291
```

For compressed payloads, expectations would be to uncompressed it. Or at least to have an option for it.
