# alternate OMERO storage futures

## variables

**write internal** - yes/no - Ability to write working memory contents out to storage in/under (managed by) the OMERO server.  This exists today.  Not sure the format.

**read external** - yes/no - Ability to read a referenced location and load the contents into working memory.  This may include multiple protocols (NFS, S3, etc.) and formats and the converters to get a particular file format into working memory.

**write external** - yes/no - Ability to write working memory contents out to storage NOT in/under (managed by) the OMERO server.  This may include multiple protocols (NFS, S3, etc.) and file formats.  This format could initially just be a serialized layout of the current working memory.

**copies** - Number of copies of a 'file' 'managed' by OMERO.  If a file written to external storage is not also referenced by OMERO... then it may not be considered in this count.

## cases

| case | write internal | read external | write external | copies | description | useful |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | no  | no  | no  | 0 | doesn't make sense | no |
| 1 | no  | no  | yes | 1 | export-only | for migration? |
| 2 | no  | yes | no  | 1 | read-only | no |
| 3 | no  | yes | yes | 1 | omero is app-only | YES, the future |
| 4 | yes | no  | no  | 1 | today - internal only | YES, but fills up local storage|
| 5 | yes | no  | yes | 1 | today + save as button | YES, transition |
| 6 | yes | yes | no  | 1 | today + reading external | YES, transition |
| 7 | yes | yes | yes | 2 | ALL THE THINGS | YES, transition |

## analysis

Cases 0, 1, and 2 don't seem very practical.

A phased upgrade/rollout/development plan may look like...

- [4] -> [5 or 6] -> [7] -> [3]

If/when OMERO can write to an external iRODS storage location... iRODS could also 'register' that file or a descendent of that file back into OMERO for future reading.  This full circle would make OMERO appear to be managing the storage, but it would not be.
