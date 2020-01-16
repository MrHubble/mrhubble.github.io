---
id: 79
title: Uploading to S3 in React Native with progress updates
date: 2018-03-07T23:22:27+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=79
permalink: /2018/03/07/uploading-s3-react-native-progress-updates/
categories:
  - React Native
---
The official approach for using AWS with React Native is to use the [Javascript SDK](https://github.com/aws/aws-sdk-js) or the [AWS Amplify library](https://github.com/aws/aws-amplify):

<img src="https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-08-at-10.07.36-am.png" alt="" width="906" height="133" class="alignnone size-full wp-image-80" srcset="https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-08-at-10.07.36-am.png 906w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-08-at-10.07.36-am-300x44.png 300w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-08-at-10.07.36-am-768x113.png 768w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-08-at-10.07.36-am-700x103.png 700w" sizes="(max-width: 906px) 100vw, 906px" /> 

<https://github.com/awslabs/aws-sdk-react-native>

However, neither library will report the progress of a file being uploaded to S3:

> for performance reasons, the SDK does not split string or Buffer uploads into chunks just to track progress in Node. The SDK tracks upload progress in browsers (using XmlHttpRequest&#8217;s progress event) and when uploading a stream in Node (by tracking how much of the input stream has been read by Node&#8217;s underlying HTTP client).

<https://github.com/aws/aws-sdk-js/issues/1323#issuecomment-371301068>

For now we can use [react-native-aws3](https://github.com/benjreinhart/react-native-aws3) to get upload progress even though the project is [no longer maintained](https://github.com/benjreinhart/react-native-aws3/issues/53).

Hopefully in the future [Amplify adds progress updates](https://github.com/aws/aws-amplify/issues/334) and React Native [supports the progress events](https://github.com/aws/aws-sdk-js/issues/1805#issuecomment-345331254).

## For reference, here is some example code that would give progress in the browser:

    var params = {
      Bucket: "myBucket",
      Key: myFileName,
      Body: file,
      ContentType: 'image/png'
    }
    
    const request = S3.putObject(params);
    request.on('httpUploadProgress', function (progress) {
      console.log(progress.loaded + " of " + progress.total + " bytes");
    });
    request.send();
    

or

    const req = S3.upload(params).on('httpUploadProgress', function (progress) {
      console.log(progress);
    }).send(function (error, data) {
      console.log("FINISHED");
      if (error)
        console.log(error)
      else{
        const url = S3.getSignedUrl('getObject', { Key: params.Key });
        console.log(url)
      }
    });
    }