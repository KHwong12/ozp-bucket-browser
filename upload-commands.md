

Ref: https://adamtheautomator.com/upload-file-to-s3/#Uploading_Multiple_Files_and_Folders_to_S3_Recursively

Every time a new plan is gazetted, do the following:

1. Copy the updated index page (`index.html` generated from ozp-downloader scripts)
1. Copy the new OZP data to *by-plan* folder
1. Copy the generated new territorial plan to *territorial-merged* folder
1. Remove the `Data Dictionary.pdf`, `Guidelines - Digital Planning Data - Chi.pdf` and other files automatically attached to zoning zip file. e.g.:
    ```
    find . -name 'Guidelines - Digital Planning Data - Chi.pdf' -delete
    ```
    
1. Sync:
  1. index.html
    ```
    aws s3 cp index.html s3://ozpmerged/
    ```
  1. New plans
    ```sh
    aws s3 sync ./by-plan s3://ozpmerged/by-plan \
      --exclude *.DS_Store \
      --dryrun
    ```
  1. New territorial plans
    ```sh
    aws s3 sync ./territorial-merged s3://ozpmerged/territorial-merged \
      --exclude *.DS_Store \
      --dryrun
    ```

---

Update index webpage

```sh
aws s3 cp index.html s3://ozpmerged/   
```

Upload territorial merged folder

```sh
aws s3 cp ./territorial-merged s3://ozpmerged/territorial-merged --recursive \
  --exclude *.DS_Store \
  --dryrun
```

Test Update with new files only? (`cp`) copy all files 

```sh
aws s3 sync ./territorial-merged s3://ozpmerged/territorial-merged \
  --exclude *.DS_Store \
  --dryrun
```

## For by-plan, use sync as local copy is kept?

```sh
aws s3 sync ./by-plan s3://ozpmerged/by-plan \
  --exclude *.DS_Store \
  --dryrun
```


Update with new files only (`cp`) copy all files

```sh
aws s3 sync ./territorial-merged s3://ozpmerged/territorial-merged \
  --exclude *.DS_Store
```

List log files

```sh
aws s3 ls s3://ozpmerged-log
```

Download log files 

```sh
aws s3 cp s3://ozpmerged-log ./log --recursive
```
