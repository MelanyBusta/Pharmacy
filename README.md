# MyPharmacy – Segunda entrega (rúbrica de autenticación, usuarios, roles y permisos)

Proyecto ASP.NET Core MVC con .NET 10, SQL Server, Entity Framework Core y ASP.NET Core Identity.

## Requisitos
- .NET SDK 10
- SQL Server (LocalDB, SQL Server Express o instancia local)
- Visual Studio 2026 o VS Code

## Configurar la base de datos
Edita `appsettings.json` y ajusta `ConnectionStrings:DefaultConnection` a tu instancia de SQL Server.

El proyecto usa `EnsureCreated()` para crear el esquema inicial al arrancar. Para un desarrollo continuo y despliegues reales se recomienda sustituirlo por migraciones EF Core.

## Ejecutar
Desde la carpeta del `.csproj`:

```bash
dotnet restore
dotnet run
```

Abre la URL HTTPS que indique la consola.

## Usuario inicial de demostración
- Correo: `admin@mypharmacy.local`
- Contraseña: `Admin12345`

Cambia esta contraseña antes de cualquier despliegue real. El usuario se crea automáticamente al iniciar por primera vez.

## Cómo funciona la autorización
- Identity autentica al usuario y administra la cookie.
- `Permission` contiene el catálogo de permisos.
- `ApplicationRole` se relaciona con permisos mediante `RolePermission`.
- Identity relaciona usuarios y roles mediante `AspNetUserRoles`.
- `[HasPermission("products.create")]` protege cada endpoint.
- `PermissionAuthorizationHandler` consulta la base de datos en cada autorización y solo permite continuar si el usuario activo posee un rol con ese permiso.

## Permisos iniciales
`users.view/create/edit/delete`, `roles.view/create/edit/delete`, `products.view/create/edit/delete`, `branches.view/create/edit/delete` y `suppliers.view/create/edit/delete`.

El rol Administrador recibe todos los permisos iniciales. Los roles Empleado y Cliente se crean sin permisos para que el administrador pueda configurarlos desde la interfaz.

## Correspondencia con la rúbrica
- Inicio y cierre de sesión: `AccountController`.
- Perfil: `ProfileController`.
- Gestión de usuarios y asociación de roles: `UsersController`.
- Gestión de roles y permisos múltiples: `RolesController`.
- Protección backend: atributos `HasPermission` y `PermissionAuthorizationHandler`.

## Notas importantes
1. El catálogo de permisos se carga como datos iniciales. La rúbrica pide asociar y modificar permisos en la creación/edición de roles; eso se hace desde el formulario de Roles.
2. La eliminación de usuarios se implementa como desactivación para conservar integridad e historial.
3. La interfaz oculta o muestra enlaces según conveniencia visual, pero la seguridad real está en los atributos de autorización de los controladores.
4. Antes de entregar, prueba cada endpoint con una cuenta sin permiso y confirma que recibe acceso denegado.
5. Este código es una base académica integral que debe compilarse y probarse en el entorno del equipo, con la conexión real a SQL Server.
