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
		service.Configure<MvcOptions>(
			o => o.Filters.Add(new RequireHttpsAttribute()));
}

2) SQL Injection

4) Cross Site Request Forgery

5) Cross Site Scripting

6) Open Redirection Attacks

7) Click jacking

8) Same Origin Policy
