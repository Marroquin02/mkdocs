# Servicios y Providers

Los **Servicios** contienen la lógica de negocio y los **Providers** son inyectados como dependencias.

## Servicios

Un servicio es una claseannotated with `@Injectable()` que puede ser inyectada en otras clases.

### Crear un Servicio

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class UsersService {
  private users: User[] = [];

  create(createUserDto: CreateUserDto): User {
    const user: User = {
      id: Date.now().toString(),
      ...createUserDto,
    };
    this.users.push(user);
    return user;
  }

  findAll(): User[] {
    return this.users;
  }

  findOne(id: string): User | undefined {
    return this.users.find(user => user.id === id);
  }

  update(id: string, updateUserDto: UpdateUserDto): User {
    const index = this.users.findIndex(user => user.id === id);
    if (index !== -1) {
      this.users[index] = { ...this.users[index], ...updateUserDto };
      return this.users[index];
    }
    return undefined;
  }

  remove(id: string): void {
    this.users = this.users.filter(user => user.id !== id);
  }
}
```

## Inyección de Dependencias

NestJS utiliza un **contenedor IoC** (Inversión de Control) para resolver dependencias.

### Inyectar un Servicio en un Controlador

```typescript
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.findAll();
  }
}
```

> **Tip**: Usar `private readonly` en el constructor es una práctica recomendada en TypeScript/Angular.

## Providers Múltiples

Puedes tener múltiples providers del mismo tipo usando **tokens**:

### Usando Tokens de Inyección

```typescript
import { Injectable, Inject } from '@nestjs/common';
import { CONFIG_TOKEN, ConfigService } from './config.service';

@Injectable()
export class DatabaseService {
  constructor(
    @Inject(CONFIG_TOKEN) private configService: ConfigService,
  ) {}
}
```

## Valores Asíncronos

Para providers que necesitan inicialización asíncrona:

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class ConfigService implements OnModuleInit {
  private config: Record<string, string>;

  async onModuleInit() {
    // Simular lectura de configuración
    this.config = await fetchConfigFromFile();
  }

  get(key: string): string {
    return this.config[key];
  }
}
```

## Scopes de Providers

| Scope | Descripción |
|-------|-------------|
| `DEFAULT` | Una sola instancia compartida |
| `REQUEST` | Nueva instancia por request |
| `TRANSIENT` | Nueva instancia por inyección |

```typescript
@Injectable({ scope: Scope.REQUEST })
export class RequestScopedService {}
```

## Exportar Servicios

Para usar un servicio en otro módulo, debe ser exportado:

```typescript
@Module({
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}
```

## Patrón Completo: Módulo de Usuarios

=== "users.module.ts"
    ```typescript
    import { Module } from '@nestjs/common';
    import { UsersController } from './users.controller';
    import { UsersService } from './users.service';

    @Module({
      controllers: [UsersController],
      providers: [UsersService],
      exports: [UsersService],
    })
    export class UsersModule {}
    ```

=== "users.service.ts"
    ```typescript
    import { Injectable } from '@nestjs/common';

    @Injectable()
    export class UsersService {
      private users: User[] = [];

      findAll(): User[] {
        return this.users;
      }

      create(data: CreateUserDto): User {
        const user = { id: Date.now().toString(), ...data };
        this.users.push(user);
        return user;
      }
    }
    ```

=== "users.controller.ts"
    ```typescript
    import { Controller, Get, Post, Body } from '@nestjs/common';

    @Controller('users')
    export class UsersController {
      constructor(private readonly usersService: UsersService) {}

      @Get()
      findAll() {
        return this.usersService.findAll();
      }

      @Post()
      create(@Body() createUserDto: CreateUserDto) {
        return this.usersService.create(createUserDto);
      }
    }
    ```

## Volver al Inicio

- [Volver a Inicio](index.md)
- [Instalación](instalacion.md)
- [Módulos y Controladores](modulos-controladores.md)
