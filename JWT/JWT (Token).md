Click here to see [[Why JWT (Token)]]

NOTE: **Token is set in `API` level**

#### Create and Use Token
- [ ] **Command Palette** => **Nuget Gallery** => Install `Microsoft.AspNetCore.Authentication.JwtBearer`

- [ ] In `Program.cs` after `#region Cors: baraye ta'eede Angular HttpClient requests`
```C#
#region Authentication & Authorization
string tokenValue = builder.Configuration["TokenKey"]!;

if (!string.IsNullOrEmpty(tokenValue))
{
    builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuerSigningKey = true,
                IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(tokenValue)),
                ValidateIssuer = false,
                ValidateAudience = false
            };
        });
}
#endregion Authentication & Authorization
```

- [ ] In `Program.cs` add `app.UseAuthentication();` between `Cors` and `Authorization`:
```C#
app.UseCors(); // this line is added

app.UseAuthentication(); // this line has to be between Cors and Authorization!

app.UseAuthorization();
```

- [ ] Place these files in `Interfaces` and `Services` folders. [[TokenService.zip]] 

- [ ] Open `appsettings.Development.json` and add this `TokenKey` like below:
	NOTE: 
	* This is for **Development** only so we can share it with github. 
```js
{
  "MongoDbSettings": {
    "ConnectionString": "mongodb://localhost:27017",
    "DatabaseName": "match-finder"
  },
  "TokenKey": "BX4RJbtQc*qBWqag Random-notsecure This should be at least 512 bytes",
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Information"
    }
  }
}
```

- [ ] Open `appsettings.json` and add this `TokenKey` like below:
	NOTE: 
	* This is for **Production** only so we can NOT share it with github. It's secure. 
	* Later we add our actual secure key instead of `null`
```js
{
  "MongoDbSettings": {
    "ConnectionString": "mongodb://localhost:27017",
    "DatabaseName": "match-finder"
  },
  "TokenKey": null,
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Information"
    }
  }
}
```

- [ ] `Program.cs` => Add this for dependency injection:
```c#
builder.Services.AddScoped<ITokenService, TokenService>();
```

- [ ] Update your `LoggedInDto` with Token like this:

- [ ] Create a new record called `LoggedInDto`. e.g. in AccountDto
```c#
public record LoggedInDto(
    string Id,
    string Email,
    string Token
);
```

- [ ] Inject `ITokenService` in any Repository that needs it. e.g. AccountRepository

- [ ] Update your returns like this:
```c#
LoggedInDto loggedInDto = new LoggedInDto(
	Id: appUser.Id,
	Token: _tokenService.CreateToken(appUser),
	Email: appUser.Email // amir@gmail.com
);
```

- [ ] Change your methods' return types appropriately. 

- [ ] Register or login with Postman and check the postman's received token with [https://jwt.io](https://jwt.io/) OR [https://jwt.ms/](https://jwt.ms/)

- [ ] Add `[Authorize]` Or `[AllowAnonymous]` to your Controllers' class or end-points/methods
	
NOTE: Any time you login, you get a NEW key!

Back to [[0 - Project Steps]]