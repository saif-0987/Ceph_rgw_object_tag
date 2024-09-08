## Object tagging Ceph RGW using CLI

### Pre-requisites:
Before you begin, ensure you have the following tools installed:
- `s3cmd`
- `awscli`

## Create Bucket:
To create a new bucket, use the following command:
```
root@client-01:~# s3cmd mb s3://my-test-bucket
```

## Upload Objects into the bucket
Upload your objects to the newly created bucket using:
```
root@client-01:~# s3cmd put file-01.txt s3://my-test-bucket
upload: 'file-01.txt' -> 's3://my-test-bucket/file-01.txt'  [1 of 1]
1048576 of 1048576   100% in    0s    24.81 MB/s  done
```
Substitute path/to/your/file with the path to your local file and `your-bucket-name` with the name of your bucket.

## Ensure no tag is assigned to the objects
```
root@client-01:~# aws s3api get-object-tagging --bucket my-test-bucket --key file-01.txt --endpoint-url http://192.168.99.132:8001
{
    "TagSet": []
}
```
Replace `your-bucket-name` with your bucket name and `your-object-key` with the key of the object.

## Assign tag to objects
To assign a tag to an object, use the following command:
```
root@client-01:~# aws s3api put-object-tagging  --bucket my-test-bucket  --key file-01.txt  --tagging '{"TagSet": [{ "Key": "designation", "Value": "confidential" }]}' --endpoint-url http://192.168.99.132:8001
```

## Verify that objects is now tagged 
To verify that the object has been tagged, use:
```
root@client-01:~# aws s3api get-object-tagging --bucket my-test-bucket --key file-01.txt --endpoint-url http://192.168.99.132:8001

{
    "TagSet": [
        {
            "Key": "designation",
            "Value": "confidential"
        }
    ]
}
```
This command will display the tags associated with the object.
