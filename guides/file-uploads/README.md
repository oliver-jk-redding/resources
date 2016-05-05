# How to upload files to Amazon S3


## Image Preview

Before you upload the image you might want to preview it in the browser - [jsfiddle](https://jsfiddle.net/6vxhyhjz/)
```html

<html>
  <head>
    function previewImage() {
      var preview = document.querySelector('img');
      var file    = document.querySelector('input[type=file]').files[0];
      var reader  = new FileReader();
      
      reader.addEventListener("load", function () {  
        preview.src = reader.result
      })
      
      if (file) reader.readAsDataURL(file);
    }
  </head>
  <body>
    <img />
    <form>
      <input type='file' onChange='previewImage()' />
    </form>
  </body>
</html>
```

## Amazon S3

Amazon Web Services (AWS) offer a lot of services for hosting websites. One of them is the [Amazon Simple Storage Service (S3)](https://aws.amazon.com/s3/) which is useful for storing images, videos and audio files.

[This guide from Amazon](https://aws.amazon.com/s3/getting-started/) has a good overview of how to sign up for an account and create your first bucket.

## Uploading files

The workflow for uploading a file is

1. User selects file in their browser
1. Browser sends a get request to your server `GET '/sign_s3'` to get a presigned url 
1. Browser uses the presigned url to send a put request to S3

Step 2 is important because S3 won't just let anybody upload a file and requires your AWS credentials to accept a request. Sending your AWS credentials down to the browser would be a bad idea though since a user could just read them and then start storing anything they want into your S3 account which could get very expensive. So Amazon have a nifty [getSignedUrl](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getSignedUrl-property) method which lets you generate a one-time url that will expire after use.

[This guide from Heroku](https://devcenter.heroku.com/articles/s3-upload-node) has a good overview of the process and [the code for the example app]([https://github.com/flyingsparx/NodeDirectUploader) is up on github.

You will need to have your AWS credentials loaded in the heroku configa - checkout [this guide on config vars](https://devcenter.heroku.com/articles/config-vars). For storing keys on your dev server you should put the credentials in your .env file and use [heroku local](https://devcenter.heroku.com/articles/heroku-local) or something like [dotenv](https://www.npmjs.com/package/dotenv)

## Resources
* [Getting started with Amazon S3](https://aws.amazon.com/s3/getting-started/)
* [Heroku guide](https://devcenter.heroku.com/articles/s3-upload-node) & [companion code]([https://github.com/flyingsparx/NodeDirectUploader)
* [Heroku config vars](https://devcenter.heroku.com/articles/config-vars)
* [AWS SDK for Node](https://aws.amazon.com/sdk-for-node-js/) & [S3 SDK](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html)
* [FileReader docs on MDN](https://developer.mozilla.org/en/docs/Web/API/FileReader)
* [XMLHttpRequest on MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
