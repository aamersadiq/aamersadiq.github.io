---
title: "Building an Application with .NET 8 and MongoDB
date: 2025-07-16
tags: [nodejs, typescript, mongodb, ddd, express, swagger, docker]
tags: [nodejs, typescript, mongodb, ddd, express, swagger, docker]
---

In this blog post, I'll walk through the development of a robust banking application built with Node.js, TypeScript, and MongoDB. This application follows domain-driven design principles and implements a clean, layered architecture to manage bank accounts and transactions.

<h4>Technologies Used</h4>

<h5>Core Technologies</h5>
<ul>
  <li><strong>Node.js</strong>: Server-side JavaScript runtime</li>
  <li><strong>TypeScript</strong>: Adds static typing to JavaScript, enhancing code quality and developer experience</li>
  <li><strong>MongoDB</strong>: NoSQL database for flexible data storage</li>
  <li><strong>Mongoose</strong>: ODM (Object Data Modeling) library for MongoDB and Node.js</li>
  <li><strong>Express</strong>: Web framework for building RESTful APIs</li>
  <li><strong>Swagger/OpenAPI</strong>: API documentation and testing interface</li>
</ul>

<h5>Development Tools</h5>
<ul>
  <li><strong>Docker & Docker Compose</strong>: Containerization for consistent development and deployment</li>
  <li><strong>Jest</strong>: Testing framework for unit and integration tests</li>
  <li><strong>ESLint & Prettier</strong>: Code quality and formatting tools</li>
  <li><strong>Git</strong>: Version control system</li>
</ul>

<h4>Architecture Overview</h4>

<p>The application follows a clean, layered architecture inspired by Domain-Driven Design (DDD) principles:</p>

<pre><code>src/
├── api/                  # API Layer (Controllers, Routes, Middleware)
├── application/          # Application Services Layer
├── domain/               # Domain Layer (Entities, Value Objects, Domain Services)
└── infrastructure/       # Infrastructure Layer (Database, External Services)
</code></pre>

<h5>Architecture Diagram</h5>

<pre><code>
        ┌─────────────────────────────────────────────────────────────────┐
        │                        Client Applications                      │
        └───────────────────────────────┬─────────────────────────────────┘
                                        │
                                        ▼
        ┌─────────────────────────────────────────────────────────────────┐
        │                           API Layer                             │
        │                                                                 │
        │  ┌─────────────┐    ┌──────────────┐    ┌────────────────────┐  │
        │  │  Controllers│    │    Routes    │    │     Middleware     │  │
        │  └─────────────┘    └──────────────┘    └────────────────────┘  │
        └───────────────────────────────┬─────────────────────────────────┘
                                        │
                                        ▼
        ┌─────────────────────────────────────────────────────────────────┐
        │                      Application Layer                          │
        │                                                                 │
        │  ┌─────────────────────────┐    ┌───────────────────────────┐   │
        │  │   Application Services  │    │           DTOs            │   │
        │  └─────────────────────────┘    └───────────────────────────┘   │
        └───────────────────────────────┬─────────────────────────────────┘
                                        │
                                        ▼
        ┌─────────────────────────────────────────────────────────────────┐
        │                         Domain Layer                            │
        │                                                                 │
        │  ┌─────────────┐  ┌─────────────────┐  ┌────────────────────┐   │
        │  │  Entities   │  │  Value Objects  │  │  Domain Services   │   │
        │  │             │  │                 │  │                    │   │
        │  │ -Account    │  │ - Money         │  │ - AccountService   │   │
        │  │ -Transaction│  │                 │  │                    │   │
        │  └─────────────┘  └─────────────────┘  └────────────────────┘   │
        └───────────────────────────────┬─────────────────────────────────┘
                                        │
                                        ▼
        ┌─────────────────────────────────────────────────────────────────┐
        │                     Infrastructure Layer                        │
        │                                                                 │
        │  ┌─────────────┐  ┌─────────────────┐  ┌────────────────────┐   │
        │  │ Repositories│  │ Database Models │  │  External Services │   │
        │  └─────────────┘  └─────────────────┘  └────────────────────┘   │
        └───────────────────────────────┬─────────────────────────────────┘
                                        │
                                        ▼
        ┌─────────────────────────────────────────────────────────────────┐
        │                          MongoDB Database                       │
        └─────────────────────────────────────────────────────────────────┘
</code></pre>

<h5>Domain Layer</h5>

<p>The domain layer contains the core business logic and entities:</p>

<ul>
  <li><strong>Entities</strong>: Account and Transaction classes that encapsulate business rules</li>
  <li><strong>Value Objects</strong>: Money class for handling currency and amounts</li>
  <li><strong>Domain Services</strong>: AccountService for business operations like transfers</li>
</ul>

<h5>Application Layer</h5>

<p>The application layer orchestrates the use cases:</p>

<ul>
  <li><strong>Application Services</strong>: Coordinates domain objects to fulfill use cases</li>
  <li><strong>DTOs</strong>: Data Transfer Objects for input/output transformation</li>
</ul>

<h5>Infrastructure Layer</h5>

<p>The infrastructure layer handles external concerns:</p>

<ul>
  <li><strong>Database</strong>: MongoDB connection and Mongoose schemas</li>
  <li><strong>Repositories</strong>: Data access patterns for domain entities</li>
</ul>

<h5>API Layer</h5>

<p>The API layer exposes the functionality to clients:</p>

<ul>
  <li><strong>Controllers</strong>: Handle HTTP requests and responses</li>
  <li><strong>Routes</strong>: Define API endpoints</li>
  <li><strong>Middleware</strong>: Request validation, error handling, etc.</li>
  <li><strong>Swagger</strong>: API documentation and testing</li>
</ul>

<h4>Key Features</h4>

<h5>Account Management</h5>
<ul>
  <li>Create new bank accounts</li>
  <li>Retrieve account details and balance</li>
  <li>List all accounts</li>
</ul>

<h5>Transaction Processing</h5>
<ul>
  <li>Deposit funds to accounts</li>
  <li>Withdraw funds from accounts</li>
  <li>Transfer funds between accounts</li>
  <li>View transaction history</li>
</ul>

<h5>Data Validation</h5>
<ul>
  <li>Input validation using middleware</li>
  <li>Business rule validation in domain layer</li>
</ul>

<h5>Error Handling</h5>
<ul>
  <li>Centralized error handling</li>
  <li>Meaningful error messages</li>
</ul>

<h4>Implementation Highlights</h4>

<h5>Domain Entities</h5>

<p>The Account entity encapsulates the core business logic for accounts:</p>

{% highlight typescript %}
export class Account {
private \_id: string;
private \_name: string;
private \_balance: Money;

constructor(id: string, name: string, balance: Money) {
this.\_id = id;
this.\_name = name;
this.\_balance = balance;
}

// Business methods
deposit(amount: Money): void {
if (amount.value <= 0) {
throw new Error('Deposit amount must be positive');
}
this.\_balance = this.\_balance.add(amount);
}

withdraw(amount: Money): void {
if (amount.value <= 0) {
throw new Error('Withdrawal amount must be positive');
}
if (this.\_balance.value < amount.value) {
throw new Error('Insufficient funds');
}
this.\_balance = this.\_balance.subtract(amount);
}
}
{% endhighlight %}

<h5>Value Objects</h5>

<p>The Money value object ensures consistent handling of monetary values:</p>

{% highlight typescript %}
export class Money {
private readonly \_value: number;

constructor(value: number) {
this.\_value = Math.round(value \* 100) / 100; // Round to 2 decimal places
}

get value(): number {
return this.\_value;
}

add(money: Money): Money {
return new Money(this.\_value + money.value);
}

subtract(money: Money): Money {
return new Money(this.\_value - money.value);
}
}
{% endhighlight %}

<h5>Domain Services</h5>

<p>The AccountService handles operations involving multiple accounts:</p>

{% highlight typescript %}
export class AccountService {
transfer(fromAccount: Account, toAccount: Account, amount: Money): void {
if (amount.value <= 0) {
throw new Error('Transfer amount must be positive');
}

    fromAccount.withdraw(amount);
    toAccount.deposit(amount);

}
}
{% endhighlight %}

<h5>Repository Pattern</h5>

<p>Repositories abstract the data access logic:</p>

{% highlight typescript %}
export class AccountRepository implements IAccountRepository {
async findById(id: string): Promise<Account | null> {
const accountDoc = await AccountModel.findById(id);
if (!accountDoc) return null;

    return new Account(
      accountDoc._id.toString(),
      accountDoc.name,
      new Money(accountDoc.balance)
    );

}

async save(account: Account): Promise<void> {
await AccountModel.findByIdAndUpdate(
account.id,
{
name: account.name,
balance: account.balance.value
},
{ upsert: true }
);
}
}
{% endhighlight %}

<h5>API Controllers</h5>

<p>Controllers handle HTTP requests and delegate to application services:</p>

{% highlight typescript %}
export class AccountController {
constructor(private accountService: AccountService) {}

async createAccount(req: Request, res: Response): Promise<void> {
const { name } = req.body;
const account = await this.accountService.createAccount(name);
res.status(201).json(account);
}

async getAccount(req: Request, res: Response): Promise<void> {
const { id } = req.params;
const account = await this.accountService.getAccount(id);
if (!account) {
res.status(404).json({ message: 'Account not found' });
return;
}
res.status(200).json(account);
}
}
{% endhighlight %}

<h4>Database Design</h4>

<p>The MongoDB database uses two main collections:</p>

<h5>Accounts Collection</h5>
{% highlight javascript %}
{
  _id: ObjectId,
  name: String,
  balance: Number
}
{% endhighlight %}

<h5>Transactions Collection</h5>
{% highlight javascript %}
{
  _id: ObjectId,
  type: String, // 'DEPOSIT', 'WITHDRAWAL', 'TRANSFER'
  amount: Number,
  fromAccountId: ObjectId, // For transfers
  toAccountId: ObjectId,   // For deposits and transfers
  timestamp: Date
}
{% endhighlight %}

<h4>Docker Setup</h4>

<p>The application uses Docker Compose to run MongoDB and MongoDB Express:</p>

{% highlight yaml %}
version: '3.8'

services:
mongo:
image: mongo:latest
container_name: banking-mongodb
restart: always
ports: - "27017:27017"
environment: - MONGO_INITDB_ROOT_USERNAME=admin - MONGO_INITDB_ROOT_PASSWORD=password
volumes: - mongodb-data:/data/db
networks: - banking-network

mongo-express:
image: mongo-express:latest
container_name: banking-mongo-express
restart: always
ports: - "8081:8081"
depends_on: - mongo
environment: - ME_CONFIG_MONGODB_ADMINUSERNAME=admin - ME_CONFIG_MONGODB_ADMINPASSWORD=password - ME_CONFIG_MONGODB_SERVER=mongo - ME_CONFIG_MONGODB_AUTH_USERNAME=admin - ME_CONFIG_MONGODB_AUTH_PASSWORD=password - ME_CONFIG_BASICAUTH_USERNAME=admin - ME_CONFIG_BASICAUTH_PASSWORD=password
networks: - banking-network

networks:
banking-network:
driver: bridge

volumes:
mongodb-data:
{% endhighlight %}

<h4>API Documentation</h4>

<p>The application uses Swagger UI for API documentation:</p>

{% highlight typescript %}
const swaggerOptions = {
definition: {
openapi: '3.0.0',
info: {
title: 'Banking API',
version: '1.0.0',
description: 'API for banking operations'
},
servers: [
{
url: '/api'
}
]
},
apis: ['./src/api/routes/*.ts']
};
{% endhighlight %}

<h4>Testing Strategy</h4>

<p>The application includes comprehensive tests:</p>

<ul>
  <li><strong>Unit Tests</strong>: Test individual components in isolation</li>
  <li><strong>Integration Tests</strong>: Test interactions between components</li>
  <li><strong>API Tests</strong>: Test the API endpoints</li>
</ul>

<h4>Challenges and Solutions</h4>

<h5>Challenge: UUID Compatibility with MongoDB</h5>
<p>MongoDB's ObjectId doesn't match the UUID format used in the domain layer.</p>

<p><strong>Solution</strong>: Created a custom UUID field in the MongoDB schema and used it as the reference ID for domain entities.</p>

<h5>Challenge: Transaction Consistency</h5>
<p>Ensuring that transfers between accounts are atomic.</p>

<p><strong>Solution</strong>: Used MongoDB transactions to ensure that both the withdrawal and deposit operations succeed or fail together.</p>

<h5>Challenge: Domain Logic vs. Persistence</h5>
<p>Keeping domain logic separate from persistence concerns.</p>
<p><strong>Solution</strong>: Used the repository pattern to abstract data access and implemented mappers to convert between domain entities and database models.</p>

<h4>GitHub Repository</h4>
<p>The complete source code for this project is available on GitHub:
<a href="https://github.com/aamersadiq/node-mongodb" target="_blank">https://github.com/aamersadiq/node-mongodb</a></p>

<h4>Conclusion</h4>

<p>This project showcases how to:</p>
<ul>
  <li>Implement domain driven design in a Node.js application</li>
  <li>Use TypeScript for type safety and better developer experience</li>
  <li>Work with MongoDB and Mongoose for flexible data storage</li>
  <li>Create a RESTful API with Express</li>
  <li>Document APIs with Swagger</li>
  <li>Containerize applications with Docker</li>
  <li>Implement comprehensive testing strategies</li>
</ul>
