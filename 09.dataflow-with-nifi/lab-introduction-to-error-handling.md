# First Basic Error Handling Flow

In previous labs we have built basic flows and improved those flows to better utilize the capabilities of NiFi. Let's start to think about some strategies for handling when things go wrong.

1) Open the flow that we've been working with that should now be at its second version. 
2) Review the "failure" relationships we created for the FetchFile processor by reading over the Relationships tab. Note here that relationships in the newest version of NiFi have at least two options available in addition to sending the relationship to another processor, 'terminate' and 'retry'. Mouse over the `?` to get a better feel on how the Terminate/Retry options work.

Here we can also see that this processor has three built in expected failures (`not.found, permission.denied, failure`). We can begin to imagine scenarios where the processor might fail. For the purposes of this lab, we are going to assume that failures at this point are unlikely, but when this processor encounters a failure we want to route it to the same person/team. In the next steps we'll look at the two ways we can combine our three different relationships.

3) Create two new funnels
4) Create a relationship between our three existing failure funnels and one of the new funnels we added to the canvas. Here, we are using the funnel for its intended use to combine different flows into a single new flow. (This begins to make more sense when we imagine that these relationships were coming from other processors or process groups and not our placeholders)
5) Create a relationship directly from FetchFile processor to the second funnel we added to the canvas. Here we will select the settings that allow us to route all three failure options to the same place through a single relationship. (This is the option we are more likely to use if we're not trying to quickly debug a new flow and/or we already know that we want failed files to be processed the same way at this point).

For now we're going to leave the failures at this point of the pipeline at placeholders so that we can move to a point in the pipeline where it is much easier to purposefully trigger an error to better illustrate a strategy for dealing with failures.

6) Right click on the ListFile processor and select the 'view state' option
7) Clear state. This will allow the ListFile processor to re-list the file already in our uploads directory.

Note: If there aren't currently any files in our upload directory, we can either create a new file in the directory, run the pipeline once (making sure that our pipeline strategy leaves source files intact and unchanged), and then follow the above steps to re-list the file, or we can add a file to the uploads directory that we know has already been processed through the pipeline and still exists in the downloads directory. 

Here we can note that we should have a failed file introduced into our flow. This happens because, by default, the `Put...` processors will 'fail' if a duplicate file has been introduced into the pipeline, providing an easy way to make sure that we aren't unintentionally duplicating files in our downstream systems. 

For now, we're going to imagine that this file had been routed to our chosen process or process group for dealing with failed files so that we can look at another interesting NiFi feature, the abiity to automatically trigger some type of notification to go out.

8) Add a new processor to the canvas, `PutEmail`
9) Look [here for the outlook settings](https://support.microsoft.com/en-gb/office/pop-imap-and-smtp-settings-for-outlook-com-d088b986-291d-42b8-9564-9c414e2aa040), or [here for the gmail settings](https://support.google.com/a/answer/176600?hl=en), or otherwise find the settings for your given email provider for how to configure the PutEmail processor.
10) Configure the processor to send a notification to yourself or another colleague.
11) Start the processor and you should soon see a notification of a flow failure!

### Bonus

1) Imagine a routing strategy for duplicate files such as routing to a central failed file directory or automatic deletion. Replace one of our 'place holder' funnels with this strategy.
2) Learn more about process groups [here](https://community.cloudera.com/t5/Community-Articles/NiFi-Understanding-how-to-use-Process-Groups-and-Remote/ta-p/245486) and reorganize the canvas to make use of process goups.