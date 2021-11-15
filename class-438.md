# Notifications
## Getting started with Amazon SNS

![awasns](https://learningtofly.dev/media/SNS_Flows.png)
* #### Step 1: Create a topic
    - Sign in to the Amazon SNS console.

    - In the left navigation pane, choose Topics.

    - On the Topics page, choose Create topic.

    - By default, the console creates a FIFO topic. Choose Standard.

    - In the Details section, enter a Name for the topic, such as MyTopic.

    - Scroll to the end of the form and choose Create topic.

    - The console opens the new topic's Details page.

* #### Step 2: Create a subscription to the topic
    - In the left navigation pane, choose Subscriptions.

    - On the Subscriptions page, choose Create subscription.

    - On the Create subscription page, choose the Topic ARN field to see a list of the topics in your AWS account.

    - Choose the topic that you created in the previous step.

    - For Protocol, choose Email.

    - For Endpoint, enter an email address that can receive notifications.

    - Choose Create subscription.

    - The console opens the new subscription's Details page.

    - Check your email inbox and choose Confirm subscription in the email from AWS Notifications. The sender ID is usually "no-reply@sns.amazonaws.com".

    - Amazon SNS opens your web browser and displays a subscription confirmation with your subscription ID.

* #### Step 3: Publish a message to the topic
    - In the left navigation pane, choose Topics.

    - On the Topics page, choose the topic that you created earlier, and then choose Publish message.

    - The console opens the Publish message to topic page.

   -  (Optional) In the Message details section, enter a Subject, such as:

    - Hello from Amazon SNS!
    In the Message body section, choose Identical payload for all delivery protocols, and then enter a message body, such as:

    - Publishing a message to an SNS topic.
    Choose Publish message.

    - The message is published to the topic, and the console opens the topic's Details page.

    - Check your email inbox and verify that you received an email from Amazon SNS with the published message.

* #### Step 4: Delete the subscription and topic
     - On the navigation panel, choose Subscriptions.

     - On the Subscriptions page, choose a confirmed subscription and then choose Delete.

     - In the Delete subscription dialog box, choose Delete.

     - The subscription is deleted.

     - On the navigation panel, choose Topics.

     - On the Topics page, choose a topic and then choose Delete.

    -  On the Delete topic MyTopic dialog box, enter delete me and then choose Delete.

     - The topic is deleted.