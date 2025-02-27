# DigitalCommerce

Created at: 2025-02-27 14:54:43
Created by: HocKenDy

## Solution Structure

```
DigitalCommerce/
├── src/
│   ├── Gateway/
│   │   └── src/
│   │       └── Gateway/
│   └── [Name]/
│       └── src/
│           ├── Domain/
│           │   └── [Name].Domain/
│           ├── Application/
│           │   └── [Name].Application/
│           ├── Infrastructure/
│           │   └── [Name].Infrastructure/
│           └── Presentation/
│               └── [Name].Presentation/
├── tests/
├── infrastructure/
└── docker/
    └── docker-compose.yml
```

## Services
- Identity
- CRM
- Game
- Loyalty
- CDP

## Project Structure Example
For Identity service:
```
Identity/
└── src/
    ├── Domain/
    │   └── Identity.Domain/
    ├── Application/
    │   └── Identity.Application/
    ├── Infrastructure/
    │   └── Identity.Infrastructure/
    └── Presentation/
        └── Identity.Presentation/
```
