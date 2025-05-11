# Automation of Barkibu's Analysis Process

## Problem Context:

We aim to integrate automation into the workflow of Barkibu’s mobile application in order to reduce the workload on our team of human specialists. To understand where automation could be implemented, it's important to remember the usual workflow of the app:

-   Customers who already have an insurance policy upload their documents related to a veterinary visit to the app. These documents may include the vet bill, the vet’s report to verify which parts of the visit may be covered by the insurance, and the pet's medical history.

-   Barkibu’s team of specialists analyzes the documents to determine which parts of the visit are covered by the insurance, whether the condition may be affected by pre-existing conditions, and how much should be reimbursed to the customer.

## Points Where Process Automation Could Be Implemented

After analyzing the problem, i believe there are three main areas where automation could be introduced:

1. Fully automating the entire process in the app, allowing the system to analyze documents, perform the analysis normally done by specialists, and present the results directly to the user.
2. Automating the reading and analysis of the documents, enabling the system to summarize the data in a simplified and more readable way for the specialist team, who would then make decisions about reimbursement amounts.
3. Having the system perform a complete analysis and estimate all relevant factors, but instead of presenting it to the user directly, the results are shown to the specialist team for verification before being finalized and shared with the user.

## Recommendation on Which Path to Follow:

In my opinion, the third option is the one we should implement. I'll start by explaining why I believe the other two options wouldn’t effectively address our main issue, which is reducing the specialists’ workload.

Starting with the first option, while it’s true we could save a lot of user response time and initially reduce the workload by having the AI do the analysis, it’s very likely that a significant portion of users would not trust the app's analysis and would request a review by the human team out of fear of AI error. This would not ease the specialists' workload and could require starting the process over.

The second option could be a good solution since it might simplify part of the specialists' task by removing the most time-consuming part—document analysis. If this is indeed the most labor-intensive part, then it could be a fully satisfactory solution. However, I believe the third option could be more effective.

The third option, developing a data analyst that performs a preliminary document analysis and generates estimates, which the specialists can then review, seems the most appropriate for our process.

## Detailed Explanation of the Suggested Solution:

Automating the initial process but ensuring the final results are verified before being presented to the user gives us the most advantages.

First of all, in terms of reducing the specialists' workload, this approach decreases the time they spend on each individual case, as they would only need to review the system’s analysis. Of course, they would still have the final say in each case, with the ability to review everything from scratch when necessary, such as in the case of errors or special circumstances. But once the system is trained to analyze effectively with acceptable quality, the team would spend significantly less time on each case.

Regarding scalability, since we can train the system using documents and parameters that matter to us (e.g., details of insurance policies and covered conditions), we can easily integrate new relevant parameters into the system.

Another important point is the user interaction with the new system. While it might not provide instant analysis like the first option could, it would still reduce response times significantly by eliminating the most time-consuming step, the manual data review by the specialist team. Furthermore, users are more likely to trust an analysis that has been approved by a human expert rather than one done solely by an unsupervised AI, improving the overall user experience and likelihood of continued engagement.

## Downsides of the Chosen Solution:

While this option has several positives, it also comes with a few drawbacks:

### Time Required to Train the Model to Our Needs:

To produce useful analysis, we’ll need to go through a process of training the analyst model using our documentation, relevant context details (such as available insurance policies and their limitations), and invest time curating the data to ensure the model’s output is genuinely helpful. This time-intensive process is the main drawback, as the system won’t be actively assisting during implementation and training.

### Potential Inaccuracies Due to Poor-Quality Documents:

If a client submits low-quality documents (e.g., mobile phone photos), the system might fail to analyze them correctly. This can be mitigated by requesting higher-quality documents from the user, or by adding a pre-processing step to assess document quality before proceeding.

## Implementation Plan

The necessary structure consists of the following:

-   The modified Barkibu mobile app
-   A server that receives the user's request, categorizes it, and performs an initial analysis using a language model
-   A portal where our specialist team can view the request, the model’s initial analysis, and if necessary, carry out a full review using the submitted documents

Starting with Barkibu's mobile app, we’d only need to modify it so that it sends data to our analysis server instead of directly to the specialist review server. I'm not sure whether the response is currently delivered to the user via email or the app, but for this example, let’s assume it's via the app, in which case we should add functionality to receive the response there as well.

On the analysis server, I propose we first process the documents to extract text data instead of dealing with PDFs, and then implement a language model trained on our context (insurance policies and relevant parameters) and real-world document types (bills, reports, medical histories). The specific model we use will depend on several factors, but QWEN’s latest version is a strong starting point.

The portal for the specialist team could be a web-based tool similar to Jira, allowing them to view requests in real-time, update and manage them, and track their status. This can be built by linking the analysis server’s output to the request database and parsing the data into the portal’s required format. This setup also helps in maintaining categorized requests for auditing and archiving purposes.

## Additional Use Cases for the Data Architecture:

Training a language model to analyze client-submitted documents also opens the door to additional functionalities, such as:

-   Creating a policy recommender based on the pet’s profile, suggesting plans according to their history, vet analysis, etc.
-   Implementing a pet profile in the app that sends personalized reminders and alerts to the user
-   Based on the pet profile, recommending relevant content (articles, videos, etc.) aligned with the pet’s needs

All of these use cases would benefit from the foundational work of training and refining the model, and continued training with new data would keep improving its accuracy and reliability.
