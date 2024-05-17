# Anti-Patterns

In this lab we are going to revisit the simple NiFi flow we set up during the introduction to Nifi lab. During the lab we utilized a simple `Get...` 
processor to retrieve files from our source, but it turns out that this may not be the best retrieval method, and may be our first encounter with an 
[anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern) when building a NiFi flow because it doesn't allow for the full utilization of NiFi's 
capabilities and may in fact be counter productive to our purposes.

We hinted at the first problem during the last lab when we noted that the GetFile processor will either delete our source file (the default behavior), 
or leave it alone. If we're not careful here we could unintentionally remove the source file, or, if NiFi does not have the permission to delete the 
origin file, our processor will throw an error and not function at all. Additionally, as we'll see when configuring our next flow, the GetFile processor 
doesn't offer some of the finer grained control of the flow that we'll get when utilizing other processors.

The second problem with utilizing the `Get` processor is that it does not allow us to take advantage of NiFi in clusterd mode. As we noted during our 
introduction to NiFi, when running as a cluster, the nodes do not spread the work of a single FlowFile across the nodes, but instead handles a unique FlowFile 
on each node. In the case of the GetFile processor, this means that, at best we have nodes that are needlesly idle and at worst we create other conflicts in 
the flow.

During this lab we are going to update the flow we created during the introduction to allow us better utilize NiFi's capabilities.

1) Open the process group we created in the introductory lab, and, if our processors are still set to run, stop them
2) Add a new processor to the canvas called `ListFile`
3) Add a new processor to the canvas calld `FetchFile`
4) Add three funnels to the canvas that we will utilize as placeholders for `FetchFile` relationships we do not yet have a plan for
5) Right click on the `PutFile` processor we configured during the first lab, select the copy option, and paste a copy of the processor below the processors 
for our new flow. (This will allow us to easily utilize the same configurations for this processor we set up previously) 
6) The `FetchFile` processor has three different ways for handling when things go wrong. We'll look at this more in depth when we cover basic error handling 
in another lab. For now, terminate each of these relationships in a different funnel.
7) Create a success relationship between `ListFile` and `FetchFile`
8) Create a success relationship between `FetchFile` and `PutFile`

Now that we're a little more comfortable with the NiFi UI, as we configure the flow, let's take a look at some of the other basic features available.

9) Right click on the `ListFile` processor, and before configuring it select the 'View Usage' option. This option is available for all processors and will open 
the documentation for that specific processor.
10) Right click on the success relationship between ListFile and FetchFile and switch to the settings tab. We will not be adjusting these setting during this lab, 
but here is one of the areas where we can control how the flow behaves. 

Here, if we were running NiFi in a cluster we would be able to make use of the `Load Balance Strategy` setting because of the way the ListFile processor works. By default, the GetFile processor is designed to retrieve and dlete each file from the source and move it through the system. Meanwhile, the ListFile processor will create a different 
FlowFile for each file in the origin and allow us to distribute the files across the nodes in our cluster for further processing. 

As noted in the introduction to Nifi, the relationships between the processors is where we are able to tune things like back pressure and prioritizers for more control 
over the flow.

11) Configure the remainder of the flow, taking the time to read the documentation for each processor
12) As we did in the previous lab, start each processor one by one so that we can see the file move through each step of the flow.
13) Once the flow has been tuned and completes successfully, remember to update the flow version!

We've just implemented the first basic building block of a NiFi flow that can be used in production. The List/Fetch processor combo allows for better control of the file movement in the flow as well as better resource utilization and should be used whenever List/Fetch processors are available.

### Bonus

1) Review the following tutorial on the [List/Fetch pattern](https://www.youtube.com/watch?v=7mbxJxjGj3w), making note of the additional pattern that makes use of a `RecordWriter` to provide even better control of the flow. Update your flow to make use of the RecordWriter.  
2) Review the [videos](https://www.youtube.com/watch?v=RjWstt7nRVY) on anti-patterns in NiFi from one of its co-creators.  
3) Review the 'Listing Strategy' on the ListFile processor and learn the different listing strategy methods and how they are implemented.