# How Do I Send Files or Images Using Qiscus In-App Chat SDK for iOS That Can Be Rendered on Built-In UI Version?

Since the beginning of 2019, we are no longer provide built-in chat UI, hence to those who are still processing on migration in newest Qiscus Chat SDK version, especially for sending file or image case,  you can follow these 2 steps.

## 1. Prepare the payload for sending a file or image

You need **payload** to send image or file, we assume you already have the **URL** after uploading the file based on this [docs.](https://documentation.qiscus.com/chat-sdk-ios/message#upload-file)

Then, preparing payload for sending the file or image, for example:

```json
{
  "url": "https://d1edrlpyc25xu0.cloudfront.net/sampleapp-65ghcsaysse/docs/upload/2sxErjgAfp/Android-Studio-Shortcuts-You-Need-the-Most-3.pdf",
  "caption": "",
  "file_name": "Android-Studio-Shortcuts-You-Need-the-Most.pdf"
}
```

and then construct Comment Model, set message type to be **file_attachment** and fill your payload in **payload**, for example:

```javascript
let message = CommentModel();

message.type = "file_attachment";
message.message = "[file]" + yourFileURL + "[/file]";
message.payload = [("url": yourFileURL), ("file_name": "your file name"), ("caption": "your caption")];

```

## 2. Send the file or image

After creating Comment model which has your payload, then you can pass your Comment model to this API, for example

```javascript
QiscusCore.shared.sendMessage(roomID: roomId, comment: message, onSuccess: { (commentModel) in
    //success sending file or image
}) { (error) in
    print(error.message)
}
```

## 3. Rendering the file or image

Since you are passing **type** and **payload,** you can filter which image or file that you need to render as a file attachment UI. Rendering the image or file can get from **URL** that you send, you can see detail in our [sample.](https://github.com/qiscus/qiscus-chat-sdk-ios-sample/blob/migration/Example/ChatView/UIChatViewController.swift#L361)
