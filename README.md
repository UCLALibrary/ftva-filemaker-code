# ftva-filemaker-code
Code (mostly scripts) extracted from FTVA Filemaker systems, and related documentation.

## Generating the files

1. Connect the Filemaker client to the **production system** (Adam).
2. For each relevant database, use Filemaker's "Tools -> Save a Copy as XML"
    1. Labeling
    2. Inventory for Labeling (after switching to the general entry layout). Note that several database files are reported as not found. These are OK: apparently they are references to former databases, but are no longer used.
3. Save the production XML exports in this repository's `xml_exports/prod_adam/` directory. 
    1. Exports from the test system should go in `xml_exports/test_eve/`.  These should be used only for comparison, being sure not to overwrite any production files here or in later steps.
    2. Export files from the test system may be parsed for comparison with production, but must not be commited to the `main` branch.  Use these only in working branches and be careful not to replace production files with test files.
    3. XML files in `xml_exports/` are not tracked in this repository.
4. Use [FileMaker XML Export Exploder](https://github.com/bc-m/fm-xml-export-exploder) to parse the exported XML into separate files.  These are mainly in two formats:
    1. XML: Individual files for each component (scripts, schemas, and more).
    2. TXT: Individual files for each script, in readable format parsed from the native Filemaker XML.
5. Files parsed with the command below will go into separate folders for each exported XML file, overwriting existing files.
6. Compare, add, and commit files via `git` as usual.

```
# Parse all exported XML files in the given directory, with output going into database-specific
# files in the current directory (note the trailing `.`)
$ fm-xml-export-exploder --obfuscate-passwords xml_exports/prod_adam/ .
```

### General notes

The exported XML files [contain information](https://help.claris.com/en/pro-help/content/documenting-schemas.html) about each database's schemas, layouts, and scripts. Although readable (well, XML...), the information uses the vendor's own terminology and most files include opaque ids. These files cannot be edited safely by hand.

For scripts, `fm-xml-export-exploder` also creates versions in each database's `scripts_sanitized` sub-directory. These appear to be accurate plain-text representations derived from the original XML, including individual parameters and values which are hidden behind pop-up dialog forms in the Filemaker script editor.

Although readable, these "sanitized" scripts also cannot be edited safely by hand. They must only be edited within the Filemaker script editor. 

The search continues for tools which allow accurate external editing and pasting back into Filemaker.

### Other useful tools

* [SharpFM](https://github.com/fuzzzerd/SharpFM): Allows limited copy/paste of scripts into & out of Filemaker.
  * Does not work correctly with our version of Filemaker: 20.1.2.204 (05-10-2023)
* [Clockwork Inspector for FileMaker](https://github.com/andykear/FileMaker-XML-inspector-open-source): Parses exported XML into a much more attractive and usable version of Filemaker's Database Design Report exported as HTML.
  * The script viewer is attractive, but does not show at least some of the parameters and values which are hidden behind pop-up dialog forms.
