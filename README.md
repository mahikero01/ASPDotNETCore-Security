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

	Prevention:
		1) Check the input - characters like quotes and slashes shall be ommitted
		2) Use least privilage account - it possble do not use admin account in application
		3) Use ORM like Entity Framework - ORM is good but if you use ORM with stored procedure ther might be a 
				possibility of SQL Injection.
 
   
4) Cross Site Request Forgery - the attacker takes advantage of the authenticated cookie of a client which is logged on to. 
		Since the said site uses a form to submit data and do transaction the attacker will send a form which is similar from the 
		site the client is currently accessing.
		
	Prevention:
	1) Use an anti forgery token in the form 
		a) use asp-action='add' in form attributes
			<form asp-action='add'> 
		b) put [ValidateAntiForgeryToken] annotation to the action (method) that is  used in submitting the form
			[HttpPost]
			[ValidateAntiForgeryToken] 
			
			
			
5) Cross Site Scripting - Attacker places a JavaScript into webpage. Provides attacker access to everything in the browser.

	Prevention:
	1) Encoding replaces dangerous characters
	2) MVC encodes everything that is in a variable automatically
	3) you can use HTML helper encode to encode input 
	4) Use CSP (Content Security Policy)

	
	
6) Open Redirection Attacks - Attacker tricks you to login to another site which is similar to a bank site you are acesing and gets your
		credentials (phising).
		
		e.g. 
		http://bank.com/Account/LogOn?returnUrl=http://bank.net/Account/LogOn
		As you can see above this link forward you to a Bank.net instead of Bank.com

		Prevention:
		1) Use a checking function to check if the URL is local
		if (!Url.IsLocalUrl(returnUrl))
		{
			//throw
		}
		
		
		
7) Click jacking - Attacker trick the user to click a button but the said button will run a malicious script.

	Prevention:
	1) User is advise for such case for prevention

	
	
	
8) Same Origin Policy - Allows only client with the same domain (differents port is considered different origin even if both run on localhost).
	
	Ways to bypass:
	1) using CORS (Cross Origin Resource Sharing) allow any origin in the Configure Method in Startup Class.
		app.UseCors(c => c.AllowAnyOrigin());
		
	2) Using CORS with policy configured (ConfigureServices method in Startup class)
		ConfigureServices {
			services.AddCors(
				options => 
				{
					option.AddPolicy(
						"AllowBankCom",
						c => c.WithOrigins("https://bank.com")
					);
				}
			);
		}
		
		Use the policy in the annotation of either controller or action
		[EnableCors("AllowBankCom")]
