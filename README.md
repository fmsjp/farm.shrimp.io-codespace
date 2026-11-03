# farm.shrimp.io-codespace

## Run in GitHub Codespaces
The repository runs entirely in a Codespace (private ports; Docker is used only for SQL Server). Nothing needs installing on your own computer.

1. **Add two Codespaces secrets** (GitHub, Settings, Codespaces, Secrets, allow this repository):
   - `SQL_SA_PASSWORD`: the SQL Server `sa` password (8+ characters with upper case, lower case and a digit).
   - `ADMIN_INITIAL_PASSWORD`: first password of the `admin` System Administrator (changed at first sign-in; same password rules). If a secret is missing, the start script generates a value and keeps it in `.devcontainer/local.env` (git-ignored).
2. **Create the Codespace** from the `main` branch (4 cores, 8 GB, 32 GB). On creation it installs .NET 9, Node 22, `dotnet-ef` and the Angular packages. On every start, `.devcontainer/start-fpms.sh`:
   - starts SQL Server in Docker;
   - generates a brand-new `Initial` migration from the current model (the `Migrations` folder is git-ignored);
   - runs the Migrator, which creates the database, turns on read-committed snapshot and seeds reference data;
   - writes `angular/src/assets/appconfig.json` for the Codespace URLs, then starts the API on port 44311 and the Angular dev server on port 4200 (it proxies `/api`, `/AbpUserConfiguration` and `/signalr`).
3. **Open the ports** from the Ports tab: *Angular app* (4200) is the app; the API (44311) serves Swagger at `/swagger` and health at `/health`.
4. **Sign in:**
   - **System Administrator:** leave the farm code blank, user `admin`, password = `ADMIN_INITIAL_PASSWORD`.
   - **Farm users:** register a farm.
   
| Database | When | Content |
| :---- | :---- | :---- |
| `FPMS` | `bash .devcontainer/start-fpms.sh` | the `admin` account and reference data only. |

