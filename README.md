
# 🌐 Laboratorio de Sistemas Distribuidos: PokeDex Cloud Deployment & Account Hardening

![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Angular](https://img.shields.io/badge/angular-%23DD0031.svg?style=for-the-badge&logo=angular&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E8.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![Security Headers](https://img.shields.io/badge/Security%20Headers-A%2B-brightgreen?style=for-the-badge)

---

## 📋 Información General

* **Institución:** Fundación Universitaria Tecnológico Comfenalco
* **Estudiante:** Joel Mendoza (`joelmendozaa@tecnocomfenalco.edu.co`)
* **Asignatura:** Sistemas Distribuidos
* **Proyecto:** Despliegue de Pokédex en la Nube y Hardening de Seguridad
* **Plataforma Cloud:** Microsoft Azure (Azure for Students / Azure Starter)
* **Entregable N° 1:** `README.md` — Tutorial paso a paso de creación y configuración de la cuenta en la nube pública.

---

## 🎯 Objetivo de la Guía

El propósito de este documento es proporcionar una guía técnica, detallada y reproducible para la creación, verificación, securización y configuración de gobernanza de una cuenta en **Microsoft Azure**, asegurando el cumplimiento de las mejores prácticas del **Cloud Adoption Framework (CAF)** de Microsoft previo al despliegue de infraestructuras y aplicaciones en producción.

---

## 📑 Tabla de Contenidos
1. [Fase 1: Creación y Activación de la Cuenta en Microsoft Azure](#-fase-1-creación-y-activación-de-la-cuenta-en-microsoft-azure)
2. [Fase 2: Configuración de Seguridad y Gobernanza de la Cuenta](#-fase-2-configuración-de-seguridad-y-gobernanza-de-la-cuenta)
3. [Fase 3: Verificación y Auditoría con Azure Cloud Shell (CLI)](#-fase-3-verificación-y-auditoría-con-azure-cloud-shell-cli)
4. [Fase 4: Estructuración y Convenciones CAF (Cloud Adoption Framework)](#-fase-4-estructuración-y-convenciones-caf-cloud-adoption-framework)
5. [Fase 5: Aprovisionamiento del Grupo de Recursos](#-fase-5-aprovisionamiento-del-grupo-de-recursos)
6. [Resumen de Comandos Ejecutados](#-resumen-de-comandos-ejecutados)

---

## 🚀 Fase 1: Creación y Activación de la Cuenta en Microsoft Azure

Para entornos académicos y profesionales, Microsoft ofrece la suscripción **Azure for Students**, la cual proporciona acceso a servicios gratuitos durante 12 meses y un crédito renovable de \$100 USD sin requerir tarjeta de crédito.

### 1.1 Requisitos Previos
* Una cuenta de correo electrónico institucional universitaria activa (ejemplo: `@universidad.edu`, `@alumnos.edu`, etc.).
* Una cuenta de Microsoft asociada o creada con dicho correo institucional.
* Navegador web moderno en modo normal o incógnito.

### 1.2 Proceso de Registro Paso a Paso
1. Ingresar al portal oficial de estudiantes de Azure: [https://azure.microsoft.com/free/students/](https://azure.microsoft.com/free/students/).
2. Hacer clic en el botón **"Start free"** / **"Empieza gratis"**.
3. Iniciar sesión con las credenciales de la cuenta de correo institucional.
4. **Verificación Académica:**
   * El sistema solicitará autenticar la condición de estudiante mediante el envío de un código de confirmación al correo institucional o a través de la federación de identidades de la universidad.
   * Introducir el código recibido para validar la identidad académica.
5. **Aceptación del Acuerdo de Suscripción:**
   * Completar los datos de perfil de contacto (Nombre, Apellidos, Teléfono de verificación).
   * Marcar la casilla de aceptación del acuerdo de cliente de Microsoft y la declaración de privacidad.
   * Hacer clic en **"Registrarse"** (**"Sign up"**).
6. Tras la validación, el sistema redirigirá automáticamente al **Portal de Administración de Azure**: [https://portal.azure.com](https://portal.azure.com).

```
   ┌────────────────────────────────────────────────────────┐
   │             Registro Azure for Students                │
   │                                                        │
   │  [ Correo Institucional ] ──► [ Verificación de Código ]│
   │                                           │            │
   │                                           ▼            │
   │  [ Portal Azure ] ◄── [ Activación Suscripción ($100) ]│
   └────────────────────────────────────────────────────────┘
```

---

## 🔐 Fase 2: Configuración de Seguridad y Gobernanza de la Cuenta

Antes de desplegar cualquier carga de trabajo, es fundamental aplicar principios de **Hardening** y control financiero sobre la cuenta de Azure.

### 2.1 Autenticación Multifactor (MFA)
1. En el Portal de Azure, hacer clic en el perfil de usuario en la esquina superior derecha y seleccionar **"Ver cuenta"** (**"View account"**).
2. Dirigirse a la sección **Información de seguridad** (**Security info**).
3. Seleccionar **"Agregar método de inicio de sesión"** y elegir **Microsoft Authenticator** o autenticación vía TOTP.
4. Escanear el código QR con el dispositivo móvil y confirmar el código numérico.
5. *Resultado:* La cuenta queda protegida contra ataques de fuerza bruta y robo de credenciales.

### 2.2 Gestión de Acceso Basado en Roles (Azure RBAC)
* La cuenta creada opera como **Propietario** (**Owner**) de la suscripción.
* Siguiendo el principio de mínimo privilegio (*Least Privilege Principle*), cualquier integración automatizada futura (ej. GitHub Actions) debe usar identidades de servicio dedicadas (*Service Principals*) o tokens restringidos (Deployment Tokens de Azure SWA), sin exponer credenciales globales de administración.

### 2.3 Configuración de Alertas de Presupuesto (Cost Management)
1. En la barra de búsqueda superior del Portal de Azure, escribir **"Cost Management + Facturación"**.
2. Seleccionar la suscripción activa (**Azure for Students**).
3. En el menú izquierdo, hacer clic en **"Presupuestos"** (**Budgets**) y luego en **"+ Agregar"**.
4. Configurar:
   * **Nombre:** `budget-pokedex-control`
   * **Periodo de restablecimiento:** Mensual.
   * **Importe:** \$10.00 USD (o el 20% del crédito disponible).
   * **Reglas de alertas:** Notificación al 50%, 80% y 100% al correo electrónico del estudiante.
5. Guardar el presupuesto. Esto garantiza control total sobre consumos anómalos.

---

## 💻 Fase 3: Verificación y Auditoría con Azure Cloud Shell (CLI)

Azure Cloud Shell es un entorno de terminal interactivo basado en navegador que permite ejecutar comandos de Azure CLI (`az`) sin necesidad de instalar herramientas locales adicionales.

### 3.1 Apertura de Cloud Shell
1. En la barra superior del portal de Azure, hacer clic en el ícono de **Cloud Shell** (`>_`).
2. Seleccionar el entorno preferido: **Bash** o **PowerShell** (para este tutorial se emplea Bash / Azure CLI estándar).
3. Si es la primera vez, el sistema creará un recurso de almacenamiento mínimo para persistir el directorio personal (`$HOME`).

### 3.2 Comandos de Verificación y Diagnóstico

#### Paso 1: Listar las suscripciones disponibles y validar estado
```bash
az account list --output table
```
*Salida esperada:*
```text
Name                CloudName    SubscriptionId                        State    IsDefault
------------------  -----------  ------------------------------------  -------  -----------
Azure for Students  AzureCloud   xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  Enabled  True
```
> [!NOTE]
> Se verifica que el estado de la suscripción esté en **`Enabled`** y marcada como **`IsDefault: True`**.

#### Paso 2: Fijar explícitamente la suscripción de trabajo
```bash
az account set --subscription "Azure for Students"
```

#### Paso 3: Inspeccionar los detalles de la cuenta y el Tenant ID
```bash
az account show --output json
```
*Salida representativa:*
```json
{
  "environmentName": "AzureCloud",
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "isDefault": true,
  "name": "Azure for Students",
  "state": "Enabled",
  "tenantId": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
  "user": {
    "name": "estudiante@universidad.edu",
    "type": "user"
  }
}
```

#### Paso 4: Validar las políticas de gobernanza impuestas en la suscripción
```bash
az policy assignment list --output table
```
*Salida representativa:*
```text
Name                                  DisplayName                                  Scope
------------------------------------  -------------------------------------------  ---------------------------------------------
audit-vm-managed-disks                Audit VMs that do not use managed disks      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx
cost-governance-tags                  Enforce required tags on resources           /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx
```
*Diagnóstico:* Permite verificar si la universidad o Microsoft tienen políticas restrictivas sobre ciertas regiones o tamaños de recursos.

#### Paso 5: Validar y Registrar el Proveedor de Recursos para Web Apps
Para poder desplegar Azure Static Web Apps, el proveedor `Microsoft.Web` debe estar registrado en la suscripción.

Verificar el estado del proveedor:
```bash
az provider show --namespace Microsoft.Web --query "registrationState" -o tsv
```

Si el estado retorna `NotRegistered`, se debe ejecutar el registro:
```bash
az provider register --namespace Microsoft.Web
```

Confirmar que todos los proveedores requeridos estén registrados:
```bash
az provider list --query "[?registrationState=='Registered'].namespace" -o table
```

---

## 🏛️ Fase 4: Estructuración y Convenciones CAF (Cloud Adoption Framework)

El **Cloud Adoption Framework (CAF)** de Microsoft establece directrices para organizar los activos en la nube mediante convenciones de nomenclatura (*naming conventions*) y estrategias de etiquetado (*tagging*).

### 4.1 Estándar de Nomenclatura Aplicado
La convención oficial sigue el patrón:
`[tipo-recurso]-[nombre-proyecto]-[entorno]`

| Tipo de Recurso | Prefijo CAF | Proyecto | Entorno | Nombre Resultante |
| :--- | :--- | :--- | :--- | :--- |
| **Resource Group** | `rg-` | `pokedex` | `prod` | `rg-pokedex-prod` |
| **Static Web App** | `swa-` | `pokedex` | `prod` | `swa-pokedex-prod` |

### 4.2 Estrategia de Etiquetado (Resource Tagging)
Todos los recursos desplegados deben contar con metadatos clave para trazabilidad, auditoría y costos:

| Nombre de Etiqueta (Tag) | Valor Ejemplo | Descripción |
| :--- | :--- | :--- |
| `Entorno` | `Produccion` | Ambiente operativo de la carga de trabajo. |
| `Proyecto` | `PokeDex-Cloud` | Nombre del sistema o solución. |
| `Propietario` | `joelmendozaa@tecnocomfenalco.edu.co` | Correo del responsable técnico. |
| `GestionadoPor` | `GitHub-Actions` | Mecanismo de despliegue y orquestación. |
| `Asignatura` | `Sistemas-Distribuidos` | Identificador académico. |

---

## 📦 Fase 5: Aprovisionamiento del Grupo de Recursos

Un **Grupo de Recursos** (`Resource Group`) es un contenedor lógico que alberga los recursos relacionados de una solución.

### 5.1 Selección de Región
Se selecciona una región con alta disponibilidad y baja latencia hacia la ubicación de los usuarios finales, por ejemplo:
* `eastus2` (East US 2) o `centralus` (Central US).

### 5.2 Creación del Grupo de Recursos con Azure CLI
Ejecutar en Azure Cloud Shell:

```bash
az group create \
  --name rg-pokedex-prod \
  --location eastus2 \
  --tags Entorno=Produccion Proyecto=PokeDex-Cloud Asignatura=Sistemas-Distribuidos Propietario=jmendoza5771@tecnocomfenalco.edu.co
```

*Salida esperada:*
```json
{
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/rg-pokedex-prod",
  "location": "eastus2",
  "managedBy": null,
  "name": "rg-pokedex-prod",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": {
    "Asignatura": "Sistemas-Distribuidos",
    "Entorno": "Produccion",
    "Propietario": "estudiante@universidad.edu",
    "Proyecto": "PokeDex-Cloud"
  },
  "type": "Microsoft.Resources/resourceGroups"
}
```

### 5.3 Verificación de la Creación
```bash
az group show --name rg-pokedex-prod --output table
```
*Salida:*
```text
Location    Name             Status
----------  ---------------  ---------
eastus2     rg-pokedex-prod  Succeeded
```

---

## 📝 Resumen de Comandos Ejecutados

| Comando | Propósito |
| :--- | :--- |
| `az account list -o table` | Lista las suscripciones vinculadas a la cuenta. |
| `az account set --subscription "<Name>"` | Selecciona la suscripción de trabajo activa. |
| `az account show -o json` | Muestra el contexto de suscripción y Tenant ID. |
| `az policy assignment list -o table` | Audita políticas corporativas o educativas aplicadas. |
| `az provider register -n Microsoft.Web` | Registra el proveedor para alojamiento de Web Apps. |
| `az group create -n rg-pokedex-prod -l eastus2 --tags ...` | Crea el grupo de recursos bajo CAF con sus etiquetas. |
| `az group show -n rg-pokedex-prod -o table` | Valida el estado `Succeeded` del grupo de recursos. |

---

## ⏭️ Siguientes Pasos

Con la cuenta en la nube completamente configurada, verificada y aprovisionada bajo los estándares de seguridad y Cloud Adoption Framework (CAF), se procede a la fase de integración y despliegue continuo de la aplicación Angular mediante **GitHub Actions** hacia **Azure Static Web Apps**.

Consulte el documento **[Despliegue.md](Despliegue.md)** para la guía paso a paso del pipeline CI/CD y el Hardening de seguridad con calificación **A+**.
