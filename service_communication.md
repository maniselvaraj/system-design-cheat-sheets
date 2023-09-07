

# Inter-Service Communication Types


<table>
  <tr>
   <td><strong>Sync</strong>
   </td>
   <td><strong>Async</strong>
   </td>
  </tr>

  <tr>
   
   <td>
        <ul>
        <li>Tight coupling
        <li>Contract between sender/receiver is rigid
        <li>Scalability is coupled (transitive?)
        <li>Need to know the destination (Meh)
        <li>Performance: Weakest link in the chain in baseline
        <li>Failure can cascade
        <li>Easier to debug end to end
        </ul>
   </td>


   <td>
        <ul>
        <li>Loose coupling
        <li>Indepdendant scalability (sort of)
        <li>Any one can consume message from sender
        <li>Flexible contract or no contract between sender and receiver
        <li>Complex setup and so harder to debug
        <li>In general, more failure resilient
        <li>Results in eventually consistent systems
        </ul>
   </td>

  </tr>
</table>

[Main page](README.md) 
