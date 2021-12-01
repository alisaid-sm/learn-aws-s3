# Learn Cloud Object Storage AWS S3

### Reference

- https://gist.github.com/nyx-code/c1c07dc7830cd596c7a1b48d0dd5d15f
- https://youtu.be/TtuCCfren_I
- https://rajputankit22.medium.com/read-write-and-delete-file-from-s3-bucket-via-nodejs-2e17047d2178

### Prerequisite

- AWS Account

### Flow

1. Create simple API POST file using express & multer
2. install aws-sdk ```npm install aws-sdk --save```
3. Create src/utils/s3.js
```
const AWS = require('aws-sdk')

const s3 = new AWS.S3({
  accessKeyId: IAM_USER_KEY,  /* required */ # Put your iam user key
  secretAccessKey: IAM_USER_SECRET, /* required */  # Put your iam user secret key
  Bucket: BUCKET_NAME     /* required */     # Put your bucket name
});

#This function for upload file to s3 bucket
const s3upload = function (params) {
    return new Promise((resolve, reject) => {
        s3.createBucket({
            Bucket: BUCKET_NAME        /* Put your bucket name */
        }, function () {
            s3.putObject(params, function (err, data) {
                if (err) {
                    reject(err)
                } else {
                     console.log("Successfully uploaded data to bucket");
                    resolve(data);
                }
            });
        });
    });
}

# This function for read/download file from s3 bucket
const s3download = function (params) {
    return new Promise((resolve, reject) => {
        s3.createBucket({
            Bucket: BUCKET_NAME        /* Put your bucket name */
        }, function () {
            s3.getObject(params, function (err, data) {
                if (err) {
                    reject(err);
                } else {
                    console.log("Successfully dowloaded data from  bucket");
                    resolve(data);
                }
            });
        });
    });
}

# This function for delete file from s3 bucket
const s3delete = function (params) {
    return new Promise((resolve, reject) => {
        s3.createBucket({
            Bucket: BUCKET_NAME        /* Put your bucket name */
        }, function () {
            s3.deleteObject(params, function (err, data) {
                if (err) console.log(err);
                else
                    console.log(
                        "Successfully deleted file from bucket";
                    );
                console.log(data);
            });
        });
    });
};

module.exports = {s3upload, s3download, s3delete};
```
4. and you can ```const {s3upload,s3download, s3delete} = require('src/utils/s3.js')``` in your controller that allows upload, read, and delete files
```
const params = {
    Bucket: bucketName, // Bucket into which you want to upload file
    Key: keyName, // Name by which you want to save it
    Body: file // Local file 
};
await s3upload(params)

const params = {
    Bucket: bucketName, // Bucket into which you want to upload file
    Key: keyName, // Name by which you want to save it
};
await s3download(params)

const params = {
    Bucket: bucketName, // Bucket into which you want to upload file
    Key: keyName, // Name by which you want to save it
};
await s3delete(params)
```
