# MinhaAPI
API em .NET 8 e MySQL

Resumo do projeto

CRUD básico para Usuários, Serviços, Profissionais e Agendamentos.
Soft delete implementado para Usuários, Serviços e Profissionais (IsDeleted).
Separação em camadas: Controllers ? Services ? Repositories ? DbContext (AppDbContext).
Mapeamento EF Core via OnModelCreating para usar nomes de tabelas/colunas em minúsculas (compatível com o schema MySQL fornecido).

Como rodar localmente

Configurar variáveis/connection string
Arquivo appsettings.json contém DefaultConnection. Atualize se necessário:
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost;Database=minhaapidb;User=root;Password=Dev@2025#MySQL;"
}

Subir a aplicação
No terminal do projeto:
$env:ASPNETCORE_ENVIRONMENT="Development"
dotnet run

Swagger (UI) para testar
Em ambiente de desenvolvimento o Swagger é habilitado.
URL (exemplo): https://localhost:5138/swagger (a porta pode variar; verifique saída do dotnet run).

Postman
Coleção Postman disponível em postman/MinhaApi.postman_collection.json no repositório.
Importe a coleção no Postman e altere a variável baseUrl se necessário.

Carregar variáveis de ambiente (.env)
O Program.cs já lê um arquivo .env na raiz do projeto (sem sobrescrever variáveis de ambiente existentes). 
Para usar, crie .env com as variáveis descritas abaixo.

Opções para carregar variáveis (exemplos):
Usar .env (recomendado)
Coloque .env na raiz do projeto (já incluído no .gitignore). 
Depois apenas rode dotnet run — o Program.cs irá carregar automaticamente.

PowerShell (sessão atual)

$env:DB_HOST = "localhost"
$env:DB_NAME = "minhaapidb"
$env:DB_USER = "root"
$env:DB_PASS = "Dev@2025#MySQL"
dotnet run
Bash / Linux / macOS (sessão atual)
export DB_HOST=localhost
export DB_NAME=minhaapidb
export DB_USER=root
export DB_PASS='Dev@2025#MySQL'
dotnet run
Carregar .env em Bash
set -a
source .env
set +a
dotnet run
VSCode (launch.json) — exemplo de configuração de depuração
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": ".NET Core Launch (web)",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/bin/Debug/net8.0/MinhaApi.dll",
      "args": [],
      "envFile": "${workspaceFolder}/.env",
      "cwd": "${workspaceFolder}",
      "stopAtEntry": false
    }
  ]
}

Rotas disponíveis

Usuarios
GET /api/usuarios — lista usuários (não deletados)
GET /api/usuarios/{id} — obter usuário por id
POST /api/usuarios — criar usuário
PUT /api/usuarios — atualizar usuário
DELETE /api/usuarios/{id} — soft delete (IsDeleted = true)

Servicos
GET /api/servicos — lista serviços (não deletados)
GET /api/servicos/{id} — obter serviço por id
POST /api/servicos — criar serviço
PUT /api/servicos — atualizar serviço
DELETE /api/servicos/{id} — soft delete

Profissionais
GET /api/profissionais — lista profissionais (não deletados)
GET /api/profissionais/{id}— obter profissional por id
POST /api/profissionais — criar profissional
PUT /api/profissionais — atualizar profissional
DELETE /api/profissionais/{id} — soft delete

Agendamentos
GET /api/Agendamentos — lista agendamentos
GET /api/Agendamentos/{id} — obter agendamento por id
POST /api/Agendamentos — criar agendamento
PUT /api/Agendamentos — atualizar agendamento
DELETE /api/Agendamentos/{id} — remover agendamento

Observações importantes
IDs: Usuario.Id é Guid (armazenado como binary(16) no MySQL). Servico.Id e Profissional.Id são int.
Antes de criar um agendamento, crie um usuário, serviço e profissional e use os IDs retornados (o serviço valida FKs antes de inserir).
Senhas: atualmente armazenadas em texto. Para produção, implemente hashing (BCrypt/Argon2) e endpoints de autenticação.
SQL schema: se precisar recriar o banco, há um arquivo sql/schema_mysql.sql com o DDL usado originalmente.

Arquitetura e estudo
Pontos para estudar: ASP.NET Core, EF Core, DI, Repository pattern, DTOs, migrations, testes (xUnit), Docker, JWT.

Suporte
Arquivo Postman: postman/MinhaApi.postman_collection.json
Swagger UI: https://localhost:{porta}/swagger (verifique porta após rodar a API)

Boa sorte ao testar!
