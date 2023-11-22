

# Microservices

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
        <li>Loose coupling paradigm
        <li>Flexible (?)
        <li>Easier to learn the code base
        <li>Parallel development
        <li>Polygot favorable
        <li>Independent deploy and upgrade
        <li>Faster releases
        <li>Easier maintenance
        <li>DB is hidden
        <li>Failure isolation
        <li>Robust and resilient
        </ul>
   </td>


   <td>
        <ul>
        <li>Network is a prime source of issues</li>
        <li>Distributed transaction is harder. Use 2 phased commit or Sagas.</li>
        <li>Distributed data management is hard. May require Scatter/gather data approach</li>
        <li>Potential performance issues</li>
        <li>Harder to debug if distributed tracing is not implemented </li>
        <li>Async communication between microservices causes more issues. See <a href="./service_communication.md">Sync vs Async Comm</a> </li>
        <li>Not ideal for startups</li>
        <li>Microservices modeled around team boundaries has pros and cons. For example, who owns what?</li>
        </ul>
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
