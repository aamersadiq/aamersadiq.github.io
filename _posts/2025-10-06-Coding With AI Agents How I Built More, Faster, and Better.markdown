---
title: "Coding With AI Agents: How I Built More, Faster, and Better"
date: 2025-10-06 10:32:45
categories: [dotnet, mongodb, web-api]
tags: [dotnet, mongodb, web-api]
---

In this post, I will outline an application using .NET 8 and MongoDB. The goal was not just to create another demo application, but to explore how these technologies can work together to create a modular system with clean architecture principles, repository patterns, and NoSQL database integration.

<h4>Introduction</h4>

<p>
This blog post explores the development of a system built with .NET 8 and MongoDB. This application provides core functionalities including account creation, money transfers between accounts, deposits, withdrawals, and transaction history tracking.
</p>

<p>
The application demonstrates modern software development practices including:
</p>

<ul>
  <li>Clean architecture with separation of concerns</li>
  <li>Domain-driven design principles</li>
  <li>Repository pattern for data access</li>
  <li>Dependency injection</li>
  <li>Validation using FluentValidation</li>
  <li>Comprehensive unit testing</li>
  <li>API documentation with Swagger/OpenAPI</li>
  <li>NoSQL database integration with MongoDB</li>
</ul>

<p>
Whether you're learning about .NET development, exploring MongoDB integration, or interested in modern application architecture, this project provides valuable insights into building modular and maintainable applications.
</p>

<h4>Technologies Used</h4>

<h5>Core Technologies</h5>

<ul>
  <li><strong><a href="https://dotnet.microsoft.com/download/dotnet/8.0">.NET 8</a></strong>: The latest version of Microsoft's cross-platform development framework</li>
  <li><strong><a href="https://docs.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-8.0">ASP.NET Core Web API</a></strong>: For building RESTful APIs</li>
  <li><strong><a href="https://www.mongodb.com/">MongoDB</a></strong>: A NoSQL document database for flexible data storage</li>
  <li><strong><a href="https://docs.mongodb.com/drivers/csharp/">MongoDB.Driver</a></strong>: The official C# driver for MongoDB</li>
</ul>

<h5>Libraries and Tools</h5>

<ul>
  <li><strong><a href="https://fluentvalidation.net/">FluentValidation</a></strong>: For robust request validation</li>
  <li><strong><a href="https://swagger.io/">Swagger/OpenAPI</a></strong>: For API documentation and testing</li>
  <li><strong><a href="https://xunit.net/">xUnit</a></strong>: For unit testing</li>
  <li><strong><a href="https://github.com/moq/moq4">Moq</a></strong>: For mocking dependencies in tests</li>
  <li><strong><a href="https://www.docker.com/">Docker</a></strong>: For containerization of MongoDB</li>
</ul>

<h4>Architecture Overview</h4>

<p>
The system follows a clean, layered architecture that promotes separation of concerns, testability, and maintainability. The application is divided into four main projects:
</p>

<ol>
  <li><strong>BankAccountManagement.API</strong>: The presentation layer containing controllers, DTOs, and validators</li>
  <li><strong>BankAccountManagement.Core</strong>: The domain layer with business entities, interfaces, and services</li>
  <li><strong>BankAccountManagement.Infrastructure</strong>: The data access layer with MongoDB integration</li>
  <li><strong>BankAccountManagement.Tests</strong>: Unit tests for all components</li>
</ol>

<h5>Architecture Diagram</h5>

<pre>
                      ┌───────────────────────────────────────────────────────────────────────────┐
                      │                         Client Applications                               │
                      └──────────────────────────────────▲────────────────────────────────────────┘
                                                        │
                                                        │
                      ┌──────────────────────────────────▼─────────────────────────────────────────┐
                      │                      BankAccountManagement.API                             │
                      │                                                                            │
                      │    ┌──────────────────┐  ┌─────────────────┐  ┌─────────────────────────┐  │
                      │    │    Controllers   │  │       DTOs      │  │       Validators        │  │
                      │    │                  │  │                 │  │                         │  │
                      │    │   AccountsCtrl   │  │   AccountDto    │  │ CreateAccountValidator  │  │
                      │    │ TransactionsCtrl │  │  TransactionDto │  │ TransferValidator       │  │
                      │    │                  │  │  DepositDto     │  │ DepositValidator        │  │
                      │    │                  │  │  WithdrawDto    │  │ WithdrawValidator       │  │
                      │    └──────────────────┘  └─────────────────┘  └─────────────────────────┘  │
                      └───────────────────────────────────▲────────────────────────────────────────┘
                                                          │
                                                          │
                      ┌───────────────────────────────────▼────────────────────────────────────────┐
                      │                         BankAccountManagement.Core                         │
                      │                                                                            │
                      │    ┌─────────────────┐  ┌───────────────────┐  ┌────────────────────────┐  │
                      │    │     Entities    │  │    Interfaces     │  │        Services        │  │
                      │    │                 │  │                   │  │                        │  │
                      │    │     Account     │  │  IRepository<T>   │  │    AccountService      │  │
                      │    │   Transaction   │  │  IAccountRepo     │  │   TransactionService   │  │
                      │    │TransactionStatus│  │  ITransactionRepo │  │                        │  │
                      │    │                 │  │  IAccountService  │  │                        │  │
                      │    │                 │  │ITransactionService│  │                        │  │
                      │    └─────────────────┘  └───────────────────┘  └────────────────────────┘  │
                      └───────────────────────────────────▲────────────────────────────────────────┘
                                                          │
                                                          │
                      ┌───────────────────────────────────▼────────────────────────────────────────┐
                      │                      BankAccountManagement.Infrastructure                  │
                      │                                                                            │
                      │    ┌────────────────┐   ┌────────────────┐   ┌────────────────────────┐    │
                      │    │  Repositories  │   │  Data Context  │   │      Configuration     │    │
                      │    │                │   │                │   │                        │    │
                      │    │ BaseRepository │   │ MongoDbContext │   │    MongoDbSettings     │    │
                      │    │ AccountRepo    │   │                │   │                        │    │
                      │    │ TransactionRepo│   │                │   │                        │    │
                      │    └────────────────┘   └────────────────┘   └────────────────────────┘    │
                      └─────────────────────────────────▲──────────────────────────────────────────┘
                                                        │
                                                        │
                      ┌─────────────────────────────────▼──────────────────────────────────────────┐
                      │                             MongoDB Database                               │
                      │                                                                            │
                      │         ┌────────────────────────┐      ┌────────────────────────┐         │
                      │         │      Accounts          │      │      Transactions      │         │
                      │         │    Collection          │      │       Collection       │         │
                      │         └────────────────────────┘      └────────────────────────┘         │
                      │                                                                            │
                      └────────────────────────────────────────────────────────────────────────────┘
</pre>

<h5>Domain Models</h5>

<p>
The system revolves around two primary domain entities:
</p>

<h4>Account</h4>
<pre><code class="language-csharp">public class Account
{
    public string Id { get; set; }
    public string AccountName { get; set; }
    public decimal Balance { get; set; }
}</code></pre>

<h4>Transaction</h4>
<pre><code class="language-csharp">public class Transaction
{
    public string Id { get; set; }
    public string FromAccountId { get; set; }
    public string ToAccountId { get; set; }
    public decimal Amount { get; set; }
    public DateTime Timestamp { get; set; }
    public string Description { get; set; }
    public TransactionStatus Status { get; set; }
}

public enum TransactionStatus
{
Pending,
Completed,
Failed
}</code></pre>

<h5>API Endpoints</h5>

<p>
The system exposes the following RESTful API endpoints:
</p>

<h4>Account Endpoints</h4>
<ul>
  <li><code>GET /api/accounts</code> - Get all accounts</li>
  <li><code>GET /api/accounts/{id}</code> - Get account by ID</li>
  <li><code>POST /api/accounts</code> - Create new account</li>
</ul>

<h4>Transaction Endpoints</h4>
<ul>
  <li><code>GET /api/transactions/account/{accountId}</code> - Get transactions for an account</li>
  <li><code>POST /api/transactions/transfer</code> - Transfer money between accounts</li>
  <li><code>POST /api/transactions/account/{accountId}/deposit</code> - Deposit money into an account</li>
  <li><code>POST /api/transactions/account/{accountId}/withdraw</code> - Withdraw money from an account</li>
</ul>

<h4>Key Implementation Features</h4>

<h5>Repository Pattern</h5>

<p>
The system uses the repository pattern to abstract data access operations:
</p>

<pre><code class="language-csharp">public interface IRepository<T> where T : class
{
    Task<IEnumerable<T>> GetAllAsync();
    Task<T> GetByIdAsync(string id);
    Task<T> AddAsync(T entity);
    Task<bool> UpdateAsync(T entity);
    Task<bool> DeleteAsync(string id);
}</code></pre>

<h5>Validation with FluentValidation</h5>

<p>
Request validation is implemented using FluentValidation:
</p>

<pre><code class="language-csharp">public class TransferDtoValidator : AbstractValidator<TransferDto>
{
    public TransferDtoValidator()
    {
        RuleFor(x => x.FromAccountId)
            .NotEmpty().WithMessage("Source account ID is required");

        RuleFor(x => x.ToAccountId)
            .NotEmpty().WithMessage("Destination account ID is required")
            .NotEqual(x => x.FromAccountId).WithMessage("Source and destination accounts must be different");

        RuleFor(x => x.Amount)
            .NotEmpty().WithMessage("Amount is required")
            .GreaterThan(0).WithMessage("Amount must be greater than zero");
    }
}</code></pre>

<h5>Comprehensive Testing</h5>

<p>
The system includes extensive unit tests for all components:
</p>

<pre><code class="language-csharp">[Fact]
public async Task TransferAsync_WithInsufficientBalance_ShouldThrowInvalidOperationException()
{
    // Arrange
    var fromAccountId = "1";
    var toAccountId = "2";
    var amount = 1000m;
    var description = "Test transfer";

    var fromAccount = new Account { Id = fromAccountId, AccountName = "Source Account", Balance = 500 };
    var toAccount = new Account { Id = toAccountId, AccountName = "Destination Account", Balance = 200 };

    _mockAccountRepository.Setup(repo => repo.GetByIdAsync(fromAccountId))
        .ReturnsAsync(fromAccount);
    _mockAccountRepository.Setup(repo => repo.GetByIdAsync(toAccountId))
        .ReturnsAsync(toAccount);

    // Act & Assert
    await Assert.ThrowsAsync<InvalidOperationException>(() => 
        _transactionService.TransferAsync(fromAccountId, toAccountId, amount, description));
}</code></pre>

<h5>MongoDB Integration</h5>

<p>
The system integrates with MongoDB using the official MongoDB.Driver:
</p>

<pre><code class="language-csharp">public class MongoDbContext
{
    private readonly IMongoDatabase _database;

    public MongoDbContext(IOptions<MongoDbSettings> settings)
    {
        var client = new MongoClient(settings.Value.ConnectionString);
        _database = client.GetDatabase(settings.Value.DatabaseName);
    }

    public IMongoCollection<Account> Accounts => _database.GetCollection<Account>("Accounts");
    public IMongoCollection<Transaction> Transactions => _database.GetCollection<Transaction>("Transactions");
}</code></pre>

<h4>GitHub Repository</h4>

<p>
The complete source code for this project is available on GitHub:
<a href="https://github.com/aamersadiq/dotnet-mongodb">https://github.com/aamersadiq/dotnet-mongodb</a>
</p>

<h4>Conclusion</h4>

<p>
This project demonstrates how to build a well-structured application using .NET 8 and MongoDB. By following clean architecture principles and implementing proper separation of concerns, the system is maintainable, testable, and extensible.
</p>

<p>
Key takeaways from this project include:
</p>

<ol>
  <li>Structuring a .NET application with clean architecture</li>
  <li>Implementing the repository pattern with MongoDB</li>
  <li>Using FluentValidation for request validation</li>
  <li>Building comprehensive unit tests</li>
  <li>Documenting APIs with Swagger/OpenAPI</li>
  <li>Implementing proper validation with FluentValidation</li>
</ol>

<p>
Whether you're building a banking system or any application that requires structured data management, the patterns and practices demonstrated in this project provide a solid foundation for your development efforts.
</p>
