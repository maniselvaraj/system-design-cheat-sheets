

# Microservices

<table>
  <tr>
  <td>Context</td>
   <td><strong>Pros</strong></td>
   <td><strong>Cons</strong></td>
  </tr>


  <tr>  
  <td>Design</td>
  <td>
     * Loose coupling paradigm. <br/>
     * Flexible (?) <br/>
     * DB is hidden. 
  </td>
  <td>
     * Not ideal for startups. <br/>
     * Microservices modeled around team boundaries has pros and cons. For example, who owns what? <br/>
     * Deciding when to adopt the microservice architecture is difficult.
  </td>
  </tr>

  <tr>  
  <td>Development</td>
  <td>
     * Easier to learn the code base. <br/>
     * Parallel development.<br/>
     * Polyglot favorable.<br/>
     enables teams to be autonomous.
  </td>
  <td>
  </td>
  </tr>


  <tr>  
  <td>Testing/Release</td>
  <td>
     * Faster releases
  </td>
  <td>* Distributed systems are complex, which makes development, testing, and deployment difficult.
  </td>
  </tr>

  <tr>  
  <td>Maintenance</td>
  <td>
     * Easier maintenance. <br/>
     * Services are small and easily maintained.
  </td>
     <td>
     * Harder to debug if distributed tracing is not implemented <br/>
     * Network is a prime source of issues<br/>
     * Async communication between microservices causes more issues. See Sync vs Async Comm <br/> 
     </td>
  </tr>

  <tr>  
  <td>Deployment</td>
  <td>
     Services are independently deployable. <br/>
     It enables the continuous delivery and deployment of large, complex applications.
  </td>
     <td>
     </td>
  </tr>

  <tr>  
  <td>Functionality</td>
  <td>
     It allows easy experimenting and adoption of new technologies.
  </td>
     <td>
     * Distributed transaction is harder. Use 2 phased commit or Sagas. <br/>
     * Distributed data management is hard. May require Scatter/gather data approach. <br/>
     * Finding the right set of services is challenging. <br/>
     * Deploying features that span multiple services requires careful coordination.
     </td>
  </tr>

  <tr>  
  <td>Scalability, Fault Tolerance, Performance</td>
  <td>
     Better Failure/Fault isolation. </br>
     Robust and resilient. </br>
     Services are independently scalable.
  </td>
     <td>
     Potential performance issues.
     </td>
  </tr>
</table>




# Monolith

<table>
  <tr>
   <td><strong>Pros</strong>
   </td>
   <td><strong>Cons</strong>
   </td>
  </tr>

  <tr>
   
   <td>
        <ul>
        <li>Tight coupling (having everything in one place is good too)
        <li>Simple to deploy (may be)
        <li>Simpler code reuse
        <li>No network issues
        <li>Better control over transactions
        </ul>
   </td>


   <td>
        <ul>
        <li>Tight coupling
        <li>Intermingled business logic (if code grows over years)
        <li>Delivery contention. Small change requires extensive testing
        <li>Ownership conflicts
        </ul>
   </td>

  </tr>
</table>

[Main page](README.md) 
