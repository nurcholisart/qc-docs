# How can I get Unread Count in Qiscus SDK?

Currently, Qiscus SDK does not provide the method to get total unread count [getTotalUnreadCount()](https://documentation.qiscus.com/chat-sdk-javascript/chat-room#get-total-unread-count-in-chat-room) anymore, but you can do this easily by following these steps:

## 1. Create a Table to Hold the Data

You’ll need to create a new table to hold the unread count consisting of `user_id, room_id, unread_count`. You can use any database of your choice for this.

## 2. Create a Webhook on Qiscus SDK

You also need to create a webhook to listen to post comments events. You can refer to this [documentation](https://documentation.qiscus.com/chat-server-api/webhook). We have two types of webhook, namely `on client post comment` and `on rest post comment`. You can use `on rest post comment` type if you’re sending a message using API from this [page](https://documentation.qiscus.com/chat-server-api/message). If you don’t then you can safely assume you’ll be using `on client post comment` webhook.  

## 3. Increasing the Unread Count

When you have setup your database table and webhook you’re ready to implement the unread count feature. When any SDK user sends a message, the webhook that we have setup before will be triggered. When it is triggered you will get a payload like this:

```json
{
  "payload": {
    "from": {
      "avatar_url": "https://d1edrlpyc25xu0.cloudfront.net/kiwari-prod/image/upload/75r6s_jOHa/1507541871-avatar-mine.png",
      "email": "Fikri",
      "id": 1427537635,
      "id_str": "1427537635",
      "name": "fikri@qiscus.com"
    },
    "message": {
      "comment_before_id": 1109884209,
      "comment_before_id_str": "1109884209",
      "created_at": "2022-10-04T03:53:08.089051Z",
      "disable_link_preview": false,
      "extras": {},
      "id": 1109885152,
      "id_str": "1109885152",
      "payload": {},
      "text": "lagi",
      "timestamp": "2022-10-04T03:53:08Z",
      "type": "text",
      "unique_temp_id": "bq1664855587121",
      "unix_nano_timestamp": 1664855588089051000,
      "unix_timestamp": 1664855588
    },
    "room": {
      "id": 93712301,
      "id_str": "93712301",
      "is_public_channel": false,
      "name": "QISCUS demo user",
      "options": "{}",
      "participants": [
        {
          "active": true,
          "avatar_url": "https://d1edrlpyc25xu0.cloudfront.net/kiwari-prod/image/upload/75r6s_jOHa/1507541871-avatar-mine.png",
          "email": "Fikri",
          "extras": {},
          "id": 1427537635,
          "id_str": "1427537635",
          "last_comment_read_id": 1109884209,
          "last_comment_read_id_str": "1109884209",
          "last_comment_received_id": 1109884209,
          "last_comment_received_id_str": "1109884209",
          "username": "fikri@qiscus.com"
        },
        {
          "active": true,
          "avatar_url": "https://image.url/image.jpeg",
          "email": "guest@qiscus.com",
          "extras": {},
          "id": 176523,
          "id_str": "176523",
          "last_comment_read_id": 0,
          "last_comment_read_id_str": "0",
          "last_comment_received_id": 1109884209,
          "last_comment_received_id_str": "1109884209",
          "username": "QISCUS demo user"
        }
      ],
      "room_avatar": "https://image.url/image.jpeg",
      "topic_id": 93712301,
      "topic_id_str": "93712301",
      "type": "single"
    }
  },
  "type": "post_comment_mobile"
}
```

From the payload above you can see that we have access to the `room.room_id`, `from.email` for the user_id that is sending the message and you can also get other participants from `room.participants`. You can loop through the participant and increase their unread count. Something like this:

```javascript
room.participants.forEach((participant) => {
  // do nothing if the user id is the same with the one sending the message
  if (participant.email === from.email) return;

  // Call the API to increase this user id unread count
  API.increaseUnreadCount(participant.email);
});
```

## 4. Removing the Unread Count

We’ll also need a method to remove the unread count whenever a user has already read all the messages. We can put this method after a user opening a room or sending a message. The code will be something like this:

```javascript
// Triggered when sending a message
qiscus.sendMessage({}).then((res) => {
  API.resetUnreadCount(user_id);
});

// Triggered when opening a chat room
qiscus.getRoomById(roomId).then((res) => {
  API.resetUnreadCount(user_id);
});

/*  
the API.resetUnreadCount(user_id) above will trigger something like this in the database  
UPDATE tbl_unread_count SET unread_count=0 WHERE user_id='$user_id'  
*/

```


