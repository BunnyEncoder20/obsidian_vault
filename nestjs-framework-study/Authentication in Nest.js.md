# Token/Bearer based

## 1. Installing packages
I installed the bellow packages:
```bash
npm i @nestjs/jwt @nestjs/passport passport passport-jwt passport-local
npm i @nestjs/config class-validator class-transformer
```

## 2. Making the Authentication Module
always better to use the nest cli for making modules, controllers and services:
```bash
nest g module auth
nest g controller auth
nest g service auth
```

## 3. Making the Controller
- This is where the signup and signin requests will be incoming.
- I've kept the User Module to only handle the things related to user (like making a new user).
- The auth.controllers and service are responsible to for getting the correct data fields to the user.services
- Hence here is how the auth.controller.ts should look like:
```ts
@Controller('auth-v2')
export class PassportAuthController {
  constructor(private authService: AuthService, private jwtService: JwtService) {}

  @HttpCode(HttpStatus.OK)
  @Post('signup')
  async signUp(
    @Body() passportSignUpDto: PassportSignUpDto, 
    @Res({ passthrough: true }) response: Response 
  ) {
    const results = await this.authService.signUp(passportSignUpDto);

    // Set tokens as HttpOnly cookies'
    this.setAuthCookies(response, results.access_token, results.refresh_token);

    // Return user info without the tokens (They'll be in cookies now)
    return {
      id: results.id,
      email: results.email,
      message: 'User registration successful',
    }
  }
}
```
- And the auth.service.ts
```ts

```