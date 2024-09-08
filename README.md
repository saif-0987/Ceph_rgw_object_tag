## Object tagging Ceph RGW using CLI

### Pre-requisites:
- s3cmd
- awscli

## Create Bucket:
`root@client-01:~# s3cmd mb s3://my-test-bucket`

## Upload Objects into the bucket
`root@client-01:~# s3cmd put file-01.txt s3://my-test-bucket`

 `upload: 'file-01.txt' -> 's3://my-test-bucket/file-01.txt'  [1 of 1]
  1048576 of 1048576   100% in    0s    24.81 MB/s  done`

## Ensure no tag is assigned to the objects
```
root@client-01:~# aws s3api get-object-tagging --bucket my-test-bucket --key file-01.txt --endpoint-url http://192.168.99.132:8001
{
    "TagSet": []
}
```
## Assign tag on objects
`root@client-01:~# aws s3api put-object-tagging  --bucket my-test-bucket  --key file-01.txt  --tagging '{"TagSet": [{ "Key": "designation", "Value": "confidential" }]}' --endpoint-url http://192.168.99.132:8001`

## Verify that objects is now tagged 
`root@client-01:~# aws s3api get-object-tagging --bucket my-test-bucket --key file-01.txt --endpoint-url http://192.168.99.132:8001`

`{
    "TagSet": [
        {
            "Key": "designation",
            "Value": "confidential"
        }
    ]
 }`
