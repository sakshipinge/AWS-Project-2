# Amazon S3-Based Document Storage System

## 1. Project Title

**Amazon S3-Based Document Storage System for an Organization**

## 2. Objective

The objective of this project is to create a secure and organized document storage system using Amazon S3.

The system stores organizational documents in folders, provides controlled access to the S3 bucket, enables versioning, and allows an earlier version of a file to be recovered when required.


## 3. AWS Services Used

The following AWS services and tools were used:

* **Amazon S3** – Used for storing and managing documents.
* **AWS Management Console** – Used to configure the S3 bucket.
* **AWS CLI** – Used to upload files, check versioning, list file versions, and recover an earlier version.

**AWS Region:** Asia Pacific (Mumbai) – `ap-south-1`

---

## 4. Architecture

The architecture of the document storage system is:


                 Organization User
                        |
                        |
                 AWS Management
                    Console / CLI
                        |
                        v
              Amazon S3 Bucket
     sakshi-storage-document-2026-123
                        |
                        v
                 student-data/
                  /          \
                 /            \
                v              v
            info.txt       server.txt
                |
                v
          S3 Versioning
                |
        -------------------
        |                 |
        v                 v
 Current Version    Previous Version
        |                 |
        |                 v
        |          Version Recovery
        |                 |
        --------> old-info-recovered.txt
```

---

## 5. Implementation Steps

### Step 1: Create the S3 Bucket

Created a unique Amazon S3 bucket named:


sakshi-storage-document-2026-123
```

The bucket was created in the Mumbai AWS Region:

ap-south-1

### Step 2: Create the Folder

Inside the S3 bucket, a folder named:

student-data
was created to organize the documents.

### Step 3: Create Local Files

Two sample files were created

info.txt
server.txt
The contents were:
**info.txt**
This is my cloud information.

**server.txt**
This is my server information.

### Step 4: Upload Files to S3
The files were uploaded to the `student-data` folder using AWS CLI.
Commands used:
aws s3 cp info.txt s3://sakshi-storage-document-2026-123/student-data/info.txt
aws s3 cp server.txt s3://sakshi-storage-document-2026-123/student-data/server.txt
The uploaded files were verified using:
aws s3 ls s3://sakshi-storage-document-2026-123/student-data/
## 6. Configuration
### Bucket Configuration

| Configuration   | Value                              |
| --------------- | ---------------------------------- |
| Bucket Name     | `sakshi-storage-document-2026-123` |
| Region          | `ap-south-1`                       |
| Storage Service | Amazon S3                          |
| Folder          | `student-data`                     |
| Files           | `info.txt`, `server.txt`           |
| Versioning      | Enabled                            |

### Versioning Configuration
Versioning was enabled on the bucket.

The configuration was verified using:

aws s3api get-bucket-versioning --bucket sakshi-storage-document-2026-123

The result was:
Status: Enabled
Versioning ensures that older versions of objects are retained when a file is modified or replaced.
## 7. Testing

The following tests were performed.

### Test 1: Verify Uploaded Files

Command:
aws s3 ls s3://sakshi-storage-document-2026-123/student-data/
Result:
The `info.txt` and `server.txt` files were successfully displayed.
**Result: PASS**
### Test 2: Verify Versioning
Command:

aws s3api get-bucket-versioning --bucket sakshi-storage-document-2026-123
Result:

Status: Enabled
**Result: PASS**
### Test 3: Create a New File Version
The original `info.txt` was modified.
Updated content:

This is my UPDATED cloud information.
The updated file was uploaded again:
aws s3 cp info.txt s3://sakshi-storage-document-2026-123/student-data/info.txt
Because versioning was enabled, Amazon S3 stored the updated file as a new version instead of permanently deleting the previous version.

**Result: PASS**
### Test 4: List Object Versions

Command:

aws s3api list-object-versions \
--bucket sakshi-storage-document-2026-123 \
--prefix student-data/info.txt
The command displayed multiple versions of `info.txt`.

**Result: PASS**

## 8. Version Recovery

The previous version of `info.txt` was recovered using its Version ID.

The previous Version ID was identified from the `list-object-versions` command.

The recovery command used was:

aws s3api get-object \
--bucket sakshi-storage-document-2026-123 \
--key student-data/info.txt \
--version-id '4.Qqk0E32Gt3.Fx5Hyx8hF4ZOtCUmK4.' \
old-info-recovered.txt
The recovered file was checked using:

cat old-info-recovered.txt
The recovered content was:
This is my cloud information.
The current version contained:
This is my UPDATED cloud information.
This confirms that an earlier version of the document was successfully recovered.

**Result: PASS**


## 9. Testing Results Summary

| Test                | Expected Result             | Actual Result                           | Status |
| ------------------- | --------------------------- | --------------------------------------- | ------ |
| Create S3 bucket    | Bucket created              | Bucket created successfully             | PASS   |
| Create folder       | `student-data` created      | Folder created successfully             | PASS   |
| Upload documents    | Files uploaded              | `info.txt` and `server.txt` uploaded    | PASS   |
| Enable versioning   | Versioning enabled          | Versioning enabled                      | PASS   |
| Create new version  | New version stored          | New version created                     | PASS   |
| List versions       | Multiple versions displayed | Multiple versions displayed             | PASS   |
| Recover old version | Previous file recovered     | Previous version recovered successfully | PASS   |

---

## 10. Result / Conclusion

The Amazon S3-based document storage system was successfully implemented.

The project demonstrates how Amazon S3 can be used to store and organize documents using folders. Files were uploaded successfully and bucket versioning was enabled to protect previous versions of documents.

An updated version of `info.txt` was created, and the earlier version was successfully identified and recovered using its Version ID.

Therefore, the project successfully demonstrates **organized document storage, version control, and recovery using Amazon S3**.
