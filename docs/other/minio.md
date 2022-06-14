# Grant access to models bucket for my user

1. Get the policy from pravda-dev
```
pravda@pravda-dev:~$ mc admin policy info minio models
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Action": [
    "s3:*"
   ],
   "Resource": [
    "arn:aws:s3:::models",
    "arn:aws:s3:::models/*"
   ]
  }
 ]
}
```

2. Copy the output to a JSON file
3. Create the policy in my computer
```bash
mc admin policy add minio models models-policy.json
```
4. Add my user to models-bucket-masters group
```bash
mc admin group add minio models-bucket-masters haroune
```
 
5. Attribute the policy to the group
```bash
mc admin policy set minio models group=models-bucket-masters
```
