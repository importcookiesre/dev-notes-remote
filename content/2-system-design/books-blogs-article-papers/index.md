


# How to read ?

1. **Understand the chapter title and problem :**
	1. If you are going to read about Buiding a Notification System, first understand what a Notification System is.
2. **First try the problem yourself.**
	Before reading the solution, spend 20-30 minutes answering:
	1. What is the problem statement ?
	2. Requirement to solve the problem.
	3. What are the main components ?
	4. What will break at scale ?
	5. How will you scale ?
3. **Read actively with a pencil**
	As you read, stop and ask questions, do research, go through external resources.


4. **Convert every chapter into your own notes :**
	Do not copy the chapter. Write:
	- problem statement
	- requirements
	- core architecture
	- scaling bottlenecks
	- tradeoffs
	- pitfalls


5. **Teach it to someone or at least to yourself**
	System design interviews are communication-heavy.  
	You should be able to explain the design in a clear order:

6. **Reconstruct the design from memory.**  
	A day later, close the book and redraw the architecture.  
	If you cannot explain it without looking, you do not own it yet.



# How to design system in interview ?

 > [!summary] A good opening : 
 > Let me first clarify the functional and non functional requirements, then I’ll estimate the scale, sketch the minimal high-level design, and finally deep dive into the scalability, components and non-functional requirements.





### Step 1 : understand the problem and gather requirements : 3-10 mins

- Functional requirement : 
	- These are the actual features, behaviors, and business actions.
	- Examples:
		- User can sign up and log in
		- User can create a post
		- User can search products
		- System sends OTP for login
		- User can upload an image
		- Admin can delete a spam comment
- Non-functional : 
	- These describe the quality, constraints, and operating expectations of the system. A simple rule: if it talks about **speed, scale, reliability, security, availability, consistency, maintainability, or compliance**, it is usually non-functional.
	- Examples:
		- The system should respond within 200 ms for 95% of requests
		- The system should handle 1 million daily active users
		- The system should be available 99.9% of the time
		- Data should be encrypted in transit and at rest
		- The system should be scalable during traffic spikes
		- Password reset must be secure and abuse-resistant

Example : 
- Functional
	- Customer can browse restaurants
	- Customer can place an order
	- Driver can accept delivery
	- Customer can track order status
	- Payment can be completed online

- Non-functional
	- Search results should appear in under 300 ms
	- Order placement should not lose requests even if a service crashes
	- System should support peak dinner-time traffic
	- Payment data must be secured
	- Notification delivery should be reliable


- **Think deeply and ask questions befor answering** :
	In a system design interview, giving out an answer quickly without thinking gives you no bonus points. Answering without a thorough understanding of the requirements is a huge red flag as the interview is not a trivia contest. There is no right answer.

- When you ask a question, the interviewer either answers your question directly or asks you to make your assumptions. Write down the assumptions, you might need them later.
- What Questions to ask ? 
	- Ask question to understand the exact requirements, like
	- What specific feature we are going to build ?
	- How many users does the product have ?
	- How fast the company expect to scale ? What are the expected scale in 3 months, 6 months and a year ?
- Example : We are building a news feed system : 
	- **You :**  Is this a mobile app? Or a web app? Or both?
	- **Interviewer :** Both

	- **You :** What are the most important feature of the product.
	- **Interviewer :** Ability to make a post and see friends new feed.

	- **You :** Is the news feed sorted in reverse chronological order or a particular order? The particular order means each post is given a different weight. For instance, posts from your close friends are more important than posts from a group.
	- **Interviewer :** To keep things simple, let us assume the feed is sorted by reverse chronological order.

	- **You :** How many friends can a user have ?
	- **Interviewer :** 5000

	- **You :** What is the traffic volume ?
	- **Interviewer :** 10 million daily active users (DAU).

	- **You :** Can feed contain images, videos or just text.
	- **Interviewer :** It contains media files including both images and videos.

> 	Chronological order is the arrangement of events, actions, or information in the sequence they occurred in time, starting from the earliest and moving to the most recent, like a timeline. 
> 	Reverse chronological mean latest first.

It is important to understand the requirements and clarify ambiguities.

### 2. Propose high level design and get buy-in : 10-15 mins

- Come up with the initial blueprint for the design.
	- ask for feedback.
	- Talk with the interviewer and work together like a teammate.
	- Draw key components which may include clients, api, web servers, cache, message queues etc
	- Do estimations.
	- Commuicate with the interviwer if it is going in the right direction.

- Example : News feed system
	- At the high level, the design can be divided into two system : Feed publishing and Feed building
	- Feed Publication :
		- When a user publishes a post, corrosponding data is written in the DB & cache, and the post is populated into your friends feed.
	- Feed Building :
		- The news feed is build from your friends posts in the reverse chronological order.

- Make a figure of both feed publication and building.



### Step 3 : Design deep dive : 10-25 mins

At this step, you and your interviewer should have achieved the following objectives: 
- Agreed on the overall **goal** and the feature **scope**.
- Sketched the HLD for the system.
- Obtained feedback from the interviewer on the HLD blueprint.
- Have some initial ideas on the areas to focus on and dive deeper based on her feedback.

Now you should work with the interviewer to identify and prioritize components in the architecture.
- Sometimes the interviewer like to dive into **high level design**. 
- While sometimes for senior candidates, she want to discuss on the system performance focusing on the bottlenecks and resouce estimation.
In most cases, the interviewer want you to dig deeper into some system component. Start with the most critical component.

**Example :** 
- In URL shortner, it is interesting to dive deeper into the hash function design that converts long url to short one.
- For chat system : how to reduce latency and how to support online/offline status are two interesting topics.

Try not to get into unnecessary details, for example : talking about the EdgeRank algo of facebook feed ranking in details, its not ideal to talk as it need a good amout of time. Just give a brief overview and proceed.

At the point, we have discussed about the high level design of the News Feed System, and the interviewer is happy with your proposal. Now we will investigate two of the most important systems of the News Feed System.
1. Feed Publishing
2. News Feed retrieval

- Dive deeper into both of them one by one.


### Step 4 :  Wrap Up : 2-5 mins

In the final step, the interviewer might ask you some followup question or give up the freedom to discuss on other points.
Here are some direction to follow : 
1. Interviewer ask to find bottleneck and do improvement. Never say your design is perfect, no design is ever perfect. Everything can be improved. Show your critical thinking and leave a good impression.
2. Recap the whole design to the interviewer.
3. Talk about error cases, failures, data loss etc.
4. Opeartional issues like monitoring, error logs, deployment.
5. How you can scale it further from 1 mil to 10 mil.
6. Propose refinements.



# How to built the systems ?


