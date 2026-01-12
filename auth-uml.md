# UML de Autenticación (RF-01)

Requerimiento: El sistema permite inicio de sesión con credenciales de personal sindicalizado (trabajador, administrativo, responsable de área), protegiendo información y diferenciando roles.

## Casos de uso
```plantuml
@startuml
left to right direction
actor Trabajador
actor "Personal administrativo" as Admin
actor "Responsable de área" as Resp

rectangle "SicanApp" {
  usecase "Iniciar sesión\n(credenciales)" as Login
  usecase "Validar credenciales\nAuthService/API" as Auth
  usecase "Obtener rol\ny permisos" as Roles
  usecase "Recordar sesión\n(token en SharedPreferences)" as Remember
  usecase "Cerrar sesión" as Logout
  usecase "Navegar a Home\npor rol" as HomeNav

  Login --> Auth
  Auth --> Roles
  Login --> Remember : incluye
  Auth --> HomeNav
  Logout --> Remember : limpia token
}

Trabajador --> Login
Admin --> Login
Resp --> Login
@enduml
```

## Secuencia de inicio de sesión
```plantuml
@startuml
title Flujo de inicio de sesión con backend

actor Usuario
participant "SignInScreen\n(Form + _login)" as UI
participant "FormState\n(validaciones)" as Form
participant "AuthService\nHTTP/JSON" as Auth
participant "SessionStore\nSharedPreferences" as Store
participant "Navigator\n(/home según rol)" as Nav

Usuario -> UI : Ingresa credenciales
UI -> Form : validate()
Form --> UI : ok/errores

alt válido
  UI -> Auth : login(userId, password)
  Auth --> UI : {token, rol} / error
  alt éxito
    UI -> Store : save(token, rol, rememberFlag)
    Store --> UI : ok
    UI -> Nav : pushReplacement(/home[rol])
  else fallo backend
    UI --> Usuario : Mostrar mensaje
  end
else inválido
  UI --> Usuario : Errores en campos
end
@enduml
```

## Vista de clases/responsabilidades
```plantuml
@startuml
title Componentes clave (autenticación)

class SignInScreen {
  - GlobalKey<FormState> _form
  - bool _remember
  + _login()
}

class AuthService {
  + Future<AuthResult> login(user, pass)
}

class AuthResult {
  + String token
  + String role
}

class SessionStore {
  + saveToken(token)
  + saveRole(role)
  + saveRemember(flag)
  + clear()
}

class NavigatorRouter {
  + goHomeByRole(role)
}

SignInScreen --> AuthService : usa
SignInScreen --> SessionStore : persiste
SignInScreen --> NavigatorRouter : navega
AuthService --> AuthResult
@enduml
```
