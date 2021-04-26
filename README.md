# Twilio Flex Web Chat UI

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

You can find the most recent version of the guide on how to perform common tasks [here](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md).

This package can only be consumed together with Twilio Flex. Visit http://twilio.com/flex to find out more.

## Instructions

1. Install all dependencies by running:
```
npm install
```

2. Copy webchat-appConfig.sample.js in public/assets folder and configure accordingly to use your Twilio account
```
cp public/assets/webchat-appConfig.sample.js public/assets/webchat-appConfig.js
```

3. Start Flex UI by running:
```
npm start
```

# Consumption

There are 2 ways to consume Flex WebChat, from CDN or npm. 

For CDN there are 2 options:

1. Add the following script tag to the index.html head tag to include WebChat bundle from CDN.
```
<script src="https://assets.flex.twilio.com/releases/flex-webchat-ui/2.8.1/twilio-flex-webchat.min.js" integrity="sha512-KzpB56iRohSbDOkfM/V0PTp9DGHMno2EJJx6Zg8Ul3byOV3xtAurIZ+NibcO+cc0SEDvodI/5SKMSv2p+gwSYw==" crossorigin></script>
```
Then add the following script tag to the index.html body tag to initialize WebChat.
```
 <script>
    const appConfig = {
        accountSid:"AC...",
        flexFlowSid:"FO..."
    };
    Twilio.FlexWebChat.renderWebChat(appConfig);
  </script>
```

2. The following steps will allow you to customize the FlexWebChat initialization flow before you render the WebChat, as it consists of two parts: initialize and render. This option allows you to tap into customization before the render method executes.
```
Twilio.FlexWebChat.createWebChat(appConfig).then(webchat => {
    const { manager } = webchat;
    
//Posting question from preengagement form as users first chat message
    Twilio.FlexWebChat.Actions.on("afterStartEngagement", (payload) => {
        const { question } = payload.formData;
        if (!question)
            return;

        const { channelSid } = manager.store.getState().flex.session;
        manager
            .chatClient.getChannelBySid(channelSid)
            .then(channel => channel.sendMessage(question));
    });
// Changing the Welcome message
    manager.strings.WelcomeMessage = "New text for welcome message";

// Render WebChat
    webchat.init();
  });
```

# Links

For more information, please visit: 
- https://www.twilio.com/docs/flex/developer/webchat/setup
- https://media.twiliocdn.com/sdk/js/chat/releases/3.4.0/docs/