## Install curl
```
sudo apt install curl
```
or
```
brew install curl
```

## Create Credentials on GCP

See PDF for more detail. Should be able to get <cliend_id> and <cliend_secret>

## Verify Device
```
curl -d "client_id=<cliend_id>&scope=https://www.googleapis.com/auth/drive.file" https://oauth2.googleapis.com/device/code
```

The result should look something like this:
```
{
  "device_code": "<device_code>",
  "user_code": "<user_code>",
  "expires_in": 1800,
  "interval": 5,
  "verification_url": "https://www.google.com/device"
}
```

Go to https://www.google.com/device for authentication using <user_code>

## Get Bearer Code
```
curl -d client_id=<client id> -d client_secret=<client secret> -d device_code=<device code> -d grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Adevice_code https://accounts.google.com/o/oauth2/token
```

The result should look something like this:
```
{
  "access_token": "<access_token>",
  "expires_in": 3599,
  "refresh_token": "1//05VqzLyxoCc2HCgYIARAAGAUSNwF-L9IrXwKWBYWO4eiu1APNtmoqHcpUREFNjLNx4coVE0mp8d3m8CQ_eQHVktTYeXnz8VLcrvo",
  "scope": "https://www.googleapis.com/auth/drive.file",
  "token_type": "Bearer"
}
```

If error, double check if authenticated.

## Upload Files

Have the file <file_name.zip> ready:

```
curl -X POST -L \
-H "Authorization: Bearer <access_token>" \
-F "metadata={name :'<file_name.zip>'};type=application/json;charset=UTF-8" \
-F "file=@<file_name.zip>;type=application/zip" \
"https://www.googleapis.com/upload/drive/v3/files?uploadType=multipart"
```

The result should look something like this:
```
{
  "kind": "drive#file",
  "id": "1wO-vrVPi0M2oQ71hIx9Vz4pcfxAFReRy",
  "name": "<file_name.zip>",
  "mimeType": "application/zip"
}
```

Now the file is uploaded to the main page of Google Drive.