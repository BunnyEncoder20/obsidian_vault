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
