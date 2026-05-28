---
tags: [devops, terraform, iac, infraestructura-como-codigo, hashicorp]
unidad: 6
clase: 7
---

# 6️⃣ Terraform — Infraestructura como Código (IaC)

> [!abstract] Idea central
> **Terraform** (de HashiCorp) permite definir la **infraestructura como código** de forma **declarativa**: describís el **estado deseado** y Terraform llama a las **APIs** de los proveedores (cloud, SaaS) para crearlo. Multi-cloud, versionable, con plan previo antes de aplicar.

[[05 - Monitoreo y Observabilidad|← Unidad 5]] · [[00 - Índice DevOps|Índice]] · Siguiente: [[07 - Seguridad (OWASP Top 10)]]

---

## ¿Qué es y para qué sirve?
Herramienta de **IaC** que permite **definir, aprovisionar y gestionar** infraestructuras complejas de forma **declarativa**, usando su propio lenguaje (HCL). Es **multiplataforma** y soporta **múltiples proveedores** de nube. (No es la única IaC: también está el **CDK de AWS**; y se puede complementar con **Ansible**.)

> [!info] Por qué importa para DevOps
> Antes, configurar infraestructura era manual (entrar a la consola de AWS/Azure y crear cada cosa) → sin trazabilidad ni historial. Con Terraform, **toda la infra vive como código en un repositorio** → versionable, auditable, reutilizable y revisable por PR (esto enlaza con **GitOps** de la [[01 - Introducción a DevOps|Unidad 1]]).

> [!example] Caso real de un alumno (networking como código)
> Un compañero usa Terraform en su trabajo (con **GitLab**) para administrar **toda la conectividad**: VPCs, VPNs, *Direct Connects*, endpoints en **AWS, Google y Huawei** (multi-cloud). Tienen organizaciones de **desarrollo** separadas para probar los deploys (rama `dev` → cuenta de Amazon de desarrollo), aunque hay recursos costosos que **solo existen en producción**.

## Cómo funciona — Write / Plan / Apply
1. **Write** — definir los **recursos** (pueden ser multi-cloud). Ej.: una app sobre un servidor, con una VPC + security groups + load balancer.
2. **Plan** (`terraform plan`) — Terraform genera un **plan de ejecución** que describe qué va a **crear, actualizar o destruir**, comparando el estado actual con tu configuración. Sirve para **previsualizar** los cambios antes de aplicarlos.
3. **Apply** (`terraform apply`) — ejecuta los cambios, respetando el **orden de dependencias**.
4. **Destroy** (`terraform destroy`) — elimina toda la infraestructura creada (no es lo usual, pero existe).

---

## Arquitectura / componentes
- **Core** — el **motor** de Terraform (la aplicación en sí; corre localmente).
- **Provider** — **plugin** que interactúa con la API de un cloud/SaaS. Cada provider define **qué recursos** podés configurar y cómo. Tienen **su propio versionado** y los pueden crear HashiCorp, empresas o terceros. Se buscan en el **Terraform Registry** (público), igual que las imágenes en Docker Hub.
- **State** — archivo **JSON** que guarda el **estado** real de la infraestructura. Terraform lo usa para **mapear recursos reales ↔ configuración** e identificar **qué cambios** hacer para llegar al estado deseado. Puede guardarse **localmente** o en un **backend remoto** (para colaborar).
- **Backend** — dónde se almacenan los archivos de estado.

## Bloques del lenguaje
**Resources** — el elemento fundamental: describe uno o más objetos de infraestructura (redes virtuales, servidores, bases de datos, DNS…).
```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```
**Variables** — parametrizan la configuración (más flexible y reutilizable).
```hcl
variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
  default     = "t2.micro"
}
```
**Outputs** — exportan información de los recursos creados.
```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```
**Modules** — agrupan y **encapsulan** recursos/configuraciones para **reutilizar**. Se pueden versionar y compartir (Terraform Registry tiene módulos comunitarios y oficiales). *Ej. del profe:* en una empresa tenían un módulo con la VPC, el usuario del equipo y variables comunes; cualquier recurso nuevo reutilizaba esas definiciones → **estandarización** vía módulos publicados en un registry interno.

## State Management, Workspaces, Security
- **State management:** con el JSON, Terraform sabe qué modificar para alcanzar el estado deseado y lo muestra en el `plan`.
- **Workspaces / Environments:** gestionar **múltiples instancias** de una misma configuración → entornos separados (**dev / testing / prod**), versiones de infra en paralelo, multi-cliente, pruebas de carga sin afectar producción. Evitan conflictos y aíslan recursos.
- **Security & Compliance:** definir políticas de seguridad/compliance **como código**, integrándose con **Sentinel** (HashiCorp) para evaluar y *enforcement* de políticas → automatización segura en entornos regulados.

## Flujo de trabajo típico
`Write` → `terraform init` (inicializa el directorio y **descarga los providers**) → `terraform plan` → `terraform apply` → (eventual `terraform destroy`).

> [!example] Demo en vivo del profe
> Mostró dos ejemplos:
> - **Provider de Render:** API Key + Owner ID (tomados de variables de entorno con prefijo `TF_VAR_`); intentó crear un Web Service pero falló porque Render ya **no permite el plan Free** vía Terraform (pedía tarjeta).
> - **Provider de Docker (local):** `terraform init` descarga el provider y genera el `.terraform.lock.hcl`; en `main.tf` declaró un contenedor **nginx** → `plan` muestra lo que se va a crear → `apply` levanta nginx en el puerto 8000. **Detalle importante:** al **cambiar el nombre** del recurso, el `plan` muestra que Docker **destruye y recrea** la instancia (no todos los atributos se actualizan "in-place"; algunos fuerzan recreación, depende del provider).

> [!note] Laboratorio que quedó pendiente
> El profe quería un laboratorio completo: **Terraform + LocalStack + Kubernetes (minikube)** para simular un **EKS** de AWS y hacer el deploy completo localmente. No pudo ser porque **archivaron el repo open source de LocalStack** justo en marzo. *(Ver [[04 - Deployment]] para K8s.)*

## Beneficios de Terraform
- **IaC** → versionable, reutilizable, documentada.
- **Multi-Cloud** → misma configuración en varias nubes (un "espejo" por nube).
- **Declarativo** → definís el **estado deseado**, no los pasos.
- **Plan de ejecución** → previsualización de cambios.
- **Gráfico de recursos** → gestión automática de **dependencias**.
- **Modularidad** → código reutilizable mediante módulos.
- Hasta puede **estimar el costo** del plan antes de aplicarlo (ej. con Infracost, ver [[03 - CI-CD]]).

> [!summary] Cierre
> Terraform es la pieza de **"Operate"** del ciclo CI/CD: la infraestructura deja de configurarse a mano y pasa a ser **código versionado, planificable y reutilizable**, con el mismo flujo de PRs y ambientes que el resto del software.
