# About agent status<a name="metrics-agent-status"></a>

Agents have a status\. It's manually set in the Contact Control Panel \(CCP\)\. 
+ When they're ready to handle contacts, they set their status in the CCP to **Available**\. This means inbound contacts can be routed to them\.
+ When agents want to stop taking inbound contacts, they set their status to a custom status that you create, such as **Break** or **Training**\. They can also change their status to **Offline**\.

**Tip**  
Supervisors can manually [change the agent's status in the real\-time metrics report](rtm-change-agent-activity-state.md)\. 

The following diagram illustrates how the agent's status in the CCP stays constant while they are handling contacts, but in the real\-time metrics report, the **Agent activity state** and the **Contact state** change\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/connect/latest/adminguide/images/tutorial1-acw-contactstate.png)

For example, when the **Agent activity state** = **Incoming**, the **Contact state** = **Incoming contact**\.

## About custom agent statuses<a name="custom-agent-status"></a>

It's possible for agents to make outbound calls when their status in the CCP is set to a custom status\. Technically, agents can make an outbound call when their CCP is set to **Offline**\. 

For example, an agent wants to make an outbound call to a contact\. Because they don't want contacts to be routed to them during this time, they set their status to a custom status\. So when you look at your real\-time metrics report, you'll see the agent is simultaneously on **NPT** \(the metric that indicates a custom status\) and **On contact**, for example\.

## About ACW \(After contact work\)<a name="agent-status-acw"></a>

After a conversation between an agent and customer ends, the contact is moved into the ACW state\.

When the agent finishes doing ACW for the contact, they click **Clear** to clear that slot so another contact can be routed to them\.

To identify how long an agent spent on ACW for a contact:
+ In the historical metrics report, **After contact work time** captures the amount of time each contact spent in ACW\.
+ In the agent event stream, you have to do some calculations\. For more information, see [Determine how long an agent spends doing ACW](determine-acw-time.md)\.

## How do you know when an agent can handle another contact?<a name="agent-availability"></a>

The **Availability** metric tells you when agents are finished with a contact and ready to have another one routed to them\.

## What appears in the real\-time metrics report?<a name="agent-status-rtm.title"></a>

To find out what the agent status is in the real\-time metrics report, look at the **Agent Activity** metric\.

## What appears in the agent event stream?<a name="agent-status-in-agent-event-stream"></a>

In the agent event stream you'll see the **AgentStatus**, for example: 

```
{
 "AWSAccountId": "012345678901",
 "AgentARN": "arn:aws:connect:us-east-1:012345678901:instance/aaaaaaaa-bbbb-cccc-dddd-111111111111/agent/agent-ARN",
  "CurrentAgentSnapshot": {
      "AgentStatus": {   //Here's the agent's status that they set in the CCP.  
          "ARN": "arn:aws:connect:us-east-1:012345678901:instance/aaaaaaaa-bbbb-cccc-dddd-111111111111/agent-state/agent-state-ARN",
          "Name": "Available",  //When an agent sets their status to "Available" it means they are ready for
                                                      // inbound contacts to be routed to them, and not say, at Lunch.  
          "StartTimestamp": "2019-05-25T18:43:59.049Z"
      },
```

## "We couldn't find this agent\. Use the agent's user name to identify them\."<a name="agent-status-cannot-find-agent"></a>

On occassion, in the **Contact summary** the **Agent** field may say **"We couldn't find this agent\. Use the agent's user name to identify them\."**

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/connect/latest/adminguide/images/agent-status-not-found.png)

This is a generic message for contacts that did not get connected to an agent at the time\. It usually means that the inbound call was not answered by the agent and the customer disconnected the call\. 

To confirm that the caller was never connected to an agent:
+ **Disconnect reason** = **Customer disconnect**\.
+ No recording of the call is found for that contact ID\.

To verify this behavior, call your contact center and disconnect after a period of time without an agent accepting the call\.