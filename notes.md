
### Slide 1: Intro of Micro-Services
- What is it? 
- Who has used it > two examples, Uber and Airbnb
- SafeTrade as a Micro Service MVP => explain diagram briefly... "The MVP features for safetrade intuitively map over into individual services"... "as per true microservice architecture, each service has it's own datastore"..."this is just a basic C4 diagram of the component level" etc

#### Real world example: Uber
- Converted to Microservice architecture, because two monolithic services had:
	- Availability risks
	- Risky, expensive deployments
	- Poor seperation of concerns
	- Inefficient execution
- Microservices brought; System reliability, Separation of concerns, Clear ownership, Autonomous execution, and Team independence
- TLDR, Uber was originally using two monolithic services but because of issues with this at scale, they opted for microservices architecture (similarly with Airbnb)

-- Also name drop Airbnb

### Slide 2: Micro Services pros n cons
#### notes: 
DevOps => the work involved in deploying and maintaining software in production. 
Distributed transaction problem: 
- In a normal single-DB system, a transaction is simple -- either everything succeeds or everything rolls back automatically.
- In micro-services/service-based, when one operation spans **multiple services with separate db's**, you lose that guarantee, e.g.:
	- user books an item -> listing service marks it unavailable + messaging service create convo + payment service charges user
	- What happens if payment fails after the listing was already marked unavailable? Item is blocked but no payment went through.

Sagas:
- Solves distributed transaction problem. Instead of one atomic reaction, you break it into a sequence of local transactions, each with a corresponding compensating action that undoes it if something later fails.
- complex to implement correctly > more unjustified complexity > service based architecture with shared db side steps this completely.

#### Pros
- Independent services
	- No shared datastore
	- Fault isolation
	- Loose service coupling and independent deployment (this is shared with service based architecture)
	- security, one system is compromised, it can't move to other services
- Scalability
	- Each service can scale independently based on need optimising costs.
- Extensibility
	- A new service can be easily implemented, just make it and hook it up.
- Reliability
	- There is an increased reliability because each service is independent, if one service goes down it will not take down other services
- Security
	- 

#### Cons
- Distributed Data Management (structural problem)
	- Because no service can read another's data. In order to find something, that other service must be called.
- Operational Overhead
	- Increase in DevOps work
		- 10+ independent deployments (service + datastore)
	- Unjustified complexity (MVP isn't at scale for the complexity to be worth it)
	- Service based would achieve similar service independence with ~$\frac{1}{2}$ the deployments needing managing
- Distributed Transactions (operational problem)
	- Saga pattern needed if a transaction fails at any point
		- saga pattern solves distributed transaction problem
		- a way to undo multi service steps in a transaction (think each "action" [step] has an equal and opposite reaction if something fails)
	- The distributed problem surfaces, this is especially prevalent for Safe Trade whose core workflows are almost all will be cross service (rent something > make a payment > update db  => needs undoing  => money back, db row set back)
	- 
- Testing
	- Integration testing is hard, services may need to be mocked or spun up.. which is slow
	- Issues may only appear during scaling in production which is difficult to debug
- Simplicity
	- Complex to understand, is a large mental model with multiple moving parts
	- Tracing an issue across multiple services can be annoying.


### Slide 3: Comparison between architectures
- Mention how Microservices out-performs service based in Scalability, Extensibility, and Reliability (trade off where service based has bottleneck/issue at db)
**BUT**
- Operational complexity (previously mentioned distributed transactions and data management)
- Data management => each service owning it's own db means reads require cross-service calls and writes require careful coordination to avoid inconsistent state.
- Testing 
	- => For a small team, this is a significant overhead 
		- shared db simplifies everything, just check the state rather than inter-services calls
		- contract testing > promising services will return data in specific formats
	- integration testing is exacerbated for microservices because of inter-communication between services and transaction workflows
		- * Safe Trade's core workflows are almost all cross-service*


### Slide 4: Conclusion
- Microservices is a credible, industry-proven architecture — and SafeTrade's feature set maps onto it naturally. But for a six-person team building an MVP, the operational burden, distributed data complexity, and testing overhead outweigh the benefits that service-based already delivers. And critically, service-based doesn't close the door — as SafeTrade grows beyond MVP, decomposing into microservices remains a viable path.
