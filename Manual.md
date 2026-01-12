@startuml
left to right direction
skinparam packageStyle rectangle

actor "Trabajador sindicalizado" as Trab
actor "Depto. Local de Trabajo" as DLT
actor "Administrador" as Admin

rectangle "App Sindical Sección 243" {

  (Iniciar sesión) as UC_Login
  (Cerrar sesión) as UC_Logout

  (Consultar comunicados) as UC_Com_List
  (Ver detalle de comunicado) as UC_Com_Det
  (Buscar/filtrar comunicados) as UC_Com_Filter

  (Registrar trámite) as UC_Tra_Reg
  (Consultar historial de trámites) as UC_Tra_List
  (Ver detalle de trámite) as UC_Tra_Det
  (Consultar estatus de trámite) as UC_Tra_Status

  (Ver credencial digital) as UC_Cred_View

  (Consultar áreas a cargo) as UC_Areas_List
  (Ver detalle de área/responsable) as UC_Areas_Det

  (Revisar trámites recibidos) as UC_DLT_Review
  (Actualizar estatus de trámite) as UC_DLT_Update
  (Agregar observaciones al trámite) as UC_DLT_Obs

  (Publicar comunicado) as UC_Admin_Com_Add
  (Editar/Eliminar comunicado) as UC_Admin_Com_Edit
  (Administrar áreas a cargo) as UC_Admin_Areas
  (Administrar usuarios y roles) as UC_Admin_Users
}

Trab --> UC_Login
Trab --> UC_Logout

Trab --> UC_Com_List
Trab --> UC_Com_Det
Trab --> UC_Com_Filter

Trab --> UC_Tra_Reg
Trab --> UC_Tra_List
Trab --> UC_Tra_Det
Trab --> UC_Tra_Status

Trab --> UC_Cred_View

Trab --> UC_Areas_List
Trab --> UC_Areas_Det

DLT --> UC_Login
DLT --> UC_DLT_Review
DLT --> UC_DLT_Update
DLT --> UC_DLT_Obs

Admin --> UC_Login
Admin --> UC_Admin_Com_Add
Admin --> UC_Admin_Com_Edit
Admin --> UC_Admin_Areas
Admin --> UC_Admin_Users

UC_Com_Det .> UC_Com_List : <<include>>
UC_Tra_Det .> UC_Tra_List : <<include>>
UC_Tra_Status .> UC_Tra_Det : <<include>>
UC_DLT_Update .> UC_DLT_Review : <<include>>
UC_DLT_Obs .> UC_DLT_Review : <<include>>

@enduml
