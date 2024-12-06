#Definitions 

---
## # Responsibilities

- On-Site
	- we have to manage everything 
- IaaS - "Infrastructure as a Service"
	- Cloup provider offers some infrastructure
- PaaS - "Platform as a Service" - Example: Webserver
	- Cloud provider offers a platform
	- we just have to check, that the latest version of our program is deployed
- SaaS - "Software as a Service" - Example: Office365 (we pay for a license & use the applications)
	- Cloud provider offers everything 

![[Pasted image 20241206094937.png]]

---
## # Why?

- Economies of scale
    - Major cloud suppliers can purchase hardware much cheaper than “regular” companies.
    - If you need more computational power or storage you needn’t buy more servers, just buy more services from cloud provider.
    - Ease of scaling up/down based on demand.
    - No need for (traditional) IT personell. Need for DevOps increases though.

---
## # Driving forces

- Pay as you go
    - If you have more customers, you might need more processing power and storage -> pay more
    - If your business is running “slower” (e.g. economic downturn) you might need fewer resources -> pay less
- Low entry costs
    - No long-term financial commitment
    - Individual developers and start-ups can start with nearly no costs.
- High availability
    - Guaranteed by SLAs ("Service Level Agreement")

---

![CloudVendor_MarketShare.png](https://deep-thought.norwin.at/_astro/cloudvendor_marketshare.BpCInJbE_Z1UwJJo.webp)

---

## # Microsoft’s revenue by segment

![MicrosoftRevenueBySegment.png](https://deep-thought.norwin.at/_astro/microsoftrevenuebysegment.DKboHHdP_XELpv.webp)

Quelle: [statista.com](https://www.statista.com/statistics/273482/segment-revenue-of-microsoft/#:~:text=Revenue%20of%20Microsoft%20broken%20down%20by%20segment%202012%2D2023&text=In%20its%202023%20financial%20year,through%20its%20intelligent%20cloud%20segment.)

---
## # SLA

Service Level Agreement
- A service must provide characteristics formally negotiated in a Service Level Agreement (SLA)

---

Test: Maybe SLA selber schreiben?

![SLA_Example_Azure_AppService.png](https://deep-thought.norwin.at/_astro/sla_example_azure_appservice.DxSbyxCn_Z2aNMmd.webp)Azure App Service SLA (as of September 2023)

---
## # Types of resources/services

- We often distinguish between
    - Compute resources
    - Storage resources

---
### # Compute resources

You pay for the time a resource will be available to compute something, e.g.

- App Service (Web Server)
- Azure Functions (Serverless on demand computing)
	- AWS - "Lambda"
- Virtual machines
- Container Registry (for managing docker images)
- Container Instances (For running docker containers)
- …

---

### Storage resources

You pay for a certain amount of storage space, e.g.

- Storage account (OP?)
- Azure Cosmos DB
- Data Lake Storage
- …

---

### Mixed resources

Sometimes it’s both storage and computational power you pay for, e.g. SQL Database

---

## Data centers

[https://datacenters.microsoft.com/globe/explore](https://datacenters.microsoft.com/globe/explore)

---

## Redundancy

For many services you can specify the desired redundancy level:

- Locally redundant storage (LRS)
    - Copies your data three times within a single physical location in the primary region
    - Least expensive
- Zone-redundant storage (ZRS)
    - Copies your data across three Azure availability zones in the primary region.
    - Recommended for applications requiring high availability
- Geo-redundant storage (GRS)
    - Copies your data three times within a single physical location in the primary region using LRS
    - It then copies your data asynchronously to a single physical location in a secondary region that is hundreds of miles away from the primary region
    - Offers durability of at least 99.99999999999999% (16 9’s) = Downtime

---

## Scaling instances

You can scale instances at creation or during usage.

![Scaling_Manually.png](https://deep-thought.norwin.at/_astro/scaling_manually.DlSPYWaI_Ta2h7.webp)

---

## Auto Scaling

There are several options for automatically scaling resources.![AutoScale.png](https://deep-thought.norwin.at/_astro/autoscale.Dig_ZVwt_iHLAh.webp)

---

![AutoScaleConfig1.png](https://deep-thought.norwin.at/_astro/autoscaleconfig1.BeFkuUNX_Z1Wjufd.webp)

---

![AutoScaleConfig2.png](https://deep-thought.norwin.at/_astro/autoscaleconfig2.Dtv3HR6j_1rEkKP.webp)