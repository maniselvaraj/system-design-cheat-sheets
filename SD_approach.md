
# How to approach a System Design Problem?


1. Be curious and question the ask like an end user (or admin)
    1. What is important for the user?
    2. How would the user use the system? What is the workflow?
    3. What is unknown to you (solutions architect)? Ask it out.
2. Question thoroughly to reduce scope.
	1. Do not design a system that susprises the client. They expect a certain solution (esp in an interview)
3. Is this a breadth or depth oriented problem? Design uber vs distributed cache
4. Is this a read or write heavy application?
5. Are there real-time needs? Many applications are real-time now.
6. Derive the high level design. 
    1. As you are doing this, discuss trade-offs. 
    2. Do not assume decisions (even if you think it is trivial) and move forward. For example, in a rate limiter problem, it is common to assume server side rate limit. But ask the interviewer if that is what he/she wants.
    3. Discuss other trade-offs like SQL vs NoSQL etc.
7. Discuss with the interviewer if it is good so far
8. Walk-through the design as if the client is using the system. This will help you catch gaps.
    1. Like resetting the counter in a API rate limiter problem at the end of the time window
    2. How can the client study the effectiveness of the system. Do they need logging and analytics?
    3. Ask the interviewer if anything that you found in this walk-through is important for them.  
9. High level design - Ask interviewer what to focus on
    1. Explore Db schema (depending on the problem)
    2. API design
    3. Address scalability. Always call out trade-offs when you scale.
