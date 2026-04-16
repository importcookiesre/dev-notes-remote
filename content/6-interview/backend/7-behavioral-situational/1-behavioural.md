

These are the questions that start with phrases like _"Tell me about a time when..."_ or _"Give me an example of..."_. It helps you provide a clear, concise story that proves you have the skills required for the job.

Use ==STAR method== to answer: 
	[[6-interview/index]]
	STAR :
		S = situation : the problem that you are facing
		T = Task : responsibility
		A = Action : What you did
		R = Result
# Framework : **5 backend STAR stories** you can reuse and adapt in interviews.


## 1. Performance issue in an API :

**Question:** Tell me about a time you improved performance.

**Situation:**  
“Our user-facing API started getting slow during peak traffic, and response time increased noticeably.”

**Task:**  
“I was responsible for finding the bottleneck and improving performance without breaking the existing behavior.”

**Action:**  
“I checked logs and metrics, found that the database query was doing a full table scan, and profiled the slow endpoint. I added the missing index, reduced unnecessary DB calls, and cached repeated reads where the data did not change often.”

**Result:**  
“The response time dropped significantly, and the API handled peak traffic much better with lower database load.”


## 2. Bug in auth or token flow

**Question:** Tell me about a production bug you fixed.

**Situation:**  
“Some users were suddenly failing to stay logged in after refresh, and the auth flow was returning 401 errors.”

**Task:**  
“I needed to identify the root cause quickly because it was affecting core user login behavior.”

**Action:**  
“I traced the request flow, checked cookie settings, inspected token validation, and reproduced the issue in staging. I found a mismatch in how the refresh token cookie was being set and fixed the configuration, then verified the flow end to end.”

**Result:**  
“Login failures stopped, the refresh flow became stable, and the issue did not reappear after deployment.”



## 3. Deployment ro release incident : 

**Question:** Tell me about a time something went wrong in deployment.

**Situation:**  
“After a release, one backend service started returning errors for a subset of requests.”

**Task:**  
“I had to identify the issue fast, reduce impact, and restore service safely.”

**Action:**  
“I compared the new release with the previous version, checked logs, and found that a new config value was causing unexpected behavior in one code path. I rolled back the release, fixed the config handling, and tested the change in staging before redeploying.”

**Result:**  
“The service returned to normal quickly, and we avoided prolonged downtime.”



## 4. Database or schema problem

**Question:** Tell me about a time you dealt with a database issue.

**Situation:**  
“As the product grew, one of our database tables became large and certain queries started slowing down.”

**Task:**  
“I needed to improve query performance while keeping the application stable.”

**Action:**  
“I analyzed the slow queries, reviewed the query plan, and found that the filters were not well supported by indexes. I added the right indexes, cleaned up an inefficient query, and validated the change under load before merging it.”

**Result:**  
“Query time improved, the database became less stressed, and the service scaled better without major code changes.”


## 5. working with a teammake or handling ambiguity

**Question:** Tell me about a time you worked with others on a difficult problem.

**Situation:**  
“We had a backend feature with unclear requirements, and the team had different ideas about how to implement it.”

**Task:**  
“I needed to help align the approach and move the feature forward without creating rework.”

**Action:**  
“I broke the problem into smaller parts, clarified edge cases with the product/team, proposed a simpler API contract, and documented the flow before implementation. I also coordinated the backend changes so the frontend and backend stayed aligned.”

**Result:**  
“The team shipped the feature without major back-and-forth, and the final implementation was cleaner and easier to maintain.”




# Questions :

