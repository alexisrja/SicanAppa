# UML de Autenticación (RF-01)

Requerimiento: El sistema permite inicio de sesión con credenciales de personal sindicalizado (trabajador, administrativo, responsable de área), protegiendo información y diferenciando roles. 

## Casos de uso

```mermaid
graph LR
    Trabajador((Trabajador))
    Admin((Personal<br>administrativo))
    Resp((Responsable<br>de área))
    
    subgraph SicanApp
        Login[Iniciar sesión<br>credenciales]
        Auth[Validar credenciales<br>AuthService/API]
        Roles[Obtener rol<br>y permisos]
        Remember[Recordar sesión<br>token en SharedPreferences]
        Logout[Cerrar sesión]
        HomeNav[Navegar a Home<br>por rol]
        
        Login --> Auth
        Auth --> Roles
        Login -.incluye.-> Remember
        Auth --> HomeNav
        Logout -.limpia token.-> Remember
    end
    
    Trabajador --> Login
    Admin --> Login
    Resp --> Login
```

## Secuencia de inicio de sesión

```mermaid
sequenceDiagram
    participant Usuario
    participant UI as SignInScreen<br>(Form + _login)
    participant Form as FormState<br>(validaciones)
    participant Auth as AuthService<br>HTTP/JSON
    participant Store as SessionStore<br>SharedPreferences
    participant Nav as Navigator<br>(/home según rol)
    
    Usuario->>UI: Ingresa credenciales
    UI->>Form: validate()
    Form-->>UI: ok/errores
    
    alt válido
        UI->>Auth: login(userId, password)
        Auth-->>UI: {token, rol} / error
        
        alt éxito
            UI->>Store: save(token, rol, rememberFlag)
            Store-->>UI: ok
            UI->>Nav: pushReplacement(/home[rol])
        else fallo backend
            UI-->>Usuario: Mostrar mensaje
        end
    else inválido
        UI-->>Usuario: Errores en campos
    end
```

## Vista de clases/responsabilidades

```mermaid
classDiagram
    class SignInScreen {
        -GlobalKey~FormState~ _form
        -bool _remember
        +_login()
    }
    
    class AuthService {
        +Future~AuthResult~ login(user, pass)
    }
    
    class AuthResult {
        +String token
        +String role
    }
    
    class SessionStore {
        +saveToken(token)
        +saveRole(role)
        +saveRemember(flag)
        +clear()
    }
    
    class NavigatorRouter {
        +goHomeByRole(role)
    }
    
    SignInScreen --> AuthService : usa
    SignInScreen --> SessionStore : persiste
    SignInScreen --> NavigatorRouter : navega
    AuthService --> AuthResult
```
