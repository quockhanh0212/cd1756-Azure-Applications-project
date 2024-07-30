# Write-up Template

### Analyze, choose, and justify the appropriate resource option for deploying the app.

*For **both** a VM or App Service solution for the CMS app:*
- *Analyze costs, scalability, availability, and workflow*
- *Choose the appropriate solution (VM or App Service) for deploying the app*
- *Justify your choice*

Virtual Machine:
- Costs: fixed monthly operating costs according to the VM size, but including also hidden costs for maintenance of the Operating System
- Scalability: Can scale both vertically and horizontally through the use of a load balancer
- Availability: If multiple are loadbalanced, this is not an issue, otherwise downtime for patching should be taken into account.
- Workflow: Provision virtual machine, log into machine to pull in application files and launch the application. Alternatively a configuration management tool like Chef or Puppet could help to automatically deploy the application to the VM, avoiding the need for manual config.

App Service:
- Costs: Lower costs, can leverage shared infrastructure. No operational costs for OS patching
- Scalability: A bit less than VMs, depending on the App Service Plan SKU (e.g. manual scale to 3 on B1 SKU). Vertical scaling limit lower than a VM.
- Availability: High available, no downtime for patching
- Workflow: CD Integration with Github, Bitbucket, local git for upload of the code. Can leverage staging slots for testing deployments.

In this scenario I would use the App Service.
The application is fairly lightweight, so vertical scaling will not be required. Using an App Service Plan with sufficient horizontal scaling for the envisioned load (which is very low in our case).

### Assess app changes that would change your decision.

If the application would include processing that requires heavy compute resources, such as e.g. PDF rendering from various CMS articles on the server-side. Rather than using a VM for this, we could look into Azure Functions or Azure Batch to spin up on-demand.

An alternative reason could be if the data confidentiality/regulations require dedicated hardware. An App Service Environment (isolated) might be a solution here.

Or a need to run a specific operating system or runtime stack not supported by App Service.