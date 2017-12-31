# ASPDotNETCore-Security

1) SSL (Secured Socket Layer) - using https from http

e.g. implmentation
startup.cs

private readonly IHostingEnvironment _env;

public Startup(IHostingEnvironment env) 
{
	_env = env;
}

public void ConfigureServices(IServiceCollection services)
{
	services.AddMvc();
	
	if (!env.IsDevelopment())
		services.Configure<MvcOptions>(
			o => o.Filters.Add(new RequireHttpsAttribute()));
}

HSTS (Http Strict Transport Security)


2) SQL Injection - injecting a script in SQL query

	e.g
		SELECT * FROM customers WHERE name='{name}'
   
   
	SQL Inject Scenario 1
		' OR '1'='1' --
		SELECT * FROM customers WHERE name='' OR '1'='1' --' (entire customer table will be visible)
 
	SQL Inject Scenario 2
		a'; DROP TABLE customers; --
		SELECT * FROM customers WHERE name='a'; DROP TABLE customers; --' (customer table will be deleted)

	Preventions 
		1) Check the input - characters like quotes and slashes shall be ommitted
		2) Use least privilage account - it possble do not use admin account in application
		3) Use ORM like Entity Framework - ORM is good but if you use ORM with stored procedure ther might be a 
				possibility of SQL Injection.
 
   
4) Cross Site Request Forgery

5) Cross Site Scripting

6) Open Redirection Attacks

7) Click jacking

8) Same Origin Policy
